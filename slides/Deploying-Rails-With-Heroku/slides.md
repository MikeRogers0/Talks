---
paginate: true
theme: custom-theme
size: 16:9
title: Deploying Rails With Heroku
_class: prose
---

# What are we going to do?

- We're going to get a Rails App online
- I'll show you how to auto run migrations on deploy
- You'll learn a little bit more about how Heroku is setup

---

<!-- _class: lead -->

# Deploying Rails With Heroku

Get your app online

---

# What makes Heroku Special?

- It's cloud infrastructure made easy

---
<!--
Let me explain!

Back in the day we used deploy using Capistrano, and quite everything we needed was on one server.

It was terrible. The servers were often poorly configured, and was hard to scale. Quite often servers would run out of storage, or require someone on call to maintain them.
-->

# What makes Heroku Special?

<div style="display: flex; justify-content: space-around; align-items: center; margin-top: 4rem;">
  <img src="/Deploying-Rails-With-Heroku/images/single-server.svg" alt="Single Server with all services on it" height="400" />
</div>

---
<!--
Then we got a bit smarter, and moved things like the Database/Redis/Storage onto their own servers. Then we'd deploy our app to one or many servers.

It was better, but we still had to maintain & scale servers.
-->

# What makes Heroku Special?

<div style="display: flex; justify-content: space-around; align-items: center; margin-top: 4rem;">
  <img src="/Deploying-Rails-With-Heroku/images/server-with-services.png" alt="Single Server with some cloud services" height="400" />
</div>

---
<!--
Then we got a bit smarter, and moved things like the Database/Redis/Storage onto their own servers. Then we'd deploy our app to one or many servers, sometimes with a load balancers in front of them.

It was better, but we still had to maintain & scale servers.
-->

# What makes Heroku Special?

<div style="display: flex; justify-content: space-around; align-items: center; margin-top: 4rem;">
  <img src="/Deploying-Rails-With-Heroku/images/with-heroku.png" alt="Single Server with some cloud services" height="400" />
</div>

---
<!--
What Heroku allowed, is really easy way to get away from those servers. They handle it, we just give them the code.

This meant we could independently scale parts of our app & not have to worry about the as much.
-->

# What makes Heroku Special?

- All icons in clouds

---

# Let's deploy something

```bash
$ rails new . --database=postgresql
```

Push it to GitHub & setup app.

---

# Add some scaffolding

```bash
$ rails g scaffold Pet name:string
```

Then show off the release tasks
