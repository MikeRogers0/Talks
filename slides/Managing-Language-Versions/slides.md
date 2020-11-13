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
I think it's really useful to be able to play lots of different languages.

Though!! I really dislike installing them! I always find it's more of a nightmare then it should be!

(Everyone nods their head - Maybe let people chime in)

I work with Ruby, I really dislike that it's problematic to install.
-->

# Where does this come from?

- I _always_ mess up installing languages, or have a Software Update breaks everything for me.

- Also I do Ruby...I still don't know the _right way_ to install it 🤫

---
<!--
I started using both these tools daily over the last few months,

and they're so fantastic!

I'm going to go into what they both are & hopefully pique your interest enough to make you want to try them.
-->


# What has worked for me?

- asdf - Manage multiple runtime versions with a single CLI tool
- Docker-Compose - Tiny virtual computers within your computer, with a sprinkle of orchestration.

---
<!--
It has the worst name! But it's super good.

You can install pretty much anything! And you can switch the version number you have

But the main thing is, it's only a few lines to get started with something new & the upgrade process is the same.
-->

# asdf

It can be used to install Node.js, Python, Ruby, Elixir, Elm - Pretty much everything!

https://asdf-vm.com/#/

---
<!--
It has a really super install page, where you can put in your setup & it'll give you tailored install instructions. 
-->

# asdf

<center>
  <img src="images/asdf-vm-setup.png" width="100%" />
</center>

---
<!--
Once you are setup, you only need to know a few commands to get Ruby working on your machine

I think it's approachable!
-->

# asdf

- Setup Ruby:
  - `$ asdf plugin add ruby`
- Install Latest Ruby Version:
  - `$ asdf install ruby 2.7.2`
  - `$ asdf global ruby 2.7.2`

---
<!--
To install python, it's similar

I think that's really powerful.
-->

# asdf

- Setup Python:
  - `$ asdf plugin add python`
- Install Latest Python Version
  - `$ asdf install python 3.9.0`
  - `$ asdf global python 3.9.0`

---

<!--
Plus it's not like messing your with system, it's just putting files in a dot folder. Which I quite like.
-->

# asdf

Once you're up and going, you'll notice when you type the python command it's actually coming from the `.asdf` folder.

```bash
$ which python
/Users/mike/.asdf/shims/python

$ python --version
Python 3.9.0
```

---

<!--
But what's really cool is you can set the version number based on the folder.

So if you have one project running an older version of a language, that's ok.

It'll create a `.tool-versions` file for each folders, everything under it will be
set to what you set here.


I've been using asdf a lot to play with exercisms lately.
-->

# asdf

Setting Ruby versions on a per-folder basis:

- `$ cd ~/Project && asdf local ruby 2.7.2`
- `$ cd ~/Other_Project && asdf local ruby 2.6.0`

---
<!--
However! There are scenarios where asdf isn't the right tool,

e.g. when you have a project which relies on multiple dependencies, installing all of them could be a tad annoying.

This is where Docker-Compose is like awesome!

It requires a bit of setup for each project, but you're in the right place it's easy for everyone on a team to have the same setup.
-->

# Docker-Compose

This command turns _everything_ on for you:

```
$ docker-compose up
```

_(With a bit of configuration)_

---
<!--
Docker-compose is kind of a deep dive topic in itself.

But you'll come across these two files
-->

# Docker-Compose

The configuration normally revolves around two files:

- `Dockerfile`
- `docker-compose.yml`

---
<!--
The Dockerfile is used to used to say "This is what I want this app's environment to look like". So like, all the things you'd need to run that bit of the app. So like JavaScript libraries and stuff.

Your Dockerfile, might look a bit like this:

They normally are a bit more complicated, I was a bit limited by what I wanted on this slide.

If you're are interested in a better example for Rails ping me after this, I have a way better file on my GitHub!
-->

The `Dockerfile` looks like:

```Dockerfile
FROM ruby:2.7.2

RUN apt-get install -y nodejs postgresql-client

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY . /usr/src/app

# Install Gems & NPM Packages
RUN bundle install
RUN yarn install --check-files

EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0", "-p", "3000"]
```

---
<!--
Here is what the docker-compose file looks like, when you run that "docker-compose up" command it's going to go into here and turn on each of these services in their own virtual machine.

When I first started docker, I found this file a bit intimidating. But once break it down a bit, that's just saying "We want a database server & our rails app, plus use our local directory for the data"
-->

The `docker-compose.yml` file looks like:

```yaml
version: "3.8"

services:
  postgres:
    image: postgres:12.3-alpine
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password

  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/usr/src/app
    ports:
      - "3000:3000"
    depends_on:
      - postgres
```

---
<!--
Once you've got the hang of Docker, you can pretty much start each project with the same command.

It's really super!
-->

# Docker-Compose

```bash
$ docker-compose up
Starting app_postgres_1 ... done
Starting app_web_1      ... done
Attaching to app_web_1
web_1        | => Booting Puma
web_1        | => Rails 6.0.3.4 application starting in development 
web_1        | => Run `rails server --help` for more startup options
web_1        | Puma starting in single mode...
web_1        | * Version 4.3.6 (ruby 2.7.1-p83), codename: Mysterious Traveller
web_1        | * Min threads: 5, max threads: 5
web_1        | * Environment: development
web_1        | * Listening on tcp://0.0.0.0:3000
web_1        | Use Ctrl-C to stop
```

---
<!--
I really did only touch a little on both tools!
-->

# asdf vs Docker-Compose

**In summary:** I like both of them!

They each make getting getting setup on new projects & languages a bit easier. asdf is better just the one languages, while Docker-Compose is super for when you have more moving parts.
