---
paginate: true
theme: custom-theme
size: 16:9
title: Developing in Multiple Languages
_class: prose
---
<!-- _class: lead -->
<!--
Hi! I'm Mike!!

I'm going to talk about making it easier to jump between languages.
-->

# Developing in Multiple Languages

Let's make it super easy to get started developing in something new!

---
<!-- _class: lead -->
<!--
I think it's really useful to know lots of different languages.

Though installing is kind of a nightmare? Has everyone had the kind experience?

(Everyone nods their head - Maybe let people chime in)

I do Ruby, I really dislike that it's problematic to install.
-->

# Where does this come from?

- I _always_ mess up installing languages, or a Software Update breaks everything for me.

- Also I do Ruby...I still don't know the right way to install it.

---
<!--
I started using both these tools daily over the last few months,

and they're so fantastic!

asdf - I'm going to cover this first, it's used for installing stuff
docker - It really is a tiny computer which ships with everything you need for your app to run.
-->


# What has worked for me?

- asdf - Manage multiple runtime versions with a single CLI tool
- Docker - Tiny virtual computer within your computer, just for your app.

---
<!--
It has the worst name! But it's super.

You can install pretty much anything! And you can switch the version number you have

But the main thing is, it's only a few lines to get started with something new & the upgrade process is the same.
-->

# asdf

It can be used to install Node.js, Python, Ruby, Elixir - Pretty much everything!

https://asdf-vm.com/#/

---
<!--
It has an install page to setup their toolage as per your environment.
-->

# asdf

TODO: Image of their install page.

---
<!--
To install ruby, all you'll need to do is these commands.
-->

# asdf

- Setup Ruby:
  - `$ asdf plugin add ruby`
- Install Latest Ruby Version:
  - `$ asdf install ruby latest`
  - `$ asdf global ruby 2.7.2`

---
<!--
To install python, it's super similar

I think that's really powerful.
-->

# asdf

- Setup Python:
  - `$ asdf plugin add python`
- Install Latest Python Version
  - `$ asdf install python latest`
  - `$ asdf global python 3.9.0`

---

<!--
But what's really cool is you can set the version number based on the folder.

It'll create a `.tool-versions` file for each folders, everything under it will be
set to what you set here.

So I really like that! It's super powerful.
-->

# asdf

- `cd ~/Project && asdf local ruby 2.7.2`
- `cd ~/Other_Project && asdf local ruby 2.6.0`

---

<!--
However, what happens when you've got a lot more going on within your app?
-->

# Docker

Super good projects with lots of dependencies (e.g. different database versions)

Uses lots of memory.

- Docker-compose is cool for turning on lots of things.

---

# Which way is best?

Up to you! I like the path of least documentation.
