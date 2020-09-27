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

# What are we going to build

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
- Easy DSL
- It's pretty flexible
- It'll require very little effort if your models are well designed
- Please don't custom code an admin interface!

---
<!--
Make sure to use an AdminUser
-->

# Installing

Add to Gemfile:

```bash
rails generate devise AdminUser
bundle add activeadmin
rails generate active_admin:install --use_webpacker
rails generate active_admin:webpacker
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

# Adding a resource

```bash
$ rails generate active_admin:resource Post
$ rails generate active_admin:resource Category
```
