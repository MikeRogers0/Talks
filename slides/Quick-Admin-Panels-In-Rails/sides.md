---
paginate: true
theme: custom-theme
size: 16:9
title: Quick Admin Panels In Rails
_class: prose
---
<!-- _class: lead -->

# Quick Admin Panels In Rails

Using Devise & ActiveAdmin 

---
<!-- _class: lead -->

# What are we going to build?

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
<!--
I really like it
-->

# Why ActiveAdmin?

- It gives you a lot out the box (e.g. search & model relationships)
- Easy DSL that is very configurable
- Really compliments good model design
- Makes concepts like CRUD & Decorators a bit easier to understand
- Please don't custom code an admin interface!

---
<!--
Make sure to use an AdminUser
-->

# Installing

Add to Gemfile:

```bash
$ bundle add devise
$ rails generate devise:install
$ bundle add activeadmin
$ rails generate active_admin:install --use_webpacker
```

---
<!--
Because separating your code is good.
-->

# Why AdminUser?

I've worked on few applications which used a `User` model for their main app and their admin panels.

It always felt like a messy experience, where every route could return to much data:

```rb
# app/controllers/posts_controller.rb

if current_user.admin? 
  @posts = Post.all
else
  @posts = current_user.posts
end
```

Having a "You manage the app over here" lead to simpler code.

---
<!--
Comes with a way to generate paghes
-->

# Adding a resource

```text {7,8}
$ rails generate

ActiveAdmin:
  active_admin:assets
  active_admin:devise
  active_admin:install
  active_admin:page
  active_admin:resource
  active_admin:webpacker
```

```bash
$ rails generate active_admin:resource Author
$ rails generate active_admin:resource Post
$ rails generate active_admin:resource Category
```

---

# Customising index

```ruby
ActiveAdmin.register Post do
  index do
    selectable_column
    column :id
    column :title
    column :author
    column :created_at
    actions
  end
end
```

---

# Changing the filters & scopes

```ruby
ActiveAdmin.register Post do
  filter :title
  filter :author
  filter :created_at
  filter :categories

  scope :published
end
```

---

# Adding a custom action

```ruby
ActiveAdmin.register Post do
 member_action :publish, method: :put do
    resource.publish!
    redirect_to resource_path, notice: "Published!"
  end

  action_item :publish, only: :show, if: proc { !resource.published? } do
    link_to 'Publish', [:publish, :admin, resource], method: :put
  end
end
```

---

# Customising the form

```ruby
ActiveAdmin.register Post do
  permit_params :title, :body, :author_id, category_ids: []

  form do |f|
    f.inputs :title, :body, :author
    f.inputs "Categories" do
      f.input :categories, as: :check_boxes
    end
    actions
  end
end
```

---

# Using a decorator

```bash
$ bundle add draper
```

```ruby
# app/decorators/post_decorator.rb
class PostDecorator < ApplicationDecorator
  delegate_all

  def body
    helpers.simple_format(object.body)
  end
end
```

---

# Using a decorator

```ruby
ActiveAdmin.register Post do
  decorate_with PostDecorator
end
```

---
<!--
Eager loading is a good way to reduce N+1s,
though use with caution, if you eager load to much your memory usage will grow.

I normally only eager load "belongs_to" type relationships
-->

# Performance - Eager Loading

```ruby
ActiveAdmin.register Post do
  # Reduce N+1s (Use with caution)
  includes :author
end
```

---
<!--
Filter with strings, not selects
-->

# Performance - Reducing Memory

```ruby
# config/initializers/active_admin.rb
ActiveAdmin.setup do |config|
  # == Filters
  config.include_default_association_filters = false

  # Or

  config.maximum_association_filter_arity = 12
end
```

---
<!--
Filter with strings, not selects
-->

# Performance - Reducing Memory

```ruby
ActiveAdmin.register Post do
  # filter :author
  filter :author_name, as: :string
end
```

---
<!--
Filter with strings, not selects
-->

# Resources

- https://github.com/activeadmin/activeadmin
- https://activeadmin.info/index.html
- https://github.com/drapergem/draper

---

<!-- _class: lead -->


# Questions?

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
