---
paginate: true
theme: custom-theme
size: 16:9
title: Installing Ruby On Rails via asdf
_class: prose
---

# What are we going to do?

- Setup Ruby using `asdf` version manager
- Run `rails new`, get it to turn on

---

<!-- _class: lead -->

# Installing Ruby On Rails

Install Ruby the 2020 Way

---

# Before we start

- This is for Linux & MacOS. If you're on Windows, use Docker.
- Be wary about advice which says you should use `sudo` on StackOverflow.
- There are other ways to install Ruby (rbenv, chruby & RVM), we're going to use asdf.

---

# Install asdf

- Visit https://asdf-vm.com
- I like asdf, it supports multiple languages so you can use it to install nodejs & yarn.
- Explain the instructions

---

# Install Ruby

- asdf works via plugins, you can install lots of languages
- `$ asdf plugin list all`
- `$ asdf plugin add ruby`
- `$ asdf install ruby latest`
- `$ asdf global ruby`

---

# Gotchas

You need to set this one file to include this line, otherwise rails project will shit the bed.

```
$ cat ~/.asdfrc 
```

```text
legacy_version_file = yes
```

---

# Also Install Yarn & Nodejs

- Also, we need to add nodejs & yarn
- `$ asdf plugin add nodejs`
- `$ asdf plugin add yarn`
- `$ asdf install nodejs latest`
- `$ asdf install yarn latest`
- `$ asdf global nodejs 14.14.0`
- `$ asdf global yarn `

---

# Install Rails gem

- `$ gem install rails`

---

# Make a new Rails Project

- `$ rails new`

---

<!-- _class: lead -->

# Questions?

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
