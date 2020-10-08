---
paginate: true
theme: custom-theme
size: 16:9
title: Simple Form
_class: prose
---
# What are we going to do?

- I'm going to show you how to make _this_ form in 5 lines of code
- We're going to change form styles _really easily_ 

---
<!-- _class: lead -->

# Simple Form

So much less code

---

# What Are We Trying To Solve?

The default form builder is really verbose, let's take a look:

```
$ rails g scaffold Pet name:string last_pet_at:datetime email:string fluffy:boolean colour:string owner:references
```

---

# What Are We Trying To Solve?

```
$ bundle add simple_form
$ rails generate simple_form:install
$ rails g scaffold Pet name:string last_pet_at:datetime email:string fluffy:boolean colour:string owner:references
```

Same form is now a bit less - that's nice, we like that.

---

# What Are We Trying To Solve?

- Let's make fields required
- Let's make a dropdown
- We can add placeholders, errors, hints (strings, then I18n)

---

# Can we customise it any more?

```
$ rails generate simple_form:install --bootstrap
```

---

# Can we customise it any more?

How to change a form & how to do it globally

```
simple_form_for @user, wrapper: :something
```

---
<!--
-->

# Resources

- https://github.com/heartcombo/simple_form

---

<!-- _class: lead -->

# Questions?

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
