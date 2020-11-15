---
paginate: true
theme: custom-theme
size: 16:9
title: Rails Internationalization (I18n)
_class: prose
---
# What are we going to do?

- I'm going to show you how to use serve your app in different languages in Rails.
- We'll go through why you'd want to do this!
- I'll show you some approaches & give some context on when to use them.
- I'll demo some ways to test it.

---
<!-- _class: lead -->

# Rails Internationalization (I18n)

Serve your app in the Language your user wants

---

# What Are We Trying To Solve?

Ever been on holiday, opened a website & you have to hunt for a country flag to get your task done? That.

- E.g.: https://www.cd.cz/default.htm

---

# How do we use i18n in Rails?

- `t('.body_text')` - Drop that into a view, it'll say "Oh hey go define a key"
- Define a key - Look it appears.

---

# How do we change the locale?

- Set it explicitly
- Scope in the browser URL
- ACCEPT_LANGUAGE header - Swap in browser.

---

# What about forms & models?

- Changing model names, attributes & errors is totally possible.
- Here is a gem to make it easier:

```ruby
$ bundle add i18n-debug --group "development"
```

---

# What about forms & models ?

- Submit a form with an error
- Change the attribute, model name & error message

---

# What about date formatting?

```ruby
$ bundle add rails-i18n
```

Look the date changes!

---

# Resources

- https://guides.rubyonrails.org/i18n.html
- https://github.com/rack/rack-contrib/blob/master/lib/rack/contrib/locale.rb

---

<!-- _class: lead -->

# Questions?

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
