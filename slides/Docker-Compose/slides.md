---
paginate: true
theme: custom-theme
size: 16:9
title: Docker Compose
_class: prose
---

<!--
Docker-Compose is like awesome!

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
