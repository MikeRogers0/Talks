---
paginate: true
theme: custom-theme
size: 16:9
title: Offline Ruby on Rails
_class: prose
---
<!-- _class: lead -->

# Offline Ruby on Rails

How to make your rails app work offline.

---

# How are we going to do this?!

We're going to use Service Workers

---

# Traditional Request

<div class="center-contents mt-12">
  <img src="images/traditional-internet.svg"/>
</div>

---

# With a Service Worker

<div class="center-contents mt-4">
  <img src="images/service-worker.svg" />
</div>

---

# With a Service Worker

<div class="center-contents mt-4">
  <img src="images/service-worker-offline.svg" />
</div>

---

# Gotchas

- URL of service worker must stay the same, e.g. `/service-worker.js`
- If you're using webpacker-dev-server, it will give you a hard time.
- ~25MB limit ( https://stackoverflow.com/a/35696506/445724 )

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
