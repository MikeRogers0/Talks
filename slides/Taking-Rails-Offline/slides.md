---
paginate: true
theme: custom-theme
size: 16:9
title: Taking Rails Offline
_class: prose
---
<!-- _class: lead -->
<!--
This started off as a "I've seen a few sites with offline fallbacks, how easy is it in rails" experiment.

I ended up quite liking the results & wanted to share them!
-->

# Taking Rails Offline

How to make your Ruby on Rails apps resilient to unreliable networks

---

# What are we going to do?

- We're going to go through some scenarios where this will be advantageous
- I'll run you the approach
- I have some gems which save you writing JavaScript
- I'll explain the limitations
- Maybe a demo!

---
<!-- _class: lead -->
<!--
So where does _this_ come from!

I have a few use cases, if you've experienced this throw up some emojis:

- You go onto a train, maybe it goes underground & no network is unavailable. So you can't view a website.
- You're at home & someone on your local network eats all the bandwidth, so pages get slow.
- Website you visited quite recently just goes down

These are all problems we can mitigate against!
-->

# Where does this come from?

Chuck up an emoji if the scenarios I'm pitching sound familiar!

---
<!-- _class: lead -->
<!--
To do that we use bit of browser technology called a Service Worker.

Chuck up your emojis if you heard of them!
-->

# How are we going to do this?!

We're going to use Service Workers

---
<!-- _class: lead -->
<!--
You might have seen them in browsers, in a normal day they're there working the background on lots of sites.
-->

# How are we going to do this?!

<div class="center-contents mt-8">
  <img src="images/service-workers-in-firefox.png" class="bordered" />
</div>

---
<!-- _class: lead -->
<!--
This is the main thing we'll be using a service worker for. You kind of see something is happening to these requests
and the service worker is doing it.
-->

# How are we going to do this?!

<div class="center-contents mt-8">
  <img src="images/service-worker-networks.png" class="bordered" />
</div>

---
<!--
So what is happening? Well in a normal request the user asks for the application, and we'll make requests to the internet.
-->

# Traditional Request

<div class="center-contents mt-12">
  <img src="images/traditional-internet.svg" class="bordered" />
</div>

---
<!--
When we add a service worker, we're able to tell the browser:

"Hey, we'd use to use some javascript to decide what to do with this request"
-->

# With a Service Worker

<div class="center-contents mt-2">
  <img src="images/service-worker.svg" class="bordered" />
</div>

---
<!--
You can do a lot, the main use case is having a cache & telling browser to just go direct to the cache
instead of even trying the network.

But it can also fallback to the cache if the network is down.
-->

# With a Service Worker

<div class="center-contents mt-2">
  <img src="images/service-worker-offline.svg" class="bordered" />
</div>

---
<!-- _class: lead -->
<!--
This is a browser thing, so we will be doing some JS.

But this is a Ruby group, so I'm going to show off the gems.

I did start writing my own thing to show you, but looked big.
-->

# How do we implement a service worker

JavaScript (But there are some nice gems to do it for us!)

---

Explain the main steps:

- application.js - Register the service worker
- Browser request the file, turns it on
- We have events like 

---
<!-- _class: lead -->
<!--

-->

# How do we implement a service worker

```javascript
// service_worker.js

```

---

# Installing with Gem

- https://github.com/rossta/serviceworker-rails
- `gem 'serviceworker-rails'`
- `rails g serviceworker:install`

---

# Installing with Gem

- Go through files, show it working

---

# How to access the debugging tools

- `about:debugging`
- Throw up URLs for different browsers

---

# Approaches to caching

- Cache files ahead of time
- Cache after visiting
- Mix of both.

---

# Using webpacker in your service worker

- Make it load with ES6 using `webpacker-pwa`
- Sweet, we can use workbox now.

---

# Gotchas

- URL of service worker must stay the same, e.g. `/service-worker.js`
- If you're using webpacker-dev-server, it will give you a hard time.
- ~25MB limit ( https://stackoverflow.com/a/35696506/445724 )

---

# Notes

- https://github.com/rossta/serviceworker-rails
- https://developers.google.com/web/fundamentals/primers/service-workers
- https://developers.google.com/web/tools/workbox/guides/advanced-recipes
- https://developers.google.com/web/ilt/pwa/caching-files-with-service-worker
- https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#avoid-url-change
- https://dev.to/coorasse/the-progressive-rails-app-46ma
- https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin
- https://github.com/coorasse/webpacker-pwa
- https://www.youtube.com/watch?v=RJZbWw5GEfU
