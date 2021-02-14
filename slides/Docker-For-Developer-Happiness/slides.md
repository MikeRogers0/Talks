---
paginate: true
theme: custom-theme
size: 16:9
title: Docker For Developer Happiness
_class: prose
---

<!-- _class: lead -->
<!--
Hi I'm Mike!

Docker solved a lot of my developer woes & I want to share it with everyone!
-->

# Docker For Developer Happiness

What is Docker & what does it solve?

---

<!--
-->

# What are we going to cover?

- What is the problem?
- How do we use Docker?
- Docker-Compose makes it easier!
- What are the drawbacks?

---

<!-- _class: lead -->
<!--
I'm going to go through some development scenarios where I think Docker would have helped.
-->

# What is the problem?

---

<!-- _class: lead -->
<!--
This is a true story!

I worked on a product where every developer install the database tool we used (Postgres) at different times & as a result we all were running different versions.

One day we had a developer use a new SQL function which solved our problems really well. We got to production & it didn't work.

Had we been using Docker, we could have say "Everyone use the same Postgres version as Production".
-->

# What is the problem?

🧑‍💻 I'm running Postgres 13
👩‍💻 I'm running Postgres 12
☁️  Jokes on you! I'm still running Postgres 10

---

<!-- _class: lead -->
<!--
Have you ever picked up a legacy project which had a bunch of steps to get started?

Installing some of those packages is hard! Sometimes they throw cryptic errors due to MacOS changing something.

Even worse, what if you end up with a random dependency from that project which screws with another project. It's all really messy.

With Docker you're creating a "little box", with everything your app needs to run in that box. Once you're done, you can throw the box away.
-->

# What is the problem?

🧑‍💻 It's my first day! Why isn't the app starting?
👩‍💻 Just install all the packages listed in the README
📘 README: _Wildly out of date_

---

<!-- _class: lead -->
<!--
This is my big fear!

Someone has a project & I don't know how to turn it on.

With docker, we can make it so how we setup & run our projects pretty much the same.
-->

# What is the problem?

👩‍💻 We need a small change on an old COBOL project!
🧑‍💻 How do I turn this on & see it works?

---

<!-- _class: lead -->
<!--
What are the steps to get going & how do you get the benefits!
-->

# How do we use Docker?

---

<!--
First off, you need to download it
-->

# How do we use Docker?

<div class="center-contents">
  <img src="images/download-docker.png" class="" width="100%" />
</div>

---

<!--
This will give you the desktop app
-->

# How do we use Docker?

<div class="center-contents">
  <img src="images/docker-desktop-app.png" class="" width="100%" />
</div>

---

<!--
After this point you'll have access to docker & all of it's commands in terminal.
-->

# How do we use Docker?

```bash{0}
$ docker help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/Users/mike/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
```

---

<!--
So if you wanted to run Ruby without installing you, could run this command,

It'll download image to run ruby, then you'll get a little ruby console to run commands.

Plus after I'm done with it, it'll go back to it's original state. So I can make a total mess & it'll be fine.

All running in a little sub computer! This I think is pretty powerful!
-->

# How do we use Docker?

```bash{0}
$ docker run --rm -it ruby:3.0.0-alpine
irb(main):001:0* [1,2,3,4].sum
=> 10
irb(main):002:0> 
```

---

<!--
What if we want to create an image, which has a few packages installing with out app there.

We can go to our project root, add a Dockerfile.

You can kind of reason what it might do here! Like, we're going to install yarn & copy the files, bundle then make it so it turns on
-->

# How do we use Docker?

```dockerfile{0}
# Dockerfile
FROM ruby:3.0.0-alpine
RUN apk --no-cache add --virtual build-dependencies build-base yarn

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY . /usr/src/app

# Install Gems & NPM Packages
RUN bundle install && yarn install --check-files

EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0", "-p", "3000"]
```

---

<!--
Then you can totally build that image & run it! Which is super cool!

You can push that image up & share it with people, it's almost job done!
-->

# How do we use Docker?

```bash{0}
$ docker build --tag local/my-app:latest .
$ docker run --rm -it local/my-app:latest
```

---

<!-- _class: lead -->
<!--
After i built a few docker files, i wanted to find a way to manage more of my app!

docker-compose was the answer. it's installed along with the other `docker` command, it should be there.
-->

# docker-compose makes it easier!

https://docs.docker.com/compose/

---

<!--
With Docker-compose, you setup a YAML file called docker-compose.yml which kind of just says:

"here are the things my app needs to run & some configuration". In this case, I've asked for a specific version of Postgres.
-->

```yaml{0}
# docker-compose.yml
version: "3.8"

services:
  postgres:
    image: postgres:12.3-alpine
    volumes:
      - ./tmp/db:/var/lib/postgresql/data

  web:
    build: .
    volumes:
      - .:/usr/src/app
    ports:
      - "3000:3000"
    depends_on:
      - postgres
```

---

<!--
From there, can run this docker-compose up command & it'll just turn on everything we need.
-->

# docker-compose makes it easier!

```bash{0}
$ docker-compose up

Starting app_postgres_1 ... done
Starting app_web_1      ... done
Attaching to app_web_1
web_1        | => Booting Puma
web_1        | => Rails 6.0.3.4 application starting in development 
web_1        | => Run `rails server --help` for more startup options
web_1        | Puma starting in single mode...
web_1        | * Listening on tcp://0.0.0.0:3000
web_1        | Use Ctrl-C to stop
```

---

<!--
I can also go in & run adhoc commands :
-->

# docker-compose makes it easier!

```bash{0}
$ docker-compose run --rm web bash

Creating app_web_run ... done
bash-5.1$ 
```

---

<!--
I can also just run on the services I care about.

So sometimes I just turn on my database, then run my rails app via my local machine.
-->

# docker-compose makes it easier!

```bash{0}
$ docker-compose up postgres

Starting app_postgres_1 ... done
```

---

<!-- _class: lead -->

# What are the drawbacks?

---

# What are the drawbacks?

- High memory usage on MacOS
- It is a machine with your machine, so it isn't as fast as running natively
- The images uses a lot of disc space.

---

<!--
I also like this quote from Nicky T!

It's not a silver bullet, you can still get some quirks slipping through & some people use it in different ways.
-->

# What are the drawbacks?

> "Works on my Docker"
Nick Taylor ( @nickytonline )

---

<!-- _class: lead -->

<!--
Breath & tell them your name!

I hope this inspired you to want to try Docker!
-->

# Thank you

Twitter: @MikeRogers0

Blog: [mikerogers.io](https://mikerogers.io/)