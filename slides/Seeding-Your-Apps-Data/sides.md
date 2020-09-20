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
  - Using them in both Dev/Production :D
- Horror Stories on Why Seeds are Useful
- Approaches to seeds
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
Maybe you'll also get this
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
