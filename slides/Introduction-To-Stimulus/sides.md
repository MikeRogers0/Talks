---
paginate: true
theme: custom-theme
size: 16:9
title: An Introduction To Stimulus.js
_class: prose
---
<script src="https://unpkg.com/stimulus@1.1.1/dist/stimulus.umd.js" type="text/javascript"></script>
<script type="text/javascript">
  const application = Stimulus.Application.start();

  class CounterController extends Stimulus.Controller {
    static get targets() {
      return ["output"];
    }

    // Called when Stimulus create an instance of the
    // CounterController class.
    initialize(){
      this.clickCount = 0
    }

    // Called when the class is connected to the HTML element.
    connect(){
      this._updateOutput();
    }

    // Called via:
    // data-action="click->counter#addOne"
    addOne() {
      this.clickCount++;
      this._updateOutput();
    }

    _updateOutput() {
      this.outputTarget.innerText = `You've clicked ${this.clickCount} times`
    }
  };

  application.register("counter", CounterController);
</script>

<!-- _class: lead -->

<img src="/Introduction-To-Stimulus/images/stimulus.svg" width="100" height="100" style="margin: 0 auto;" />

# An Introduction To Stimulus.js

What is it & when is it useful?

---
<!-- _class: lead -->
<!--
Online I go by @MikeRogers0, normally I have a fairly dull setup which is a pretty enjoyable way to work.
-->

# Before I start

<div style="display: flex; justify-content: space-around; align-items: center; font-size: 1.2rem; margin-top: 10rem; margin-bottom: 5rem;">
  <img src="/assets/images/youtube-like.svg" height="100" />
  <img src="/assets/images/youtube-subscribe.png" height="100" />
</div>

---
<!-- _class: lead -->
<!--
- Here is what I'm aiming to cover
- The idea of this talk is give you an idea of the use cases where it's useful, what it's best for
- But also where it's not the best choice.
-->

# What we'll cover

- Where does Stimulus Come From?
- What does it look like?
- When is it useful?
- How can we set it up in Rails!

The outcome should be you'll confident enough to give Stimulus a try & know about where it's useful.

---
<!-- _class: lead -->
<!--
We didn't have many options.
Single Page Applications were out there, but in their infancy, along with understanding of progressive enhancement.

The end result was lots of spaghetti code.
-->

# Where does Stimulus Come From?

When Ruby On Rails first started, our options were limited.

<div style="display: flex; justify-content: space-around; align-items: center; font-size: 1.2rem; margin-top: 5rem;">
  <img src="/Introduction-To-Stimulus/images/coffeescript-logo.png" height="150" />
  <img src="/Introduction-To-Stimulus/images/jQuery_logo.png" height="100" />
  <img src="/Introduction-To-Stimulus/images/backbone.png" height="125" />
</div>

---
<!-- _class: lead -->
<!--
React & Vue took off, they solved a lot of problems.
But they also added an additional moving part of our apps we could potentially avoid.
-->

# Where does Stimulus Come From?

Then we ended up with libraries which favoured rendering on the client side.

<div style="display: flex; justify-content: space-around; align-items: center; font-size: 1.2rem; margin-top: 5rem;">
  <img src="/Introduction-To-Stimulus/images/React_logo_logotype_emblem.png" height="200" />
  <img src="/Introduction-To-Stimulus/images/1200px-Vue.js_Logo_2.svg.png" height="200" />
</div>

---
<!-- _class: lead -->
<!--
Not to mention added some new problems HTML kind of solved.

I really wanted to just get back to sending HTML to users, with a bit of JavaScript to jazz it up.
-->

# Where does Stimulus Come From?

<div align="center" style="margin-top: 2.5rem;">

[![HTML is faster then React](/Introduction-To-Stimulus/images/zachleat-html-is-fast.png)](https://twitter.com/zachleat/status/1169998370041208832)

</div>

---
<!-- _class: lead -->
<!--
The story:

- Basecamp wanted that "fluid interfaces set free from the full-page refresh" you can get with SPA
- Stimulus came about because they could achieve that with Turbolinks + some organised JS
-->

# Where does Stimulus Come From?

<img src="/Introduction-To-Stimulus/images/basecamp.svg" width="300" height="200" style="margin: 1rem auto; display: block;" />

- It was made Javan Makhmali (He works at Basecamp)
- First launched released in January 2018
- Current version is v1.1.1

---
<!-- _class: lead -->
<!--
Then it was added to Rails.
-->

# Where does Stimulus Come From?

```text {9}
$ rails webpacker
Available Webpacker tasks are:
webpacker:info                Provides information on Webpacker's environment
webpacker:install:react       Installs and setup example React component
webpacker:install:vue         Installs and setup example Vue component
webpacker:install:angular     Installs and setup example Angular component
webpacker:install:elm         Installs and setup example Elm component
webpacker:install:svelte      Installs and setup example Svelte component
webpacker:install:stimulus    Installs and setup example Stimulus component
webpacker:install:erb         Installs Erb loader with an example
webpacker:install:coffee      Installs CoffeeScript loader with an example
webpacker:install:typescript  Installs Typescript loader with an example
```

---
<!--
Not to unapproachable!
-->

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; grid-template-rows: repeat(2, 1fr); column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div style="grid-area: 1 / 1 / 2 / 2;">

```html
<!-- index.html -->
<div data-controller="counter">
  <span data-target="counter.output"></span>
  <button data-action="click->counter#addOne">
    Add One
  </button>
</div>
```

</div>

<div style="grid-area: 2 / 1 / 3 / 2; ">
  <div data-controller="counter">
    <span data-target="counter.output">0</span>
    <button data-action="click->counter#addOne">
      Add One
    </button>
  </div>
</div>
<div style="grid-area: 1 / 2 / 3 / 3;">

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

</div>
</div>

---
<!--
Controllers have names often linked to their file names,
so normally it's pretty easy to be like "Ok I'm looking at this in the HTML, let's find the file"
-->

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; grid-template-rows: repeat(2, 1fr); column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div style="grid-area: 1 / 1 / 2 / 2;">

```html {2}
<!-- index.html -->
<div data-controller="counter">
  <span data-target="counter.output"></span>
  <button data-action="click->counter#addOne">
    Add One
  </button>
</div>
```

</div>

<div style="grid-area: 2 / 1 / 3 / 2; ">
  <div data-controller="counter">
    <span data-target="counter.output">3</span>
    <button data-action="click->counter#addOne">
      Add One
    </button>
  </div>
</div>
<div style="grid-area: 1 / 2 / 3 / 3;">

```javascript {1}
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

</div>
</div>

---
<!--
Targets: You can follow the breadcrumb hints as to what might end up being used for something.
-->

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; grid-template-rows: repeat(2, 1fr); column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div style="grid-area: 1 / 1 / 2 / 2;">

```html {3}
<!-- index.html -->
<div data-controller="counter">
  <span data-target="counter.output"></span>
  <button data-action="click->counter#addOne">
    Add One
  </button>
</div>
```

</div>

<div style="grid-area: 2 / 1 / 3 / 2; ">
  <div data-controller="counter">
    <span data-target="counter.output">3</span>
    <button data-action="click->counter#addOne">
      Add One
    </button>
  </div>
</div>
<div style="grid-area: 1 / 2 / 3 / 3;">

```javascript {5,21}
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

</div>
</div>

---
<!--
Actions: Again, you can follow the breadcrumbs & it's easy.

>> Click the button on this one <<
-->

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; grid-template-rows: repeat(2, 1fr); column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div style="grid-area: 1 / 1 / 2 / 2;">

```html {4}
<!-- index.html -->
<div data-controller="counter">
  <span data-target="counter.output"></span>
  <button data-action="click->counter#addOne">
    Add One
  </button>
</div>
```

</div>

<div style="grid-area: 2 / 1 / 3 / 2; ">
  <div data-controller="counter">
    <span data-target="counter.output">3</span>
    <button data-action="click->counter#addOne">
      Add One
    </button>
  </div>
</div>
<div style="grid-area: 1 / 2 / 3 / 3;">

```javascript {15}
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

</div>
</div>

---
<!--
Lots of places, I had lots of wins replacing jQuery on old rails app with it.
-->

# When is it useful?

- A replacement jQuery on older sites
- You want Progressive Enhancement (It'll work without JavaScript, but is enhanced when JavaScript is ready)
- You want a little JavaScript within your app, but want a little bit of organisation to the code
- Small dev team who jump between the frontend & backend

---

# When is it not useful?

- API Driven Apps - It's possible, but react has better toolage
- You're already setup using React/Vue & it's working.
- When no JS would work :D

---
# How can we set it up in Rails!

```bash
$ bundle add webpacker
$ bundle
$ bundle exec rails webpacker:install:stimulus
```

---
<!-- _class: lead -->


# Questions?

@MikeRogers0 on Twitter
