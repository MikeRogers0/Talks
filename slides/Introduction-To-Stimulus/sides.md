---
paginate: true
theme: custom-theme
size: 16:9
title: An Introduction To Stimulus.js
_class: prose
---
<!-- _class: lead -->

<img src="/Introduction-To-Stimulus/images/stimulus.svg" width="100" height="100" style="margin: 0 auto;" />

# An Introduction To Stimulus.js

What is it & when is it useful?

---

<!--
Online I go by @MikeRogers0, normally I have a fairly dull setup which is a pretty enjoyable way to work.
-->

# Who am I?

I'm Mike Rogers! Here is my normal stack:

<div style="display: flex; justify-content: space-around; font-size: 1.2rem; margin-top: 5rem;">
  <img src="/Introduction-To-Stimulus/images/ruby-on-rails.svg" width="200" height="100" />
  <img src="/Introduction-To-Stimulus/images/stimulus.svg" width="100" height="100" />
  <img src="/Introduction-To-Stimulus/images/docker-icon.svg" width="200" height="125" />
</div>
<div style="display: flex; justify-content: space-around; font-size: 1.2rem; margin-top: 2rem;">
  <img src="/Introduction-To-Stimulus/images/sidekiq-logo-png-transparent.png" width="100" height="100" />
  <img src="/Introduction-To-Stimulus/images/heroku-logo-stroke-purple.svg" width="100" height="100" />
  <img src="/Introduction-To-Stimulus/images/postgresql.png" width="100" height="100" />
</div>

---
<!--
- Here is what I'm aiming to cover
- The idea of this talk is give you an idea of the use cases where it's useful, what it's best for
- But also where it's not the best choice.
-->

# What we'll cover

- Where does Stimulus Come From?
- What does it look like?
- How can we set it up in Rails!
- When is it useful?

The outcome should be you'll confident enough to give Stimulus a try & know about where it's useful.

---
<!--
- For me, Stimulus reached my radar when it was added to Webpacker
- So I typed 'rails webpacker' one day, and was like "What are these options?"
- So I kind looked at all the options
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
<!--
Not to unapproachable
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
    <span data-target="counter.output">12</span>
    <button data-action="click->counter#addOne">
      Add One
    </button>
  </div>

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

<div style="display: grid; grid-template-columns: 40% 60%; column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div>

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
<div>

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

<div style="display: grid; grid-template-columns: 40% 60%; column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div>

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
<div>

```javascript {5}
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
-->

# What does it look like?

<div style="display: grid; grid-template-columns: 40% 60%; column-gap: 1rem; row-gap: 1rem; font-size: 1.2rem;">
<div>

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
<div>

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

# Blank Element

---

# How can we set it up in Rails!

```bash
$ bundle add webpacker
$ bundle
$ bundle exec rails webpacker:install:stimulus
```
