---
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
title: An Introduction To Stimulus.js
---

![bg left:40% 80%](https://raw.githubusercontent.com/marp-team/marp/master/marp.png)

# **An Introduction To Stimulus.js**

What is it & when is it useful?

https://mikerogers.io/

---

# Who am I?

I'm Mike!

{ Add Icons Of frameworks I've done over the years }

---

# What we'll cover

- Where does Stimulus Come From?
- What does it look like?
- How can we set it up in Rails!
- When is it useful?

The outcome should be you'll confident enough to give Stimulus a try & know about where it's useful.

---

# Where does Stimulus Come From?

{ Basecamp Logo }

It's made by Basecamp.

<!--
Story is they wanted a way to organise their JavaScript, but didn't like SPA. Stimulus is what came out.
-->

---

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div>
</div>
<div>
</div>
</div>

```html {1,2,4-5}
<!-- index.html -->
<div data-controller="counter">
  <span data-target="counter.output"></span>
  <button data-action="click->counter#addOne">
    Add One
  </button>
</div>
```

```javascript
// counter_controller.js
import { Controller } from "stimulus"

export default class extends Controller {
  static targets = [ "output" ]
  
  initialize(){
    this.clickCount = 0
  }
  
  connect(){
    this._updateOutput();
  }

  addOne() {
    this.clickCount++;
    this._updateOutput();
  }
  
  _updateOutput() {
    this.outputTarget.innerText = `You've clicked ${this.clickCount} times`
  }
};
```

---

# How can we set it up in Rails!

```bash
$ bundle add webpacker
$ bundle
$ bundle exec rails webpacker:install:stimulus
```
