---
paginate: true
theme: custom-theme
size: 16:9
title: Seeding Data In Ruby On Rails
_class: prose
---
<!-- _class: lead -->

# Seeding Data In Ruby On Rails

Rails seeds are very powerful

---
<!-- _class: lead -->

# What We'll Cover

- What Are Seeds?
- Horror Stories On Why Seeds Are Useful
- Approaches To Seeding
- Testing Seeds

---
<!-- _class: lead -->
<!--
Please remember to Like/Comment/Subscribe!
-->

# Before I Start

<div style="display: flex; justify-content: space-around; align-items: center; font-size: 1.2rem; margin-top: 10rem; margin-bottom: 5rem;">
  <img src="/assets/images/youtube-like.svg" height="100" class="wiggle" />
  <img src="/assets/images/youtube-subscribe.png" height="100" class="wiggle" />
</div>

---
<!-- _class: lead -->
<!--
Could be a user, could be Tax Rates or chucks of fixed data.
-->

# What Are Seeds?

They're bits of data we use to pre-populate our database.

They could be anything from a User ever developer will use for local development, to chunks of data used for in all environments.

---
<!-- _class: lead -->
<!--
-->

# What Are Seeds?

In Ruby On Rails, we often use the `rails db:seed` command and you should be able to run it multiple times without fear of it going wrong.

Personally I just run `bin/setup` & expect it to do everything I need.

---
<!--
-->

# What Are Seeds?

Ruby on Rails stores them in the `db/seeds.rb` file.

```bash {6}
▸ app/
▸ bin/
▸ config/
▾ db/
    schema.rb
    seeds.rb
```


---
<!--
The default sample sucks so hard.

It can't comfortably be run multiple times & doesn't allow for if the data has changed in the DB since seeding.
-->

# What Are Seeds?

The out of the box the seeds file looks like:

```ruby
# This file should contain all the record creation needed to seed the database with its default values.
# The data can then be loaded with the rails db:seed command (or created alongside the database with db:setup).
#
# Examples:
#
#   movies = Movie.create([{ name: 'Star Wars' }, { name: 'Lord of the Rings' }])
#   Character.create(name: 'Luke', movie: movies.first)
```
---
<!--
If I'm lucky they look like this
-->

# What Are Seeds?

If I'm lucky, I'd pick up a project where I can safely rerun the seeds:

```ruby
# db/seeds.rb

User.find_or_create_by!(email: 'admin@example.com') do |user|
  user.password = "12345678"
  user.password_confirmation = "12345678"
end if Rails.env.development?
```

---
<!--
Maybe you'll also get this, or just a blank file.

They we're made years ago & were to much of a hassle to keep up to date.
-->

# What Are Seeds?

Sometimes they'd look a bit more like:

```ruby
# db/seeds.rb

# Last updated 10 years ago - DON'T USE RUN IN PRODUCTION
TaxRate.find_or_create_by!(origin: 'NL', destination: 'GB') do |tax_rate|
  tax_rate.amount = "1.10"
end
```

# <center>🤔</center>

---
<!-- _class: lead -->
<!--
First story: Dev given production copy of production DB. Laptop was stolen.
-->

# Horror Stories On Why Seeds Are Useful

The stolen laptop

---
<!-- _class: lead -->
<!--
Second story: Preview Environment used real data. It then emailed all the users.
-->

# Horror Stories On Why Seeds are Useful

Preview Environment started emailing real users

---
<!-- _class: lead -->
<!--
Last Story: Don't make devs jump through hoops to have a decent environment.
-->

# Horror Stories On Why Seeds are Useful

The poor initial developer experience

---
<!--
-->

# Approaches To Seeding

- Explicitly Defined Seeds
- Faker Generated Seeds
- Fixtures & Factories Generated Seeds
- Anonymised Production Database
- Plain Old Ruby Objects

---
<!--
-->

# Explicitly Defined Seeds

These are the ones you'd find in you `db/seeds.rb` file.

The files can become very big pretty fast. I've seen them broken up into smaller files before:

```ruby
# db/seeds.rb
# Load all the files in db/seeds folder
Dir[File.join(Rails.root, 'db', 'seeds', '*.rb')].sort.each do |seed|
  load seed
end
```

---
<!--
Though hasn't been updated in a while
-->

# Explicitly Defined Seeds

https://github.com/james2m/seedbank

Gives you more fine grain control of the order your seeds are run:

```ruby
# db/seeds/companies.seeds.rb
Company.find_or_create_by_name('Hatch')
```

```ruby
# db/seeds/projects.seeds.rb
after :companies do
  company = Company.find_by_name('Hatch')
  company.projects.create(title: 'Seedbank')
end
```

---
<!--
-->

# Faker Generated Seeds

https://github.com/faker-ruby/faker

```ruby
require 'faker'

Faker::Name.name      #=> "Christophe Bartell"

Faker::Internet.email #=> "kirsten.greenholt@corkeryfisher.info"
```

---
<!--
I like this, especially in development environments
-->

# Faker Generated Seeds

```ruby
# db/seeds.rb

require 'faker'

User.find_or_create_by!(email: 'admin@example.com') do |user|
  user.name = Faker::Name.name
  user.password = "12345678"
  user.password_confirmation = "12345678"

  user.posts << Post.new(title: Faker::Job.title)
  user.posts << Post.new(title: Faker::Job.title)
  user.posts << Post.new(title: Faker::Job.title)
end if ENV['DURING_RELEASE_SEED_USER'] || Rails.env.development?
```

---
<!-- footer: https://thoughtbot.com/blog/factory_girl-for-seed-data -->
<!--
Don't use this approach. You might have problems with foreign keys
-->

# Fixtures & Factories Generated Seeds

There is a command `rails db:fixtures:load FIXTURES=users,posts`, it will load fixtures into your current environment.

You could also loop through your Factories, but ThoughtBot doesn't recommend doing it (it has a lot of short comings).

_I've not seen this approach used in the wild._

---
<!-- footer: "" -->
<!--
-->

# Anonymised Production Database

https://github.com/evilmartians/evil-seed

```
require 'evil_seed'
EvilSeed.configure do |config|
  config.root('User', 'created_at > ?', Time.current.beginning_of_day)

  config.anonymize("User")
    name  { Faker::Name.name }
    email { Faker::Internet.email }
  end
end 

EvilSeed.dump('path/to/new_dump.sql')
```

---
<!--
What if you don't put it in the database at all?
-->

# Plain Old Ruby Objects


```ruby
Role = Struct.new(:id, :display_adverts, keyword_init: true) do
  def self.find(id)
    all.find { |plan| plan.id == id } || all.first
  end

  def self.all
    @all ||= [
      Role.new(id: 'free', display_adverts: true),
      Role.new(id: 'premium', display_adverts: false)
    ]
  end

  alias_method :display_adverts?, :display_adverts
end
```

---
<!--
This is fine & my favourite thing to do.
-->

# Plain Old Ruby Objects


```ruby
class User < ApplicationRecord
  def plan
    @plan ||= Plan.find(plan_id)
  end
end

User.new(plan_id: 'premium').plan.display_adverts?
# Outputs: false
```

---
<!--
This is one of my favourite bits of code.
-->

# Testing Seeds

Yes you can!

```ruby
# spec/db/seeds_spec.rb
RSpec.describe 'Rails.application' do
  describe '#load_seed' do
    subject { Rails.application.load_seed }

    it do
      expect { subject }.to change(User, :count).by(1)
        .and change(Project, :count).by(1)
    end
  end
end
```

---
<!--
-->

# What is the best way?

- You should be able to run `rails db:seed` multiple times without fear.
- Use them in a preview environment! You'll be more incentivised to keep them up to date.
- Plain Old Ruby Objects for data that needs to be consistent across all environments.
- Use partial dumps of production to investigate exceptions more closely.

---
<!-- _class: lead -->


# Questions?

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
