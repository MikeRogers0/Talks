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

I'm going to be talking seeding in rails, it's something which we all probably know about
but most the projects we're using probably are have empty files.
-->

# Seeding Data In Ruby On Rails

A good `db/seeds.rb`, helps every app grow

---
<!-- _class: lead -->
<!--
This is what I'm going to be covering in this.

The idea is by the end of it, you'll want to take any existing/new project and just add a few seeds.

Plus I have some cool tricks to get a lot of value from your seed data.
-->

# What We'll Cover

- What Are Seeds?
- We'll Check The Vibe
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

- Preview environment didn't have basic auth on it & passwords were the same.
- The preview environments ran the cronjobs same as production
- The dataset was an anonymised snapshot of production, often a few months behind (It would take a while to run & was very manual).
- The preview environment used MailTrap to stop emails going out. But one day our client wanted to see the emails in their inbox. So some emails weren't anonymised in the snapshot.

Then one day the client said "I want an exact copy of production in staging".

Lots of whoopsies there, but if we had better process the preview environments sample data, it would have been avoidable.
-->

# Horror Stories On Why Seeds are Useful

Preview Environment Emailing Real Users

---
<!-- _class: lead -->
<!--
You always kind of know a project is going to be rubbish when you need to open console to create a user to login with.

It's kind of nice to go through a local application with a developer & show them lots of pages with real feeling content on it.
Plus it's even better if there are 100s of records to make N+1s obvious.


This is something I'm quite passionate about, like if we want people to really enjoy working with Rails we need to care about this kind of low hanging fruit.
-->

# Horror Stories On Why Seeds are Useful

The poor initial developer experience

---
<!--
So know we know what were trying to avoid, how do we do it?
-->

# Approaches To Seeding

- Explicitly Defined Seeds
- Faker Generated Seeds
- Fixtures & Factories Generated Seeds
- Anonymised Production Database
- Plain Old Ruby Objects

---
<!--
These are the ones you'd find in you `db/seeds.rb` file.

The files can become very big pretty fast. I've seen them broken up into smaller files before

Sometimes they might if statements to handles different environments and whatnot.
-->

# Explicitly Defined Seeds


```ruby {0}
# db/seeds.rb
# Load all the files in db/seeds folder
Dir[File.join(Rails.root, 'db', 'seeds', '*.rb')].sort.each do |seed|
  load seed
end
```

```ruby {0}
# db/seeds/companies.seeds.rb
Company.find_or_create_by!(name: 'Hatch') do |company|
  company.other_details = "Awesome Stuff"
  company.projects = [Project.new(title: 'Acme')]
end
```

---
<!--
We all know faker?

I find fuzz testing in development is kind of nice to find weird use cases.
If I don't use Faker, I normally end up rolling my face over the keyboard.

It make me think about the HTML components I'm designing.
-->

# Faker Generated Seeds

https://github.com/faker-ruby/faker

```ruby {0}
require 'faker'

Faker::Name.name      #=> "Christophe Bartell"
Faker::Internet.email #=> "kirsten.greenholt@corkeryfisher.info"
```

---
<!--
I like this, especially in development environments
-->

# Faker Generated Seeds

```ruby {0}
# db/seeds.rb
require 'faker'

User.find_or_create_by!(email: 'admin@example.com') do |user|
  user.name = Faker::Name.name
  user.password = "12345678"
  user.password_confirmation = "12345678"

  user.posts << Post.new(title: Faker::Job.title)
  user.posts << Post.new(title: Faker::Job.title)
  user.posts << Post.new(title: Faker::Job.title)
end if ENV['SEED_USER']
```

---
<!--
You can also take it to the next level, and use the data you'll use in your tests to the mess with your app.

When I was experimenting with this, I found this command.
-->

# Fixtures Generated Seeds

```bash {0}
$ rails db:fixtures:load FIXTURES=users,posts
```

There is a command, it will load fixtures into your current development environment.

_I've not seen this approach used in the wild, when I tried it I had trouble with foreign keys (Plus I've never worked on a project with decent fixtures)._

---
<!--
So I wondered what would happen if I just used these instead!

I generally have pretty flushed out factories, which are good representations of my expected data.
-->
<!-- footer: https://thoughtbot.com/blog/factory_girl-for-seed-data -->

# Factories Generated Seeds

```ruby {0}
FactoryBot.define do
  factory :user do
    name { Faker::Name.name }
    password { User.new.send(:password_digest, "12345678") }
    sequence(:email) { |n| "email#{n}@example.com" }

    factory :user_with_channel do
      accounts { [build(:account_with_channel, owner: instance)] }
    end
  end
end
```

---
<!--
I'm really liking this approach!

ThoughtBot doesn't encourage this, but I've been using it a little & I really like it.

The happy side effect I'm noticing it is:
- Easier to write tests, as I'm looking at what the test will see.
- I'm more incentivised to make working factories.

I think if your app is simple, and you're starting fresh this could be a valid approach.
-->

# Factories Generated Seeds


```ruby {0}
# db/seeds.rb
require 'factory_bot'

if ENV['SEED_USER'] && User.where(email: 'example@example.com').zero?
  FactoryBot.create(:user_with_channel, email: 'example@example.com'))
end
```

I've been really liking this approach, it makes me want to have really good factories & run `rails db:reset` often.

---
<!-- footer: "" -->
<!--
But what if you're picking up a project where there is nothing?

Evil Martians have this pretty cool library for taking snapshots of user data & the relationships.

So potentially you could automate a subset of data to be available for your local development & review apps.

Potentially you could also run this in a way where you can log who is requesting what data, weather via version control or something else.
-->

# Anonymised Production Database

```ruby {0}
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

https://github.com/evilmartians/evil-seed

---
<!--
Sometimes I find data is the same in all environments, but is in the database.

So lets say you have a user with a Plan relationship. The plans are all the same for each environment.
-->

# Plain Old Ruby Objects

Have you ever seen this?

```ruby {0}
class User < ApplicationRecord
  belongs_to :plan
end

User.new(plan: Plan.premium).plan.display_adverts?
# Outputs: false
```

---

<!--
So what if we pulled all the data from that table & put it into a Struct
Along with some helper methods
-->

# Plain Old Ruby Objects


```ruby {0}
# models/plan.rb
Plan = Struct.new(:id, :display_adverts, keyword_init: true) do
  def self.all
    @all ||= [
      Plan.new(id: 'free', display_adverts: true),
      Plan.new(id: 'premium', display_adverts: false)
    ]
  end

  alias_method :display_adverts?, :display_adverts
end
```

---
<!--
Then rewrote our model to look like this.

We avoid having to touch the database for this information, plus it's now the same across all environments.

Plus all the data changes are in version control.
-->

# Plain Old Ruby Objects


```ruby {0}
class User < ApplicationRecord
  def plan
    @plan ||= Plan.all.find { |plan| plan.id == plan_id }
  end
end

User.new(plan_id: 'premium').plan.display_adverts?
# Outputs: false
```


---
<!--
DO IT!!

I started just adding a file where I'd call the load_seed function
with the various ENVs which do stuff & I'd just check stuff happens in my database.

It doesn't have to be anything fancy, but it will pick up on any whoopsies.
-->

# Testing Seeds

```ruby {0}
# spec/db/seeds_spec.rb
RSpec.describe 'Rails.application' do
  describe '#load_seed' do
    subject { Rails.application.load_seed }
    before { ENV['SEED_USER'] = 'true' }
    after { ENV['SEED_USER'] = nil }

    it do
      expect { subject }.to change(User, :count).by(1)
        .and change(Channel, :count).by(1)
    end
  end
end
```

---
<!--
Plus as I use FactoryBot for my seeds right now, I often call the linter as part of the test suite.

It's a really cool! From Factories I'm going to write anyway, I'm able to use them more effectively.
-->

# Testing Seeds

If you're using FactoryBot you can Lint factories to find out quickly why something is failing.

```ruby {0}
# spec/factory_bot_spec.rb
require "rails_helper"

describe FactoryBot do
  it { FactoryBot.lint traits: true }
end
```


---
<!--
So what should we be doing?

So you should be using seeds, they're a good foundation for any app.
-->

# What is the best way?

- You should be able to run `rails db:seed` multiple times without fear, ideally using ENV's to decide what is seeded.
- Use them in a preview environment! You'll be more incentivised to keep them up to date & fleshed out.
- Plain Old Ruby Objects for data that needs to be consistent across all environments.
- Using FactoryBot for Seeds makes testing easier.
- Use partial anonymised dumps of production to investigate exceptions more closely.

---
<!-- _class: lead -->
<!--
Make a project better
-->

# Homework

I want you to take a project with incomplete seeds, and make them 10% better.

Then let me know how it goes!

---
<!-- _class: lead -->


# Thank you!

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
