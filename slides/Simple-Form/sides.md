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

The default form builder requires a lot of writing, let's take a look:

```
$ rails g scaffold Pet name:string last_hugged_at:datetime email:string fluffy:boolean colour:string owner:references
```

- Out of the box the default form builder
- Got to output the label & field serperatly - so easy to forget
- It's requires saying the field types
- because you've got to write more, it more risk of implimenting an inconsistent design

---

# What Are We Trying To Solve?

```
$ bundle add simple_form
$ rails generate simple_form:install
$ rails g scaffold Pet name:string last_hugged_at:datetime email:string fluffy:boolean colour:string owner:references
```

Same form is now a bit less - that's nice, we like that.

---

# What Are We Trying To Solve?

- Let's make fields required
- Let's make a dropdown
- We can add placeholders, errors, hints (strings, then I18n)

```
<%= f.input :colour, collection: @pet.class.colours.keys %>
  enum colour: {
    red: 'red',
    blue: 'blue',
    ginger: 'ginger'
  }
```

---

# Can we customise it any more?

```
$ rails generate simple_form:install --bootstrap
```

---

# Can we customise it any more?

How to change a form & how to do it globally

```
<%= simple_form_for(@pet, wrapper: :horizontal_form) do |f| %>
```

You could potentially make one of these using Tailwind classes.

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
