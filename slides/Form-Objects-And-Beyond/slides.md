---
paginate: true
theme: custom-theme
size: 16:9
title: Forms Objects & Beyond
_class: prose
---
<!-- _class: lead -->
<!--
Hello Everyone!

Most of what I do in Rails is just handling form input, E.g. someone wants to create a record in the database &
here are the values.

However, as you grow an app keeping things understandable is hard.

This talk could have been titled "I had to look at lots of rubbish code, and I wish they had done this"
-->

# Forms Objects & Beyond

Techniques for keeping action related code clean, understandable & enjoyable as your app grows.

---
<!-- _class: lead -->
<!--
I'm going to throw some code at you during this talk!

I'll post links afterwards, but open source & written in Markdown.

If you like it, I totally encourage you to take it & do whatever with it :)
-->

# Code Samples

- https://talks.mikerogers.io/
- https://github.com/mikerogers0/talks

---
<!-- _class: lead -->
<!--
The aim of this talk is to give you tools in your pocket on how to solve these more complex forms in a nicer way.
-->

# What We'll Cover

- What the Rails Scaffold gives us, and where it starts getting hard
- Inheriting from a Model
- Service Classes
- Form Objects
- What about Multi-Page forms?
- dry-monads


---
<!-- _class: lead -->
<!--
I'm guessing we've all run the scaffolding generator & had a look at the files?
-->

# Rails Scaffold

```bash {0}
$ bundle exec rails generate scaffold Post title:string body:text status:string
```

---
<!-- _class: lead -->
<!--
This is super nice! When we run this we'll get our model, a controller, a factory & a nice test.
I love this!
-->

# Rails Scaffold

```bash {0}
TODO: List of generated files
```

---
<!-- _class: lead -->
<!--
Here is the model, it's not doing anything to fancy other then making sure we have the fields presence
-->

# Rails Scaffold - Model

```ruby {0}
TODO: A sample of the factory
```

---
<!-- _class: lead -->
<!--
I get a nice Factory, which means I can generate lots of valid states for my object
-->

# Rails Scaffold - Factories

```ruby {0}
TODO: A sample of a factory
```

---
<!-- _class: lead -->
<!--
I love these requests tests! You can handle both a valid submission & an invalid submission. AWESOME!
-->

# Rails Scaffold - The Request Test

```ruby {0}
TODO: A few lines from the request test
```

---
<!-- _class: lead -->
<!--
I like to cut down my views a bit using simple_form, but overall there isn't to much magic here.
-->

# Rails Scaffold - The View

```ruby {0}
TODO: Sample from the view (Maybe a screenshot also)
```

---
<!-- _class: lead -->
<!--
I like my controllers to just have CRUD actions. We could cut this down a bit, but overall it's pretty clear what's going on.
-->

# Rails Scaffold - The Controller

```ruby {0}
TODO: A few lines from the controller create action
```

---
<!-- _class: lead -->
<!--
Overall, I think this is pretty nice! Like a developer could follow the breadcrumbs & would probably know their way around.

However, let's go through some feature requests from a customer & what could happens
-->

# Rails Scaffold - How does this become hard to work with?

Week 1:
👨‍💼: "When we make it send an Email after it's created?"
🧑‍🚀: "Yes!"

---
<!-- _class: lead -->
<!--
So our developer adds a line to the controller, updated the request test and calls it a day.
-->

# Rails Scaffold - How does this become hard to work with?

```ruby {0}
TODO: A few lines from the controller create action with a call to the ActionMailer
```

---
<!-- _class: lead -->
<!--
Then some time passes, and the client wants a new feature.
-->

# Rails Scaffold - How does this become hard to work with?

Week 2:
👨‍💼: "It should make an API call to our CRM system also, but only if the user accepts the Terms of Use (But make it required)!"
🧑‍🚀: "Yes!"
👨‍💼: "Also! Add a comment to the new post _if_ it's the users first post!"
🧑‍🚀: "Great Idea!"

---
<!-- _class: lead -->
<!--
So our developer adds a line to the controller & updates the test, and calls it a day.
-->

# Rails Scaffold - How does this become hard to work with?

```ruby {0}
TODO: A few lines from the controller create action with a call to our ActiveJob, ActionMailer & an extra object creation
```

---
<!-- _class: lead -->
<!--
Then some time passes, the client now has an Admin Panel to create posts & wants some of this in there
-->

# Rails Scaffold - How does this become hard to work with?

Week 16:
👨‍💼: "Why isn't it our Admin Panel for the Post making the API Call to the CRM System?!"
🧑‍🚀: "I can see we're duplicating code, I can refactor this!"

---
<!-- _class: lead -->
<!--
So our developer adds a callback to the model (Maybe a context), and also leaves some stuff in the controller.

Then they add the tests for the model, but don't remove the stuff from the request test from earlier.

So we now in the place where our request tests are testing things, which are more closely related to our model life cycle.
-->

# Rails Scaffold - How does this become hard to work with?

```ruby {0}
TODO: Show our new model, along with the callback. Then some tests
```

---
<!-- _class: lead -->
<!--
This process is then repeated over time, across multiple models & controllers. Maybe some team members are recycled also.

We end up in a scenario where we have lots of business logic split all over our app, but also our tests make this hard to remove.

We also have the scenario where where we have a lot going on when setting up for a small test, and that makes things slower to test.
-->

# Rails Scaffold - How does this become hard to work with?

Week 52:
🧑‍🚀: "Why is this test failing when I change this? Why is it doing this extra stuff!?!"
🧑‍🚀: "I just want to create a post for this test, why is it breaking?"

---
<!-- _class: lead -->
<!--
Lets wind back the clock.

What if we used a service class, so we wrap it in a nice class to do the things!
-->

# Service Class

```ruby {0}
class Post::CreatorService
  def initialize(post:, params:)
    @post = post
    @params = params
  end

  def create_post
    post.attributes = params
    return false unless post.valid?

    post.comments.build(body: "Sample") if post.user.posts.empty?
    post.save
    User::PostMailer.with(post: post).post_created_notification.deliver_later

    if synchronise_to_crm?
      CRMSystem::Post::SynchronisationJob.perform_later(post)
    end

    true
  end

  private
  def synchronise_to_crm?
    params[:accepted_terms_of_use] == "1"
  end
end
```

---
<!-- _class: lead -->
<!--
Which we'd use a bit like this.

This is ok, but my beef with service objects in this space is if we wanted to get nice errors our of this it'll become messy.
-->

# Service Class

```ruby {0}
TODO: Sample of the controller usage
```

---
<!-- _class: lead -->
<!--
Let's try again!

We could inherit from our model, this would work for times when we have action specific logic.

I don't love the callbacks.
-->

# Inheriting from a Model

```ruby {0}
class Post::CreatedByUser < Post
  validates :terms_of_use_accepted, acceptance: true

  before_create :build_comment, if: { post.user.posts.empty? }
  after_create :perform_crm_synchronise
  after_create :deliver_created_notification
end
```

---
<!-- _class: lead -->
<!--
This is actually, super nice! But not perfect
-->

# Inheriting from a Model

```ruby {0}
TODO: Sample of controller using a different model
```

---
<!-- _class: lead -->
<!--
Let's try again!

What if we had an action super specific 
-->

# Form Objects

```ruby {0}
class Post::CreatedByUserForm
  include ActiveModel::Model

  attr_accessor :user
  attr_accessor :terms_of_use_accepted

  validates :terms_of_use_accepted, acceptance: true

  def save
    build_post_comment if user.posts.empty?
    return false unless valid?

    perform_crm_synchronise
    deliver_notification

    true
  end

  def post
    @post ||= user.post.new
  end
end
```

---
<!-- _class: lead -->
<!--
What about multi-page forms?
-->

# What about Multi-Page forms?

```ruby {0}
TODO: Save the object to the DB each step, using form objects to validate each step.
```

---
<!-- _class: lead -->
<!--
Make a project better
-->

# Homework

Let me know how it goes!

---
<!-- _class: lead -->


# Thank you!

[MikeRogers.io](https://mikerogers.io/)
@MikeRogers0 on Twitter
