---
paginate: true
theme: custom-theme
size: 16:9
title: Seeding your Apps Data
_class: prose
---
<!-- _class: lead -->

# Seeding your Apps Data

Rails seeds are very powerful

---
<!-- _class: lead -->
<!--
We're going through stuff!
-->

# What we'll cover

- What are seeds?
- Are they just for Development environments?
- Horror Stories on Why Seeds are Useful
- Approaches to seeding
- Ways to make it easy!

---
<!-- _class: lead -->
<!--
Please remember to Like/Comment/Subscribe!
-->

# Before I start

<div style="display: flex; justify-content: space-around; align-items: center; font-size: 1.2rem; margin-top: 10rem; margin-bottom: 5rem;">
  <img src="/assets/images/youtube-like.svg" height="100" class="wiggle" />
  <img src="/assets/images/youtube-subscribe.png" height="100" class="wiggle" />
</div>

---
<!--
-->

# What are seeds?

They're bits of data we use to pre-populate our database. They could be anything from a standard local development user, to even pricing data.

Out of the box, Rails stores them in the `db/seeds.rb` file. They're added to your environment by running `rails db:seed` (Which is often part of `bin/setup`).

---
<!--
The default sample sucks so hard.

It can't comfortably be run multiple times & doesn't allow for if the data has changed in the DB since seeding.
-->

# What are seeds?

The out of the box seeds sample looks like:

```ruby
# db/seeds.rb

movies = Movie.create([
  { name: 'Star Wars' },
  { name: 'Lord of the Rings' }
])
Character.create(name: 'Luke', movie: movies.first)
```
---
<!--
If I'm lucky they look like this
-->

# What are seeds?

Normally they'd probably look a bit more like:

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

# What are seeds?

Normally they'd probably look a bit more like:

```ruby
# db/seeds.rb

# Last updated 10 years ago - DON'T USE RUN IN PRODUCTION
TaxRate.find_or_create_by!(origin: 'NL', destination: 'GB') do |tax_rate|
  tax_rate.amount = "1.10"
end
```
---
<!-- _class: lead -->
<!--
Normally that's where it's only used.

Preview Environments sometimes have a set of seeds to help demo the environment.

If something if the same, don't put it in the DB, use a PORO or something.
-->

# Are they just for Development environments?

You can use them in all environments (Test, Development, Preview & Production).

Though if something is the same between all environments, it probably doesn't belong in the Database.

---
<!-- _class: lead -->
<!--
First story: Dev given production copy of production DB. Laptop was stolen.
-->

# Horror Stories on Why Seeds are Useful

The stolen laptop

---
<!-- _class: lead -->
<!--
Second story: Preview Environment used real data. It then emailed all the users.
-->

# Horror Stories on Why Seeds are Useful

Preview Environment started emailing real users

---
<!-- _class: lead -->
<!--
Last Story: Don't make devs jump through hoops to have a decent environment.
-->

# Horror Stories on Why Seeds are Useful

The poor initial developer experience

---
<!--
-->

# Approaches to seeding

- Explicitly defined seeds - `db/seeds.rb` & similar approaches
- What about fixtures & factories?
- A shared anonymised version of the production DB

---
<!--
-->

# Explicitly defined seeds

- `db/seeds.rb` & `rails db:seed`/`rails db:seed:replant` - Can get messy quickly, multiple seed files helps.
- https://github.com/rroblak/seed_dump - Dump your existing DB into seeds
- https://github.com/mbleigh/seed-fu - Kind of a nice structured approach
- https://github.com/james2m/seedbank - Super when things needs to be done in a specific order.

---
<!--
-->

# What about fixtures & factories?

There is a command `rails db:fixtures:load FIXTURES=users,posts`, it will load fixtures into your development environment.

Kind of rare, though I understand it's unreliable around foreign keys. But I like how this will give your tests good objects to test against.

You can loop through factories.

---
<!--
-->

# A shared anonymised version of the production DB

https://github.com/sunitparekh/data-anonymization
https://github.com/DivanteLtd/anonymizer
https://github.com/evilmartians/evil-seed
