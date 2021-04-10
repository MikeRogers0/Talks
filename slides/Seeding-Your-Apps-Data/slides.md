---
paginate: true
theme: custom-theme
size: 16:9
title: Seeding Data In Ruby On Rails
_class: prose
---
<!-- _class: lead -->
<!--
Hello Everyone!

I'm going to be seeding in rails, it's something which we all probably know about
but most the projects we're using probably are have empty files.
-->

# Seeding Data In Ruby On Rails

A good `db/seeds.rb` file, makes every app amazing

---
<!-- _class: lead -->
<!--
This is what I'm going to be covering in this.

The idea is by the end of it, you'll want to take any existing/new project and just add a few seeds.

Plus I have some cool tricks to get a lot of value from your seed data.
-->

# What We'll Cover

- What Are Seeds?
- We'll check the vibe of the room
- Horror Stories On Why Seeds Are Useful
- Approaches To Seeding
- Testing Seeds


---
<!-- _class: lead -->
<!--
Could be a user, could be Tax Rates or chucks of fixed data.

The idea is that is development we have a good representation of production so we can build a good UX.
And in production we have any data that is good to be available.
-->

# What Are Seeds?

They're bits of data we use to pre-populate our database.

They could be anything from a User ever developer will use for local development, to chunks of data used for in all environments.

---
<!-- _class: lead -->
<!--
Rails has some built in commands to make this easier for us, and also their is the awesome bin/setup fil.e
-->

# What Are Seeds?

In Ruby On Rails, we often use the `rails db:seed` command and you should be able to run it multiple times without fear of it going wrong.

Personally I just run `bin/setup` & expect it to do everything I need.

---
<!--
They also live here:
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
The default file sucks so hard. I hate it.

I don't think you'd ever want your seeds to look like this at all.
-->

# What Are Seeds?

The out of the box the seeds file looks like:

```ruby {0}
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
If I'm lucky they look like this. Where there is actually something a bit more useful going on.
-->

# What Are Seeds?

If I'm lucky, I'd pick up a project where I can safely rerun the seeds:

```ruby {0}
# db/seeds.rb

User.find_or_create_by!(email: 'admin@example.com') do |user|
  user.password = "12345678"
  user.password_confirmation = "12345678"
end if Rails.env.development?
```

---
<!--
Maybe you'll also get this, or just a blank file.

You might also get something a bit suspect. This is taken from a real project I worked on.

I wish I had written that comment, but I didn't. In the production DB that value is different now.

They we're made years ago & were to much of a hassle to keep up to date.
-->

# What Are Seeds?

Sometimes they'd look a bit more like:

```ruby {0}
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
Before we continue! Let's check the vibes of the room!

Throw up some emojis!

Who has picked up a project where it was just blank? I feel it's pretty common.
-->

# Who has worked on a project there were no seeds at all?

👍

---
<!-- _class: lead -->
<!--
I used to work in agencies quite a lot & it was pretty common to just take a copy of the production DB.
-->

# Who has worked on a project where they were sent a database dump to develop locally?

👍

---
<!-- _class: lead -->
<!--
I don't think I've ever worked on a project which was amazing.

I'm going to show you a very cool trick I've started doing which I think is the way to go.
-->

# Who has worked on a project they felt the seeds were well maintained?

👍

---
<!-- _class: lead -->
<!--
First story:

This was a few years ago now, but a developer was sent a copy of the production database to use locally.

Then someone broke into their office on a weekend (Smashed the door in) and stole their laptop.

They had to tell the customer about it (Code + DB lost) & it was very uncomfortable.

As we all work from home, this one is becoming more important to think about.

Aside:
I heard that Facebook the developers dev environment is in a cloud machine the SSH into.
I'm super excited about GitHub Codespaces for not having the codebase/DB on local machines, but instead of cloud machine which is destroyed at the end of each session.
-->

# Horror Stories On Why Seeds Are Useful

The Stolen Laptop

---
<!-- _class: lead -->
<!--
Second story:
I worked on an application where a series of failures happen.

It was a project where it would send users email of what they need to look at that day via a cronjob;

- The preview environments ran the cronjobs same as production
- The dataset was an anonymised snapshot of production, often a few months behind (It would take a while to run & was very manual).
- The preview environment used MailTrap to stop emails going out. But one day our client wanted to see the emails in their inbox. So some emails weren't anonymised in the snapshot.

Then one day the client said "I want an exact copy of production in staging".
We found out something went horribly wrong by the client asking why people were getting two of the same emails the next day.

Lots of whoopsies there, but if we had better process around the whole
-->

# Horror Stories On Why Seeds are Useful

Preview Environment Emailing Real Users

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
