---
paginate: true
theme: custom-theme
size: 16:9
title: Exercism: Darts
_class: prose
---
<!-- _class: lead -->
<!--
Hello! I'm Mike!

I'm from the UK, you can normally find me online as @MikeRogers0
-->

# Exercism: Darts

By @MikeRogers0

---
<!-- _class: lead -->
<!--
This is the main bit of the task which jumped out to me.

Especially the word "Cartesian (Car-tea-shin) Coordinates". I think that was keyword hint, but I kind of ignored it.

-->

# What are we trying to solve?

> Write a method that, given a point in the target (defined by its real _Cartesian coordinates_ x and y), returns the correct amount earned by a dart landing in that point.

---
<!--
So what are we trying to do?

We're going to be given coordinates, we need to return a score.
-->

# What are we trying to solve?

- We're going to be given a set of coordinates
- We have to return the score based on how close that is to the target (`0,0`)
  - outside the target: 0 points.
  - in the outer circle of the target: 1 point.
  - in the middle circle of the target: 5 points.
  - in the inner circle of the target: 10 points.

---
<!--

I thought the best place to start would be by getting the correct score given just one coordinate. 

So I just passed in X, looped though the radius-to-score mapping returning the source.
-->

# Let's start with the x coordinate

```ruby
class Darts
  RADIUS_SCORE_MAP = { 1 => 10, 5 => 5, 10 => 1 }

  def initialize(x)
    @x = x.to_i
  end

  def score
    RADIUS_SCORE_MAP.each do |radius, map_score|
      return map_score if radius >= @x
    end
    0
  end
end
```

---
<!--
That actually worked out ok.

I thought I was on the right track to solving this.
-->

# Let's start with the x coordinate

```ruby
Darts.new(0).score # Returns 10
Darts.new(2).score # Returns 5
Darts.new(9).score # Returns 1
Darts.new(11).score # Returns 0
```

---
<!--
So I thought the right way I could get the correct was to just to pick the biggest of X & Y, then get the score from that.

I messed about with a few coordinates like:

x = 9, y = 0
x = 1, y = 1
&
x = 0, y = 2

It look like it was working. I was feeling pretty good.
-->

# Awesome now both coordinates

What if I used the biggest of the two coordinates?


```ruby {6}
class Darts
  def initialize(x, y)
  # [..]

  def score
    RADIUS_SCORE_MAP.each do |radius, map_score|
      return map_score if radius >= [@x, @y].max
    end
    0
  end
end
```

---
<!--
Spoiler: That was not the right solution.

I then came back the term:
"Cartesian (Car-tea-shin) Coordinates"

I searched on Wikipedia, ended up on a page with diagram of a circle.
-->

# Awesome now both coordinates

```bash
$ ruby darts_test.rb
Run options: --seed 54261

# Running:
..F..F.F..FFF

Finished in 0.001701s, 7642.5632 runs/s, 7642.5632 assertions/s.
13 runs, 13 assertions, 6 failures, 0 errors, 0 skips
```

<center class="text-2xl">
🤔🤔🤔🤔
</center>

---
<!--
Then I realised my mistake, and I drew it onto a graph to emphasise it a bit better.

If we look at 9,-9.
My "take the biggest coordinates" approach wasn't so full proof. Obviously it would have worked if these were squares.
-->

<center>
  <img src="/Exercisms/Darts/images/mapped_points.svg" />
</center>

---
<!--
What we need to do is figure is if the point is within one of the circles.

This distance, the black line
-->

<center>
  <img src="/Exercisms/Darts/images/mapped_points-with-line.svg" />
</center>

---
<!--
Luckily we know the distance these two lines, which gives us something which looks
suspiciously like a triangle.
-->

<center>
  <img src="/Exercisms/Darts/images/mapped_points-with-triangle.svg" />
</center>

---
<!--
We need to use Pythagorean theorem!
-->
<!-- _class: lead -->

# Pythagorean theorem

a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup>

<center style="margin-top: 4rem;">
  <video preload="auto" autoplay="true" loop="true" playsinline="" aria-label="Adventure Time Finn GIF" src="/Exercisms/Darts/images/mathematical.mp4" type="video/mp4" width="400" height="240"></video>
</center>

---
<!--
I did have to search how to do "To the power of" & "Square root" in ruby.

While searching Stackoverflow, I found:
-->

# Pythagorean theorem

```ruby
class Darts
  # [..]

  def hypotenuse
    Math.sqrt(@x**2 + @y**2)
  end
end

Darts.new(0, 10).hypotenuse # 10.0
Darts.new(-5, 0).hypotenuse # 5.0
Darts.new(9, -9).hypotenuse # 12.727922061357855
```

---
<!--
I found out yesterday, there is actually a Math.hypot method to do this!
-->

# Pythagorean theorem

```ruby
class Darts
  # [..]

  def hypotenuse
    Math.hypot(@x, @y)
  end
end

Darts.new(0, 10).hypotenuse # 10.0
Darts.new(-5, 0).hypotenuse # 5.0
Darts.new(9, -9).hypotenuse # 12.727922061357855
```

<center>
🥳
</center>

---
<!--
My full code looked a bit like:

- I'm not fan about how I'm just setting x & z in the initialize

- I don't really like how I calculated the score, or the "hypotenuse_score_map" variable naming.

-- If I was to add a new score circle in & put it in the wrong place, it would probably break.

-->

```ruby
class Darts
  HYPOTENUSE_SCORE_MAP = { 1 => 10, 5 => 5, 10 => 1 }

  def initialize(x, y)
    @x = x
    @y = y
  end

  def score
    HYPOTENUSE_SCORE_MAP
      .select { |radius, _map_score| radius >= hypotenuse }
      .values
      .first || 0
  end

  private

  def hypotenuse
    Math.hypot(@x, @y)
  end
end

```

---

<!--
However! The tests were passing, so I was happy enough to submit it.

I did run the tests a bunch & look what happens if I was to calculate the hypotenuse in the initialize, but that ended up being an exceptionally tiny difference.
-->

# All the tests pass


```bash
$ ruby darts_test.rb  
Run options: --seed 33650

# Running:

.............

Finished in 0.001264s, 10284.8101 runs/s, 10284.8101 assertions/s.
13 runs, 13 assertions, 0 failures, 0 errors, 0 skips
```

<center>
🥳🥳🥳
</center>
