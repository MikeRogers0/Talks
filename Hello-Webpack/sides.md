---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
---

![bg left:40% 80%](https://raw.githubusercontent.com/marp-team/marp/master/marp.png)

# **Hello Webpack**

Webpack is pretty nifty! Let's get you up and going!

https://mikerogers.io/

---

# What we'll cover

- A quick history (How did we get here?)
- What are the main benefits of Webpack
- What about the bad stuff?
- How to get setup in Rails
- Let's write some Stimulus JS!
- What about CSS?
- Some really cool stuff! (Tailwind/PurgeCSS)

---

# A quick history (How did we get here?)

- Back when I started Rails, the asset pipeline was amazing!
- Then we wanted assets...Queue image of various approaches (Vendor folder, Linking directly to somewhere, gems just for assets, rails assets)
- Yarn won as the best way, but it wasn't baked into Rails.
- While this was happening, NPM kind of solved the problem for us.

---

# What are the main benefits of Webpack

- You get to use JS assets from NPM (Yay)
- PostCSS (It's super fast!)
- webpack-dev-server (Reload pages when JS changes)
- It's _fast_.
- Modern JS (Much cleaner)

---

# What about the bad stuff?

- Say goodbye to jQuery, it's not ideal
- Some gems aren't setup for it yet, but for them Assets Pipeline is ok.
- `app/javascript` is a terrible name.
- But that's OK! It'll get better.

--- 

# How to get setup in Rails

- Install gem
- Run command
- Change some paths around...done!
- Use [Strangler fig approach](https://martinfowler.com/bliki/StranglerFigApplication.html) of moving assets over.

---

# Let's write some Stimulus JS!

- Demo writing some bits.

---

# What about CSS?

Yeah it can do that also! Do a quick demo of PostCSS/SCSS.

---

# Some really cool stuff! (Tailwind/PurgeCSS)

- Tailwind!
- PurgeCSS
- JS packages for doing stuff in the asset build step, e.g. inlining SVGs * optimising images.
