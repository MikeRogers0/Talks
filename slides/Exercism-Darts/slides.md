---
paginate: true
theme: custom-theme
size: 16:9
title: Exercism: Darts
_class: prose
---

<!-- _class: lead -->

# Exercism: Darts

By @MikeRogers0

---
<!-- _class: lead -->

# What are we trying to solve?

> Write a method that, given a point in the target (defined by its real Cartesian coordinates x and y), returns the correct amount earned by a dart landing in that point.


---
<!--

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
I wanted to find a way to return the correct score based on
a single coordinate.
I even converted it to an integer to make it easier on myself.
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
That worked as we hoped!
-->

# Let's start with the x coordinate

```ruby
Darts.new(0).score # Returns 10
Darts.new(2).score # Returns 5
Darts.new(9).score # Returns 1
Darts.new(11).score # Returns 0
```

---

# Awesome now both coordinates

I wrote some code to pick the highest of the two coordinates, then return the score:


```ruby {6}
class Darts
  # [..]

  def score
    RADIUS_SCORE_MAP.each do |radius, map_score|
      return map_score if radius >= highest_coordinate
    end
    0
  end
end
```

---

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
What was I doing wrong?
Let's draw it out!

If we look at 9,-9 my "take the biggest coordinates" wasn't so full proof. It would have worked if these were squares.

What we need to do is figure is if the point is within one of the arches.
-->

<center>
  <img src="/Exercism-Darts/images/mapped_points.svg" />
</center>

---
<!--
This distance.
-->

<center>
  <img src="/Exercism-Darts/images/mapped_points-with-line.svg" />
</center>

---
<!--
Oh, a triangle!
-->

<center>
  <img src="/Exercism-Darts/images/mapped_points-with-triangle.svg" />
</center>

---
<!--
We need to use Pythagorean theorem!
-->
<!-- _class: lead -->

# Pythagorean theorem

a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup>

<center style="margin-top: 4rem;">
  <video preload="auto" autoplay="true" loop="true" playsinline="" aria-label="Adventure Time Finn GIF" src="/Exercism-Darts/images/mathematical.mp4" type="video/mp4" width="400" height="240"></video>
</center>

---
<!--
We need to use Pythagorean theorem!
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
We need to use Pythagorean theorem!
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
We need to use Pythagorean theorem!
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
    Math.sqrt(@x**2 + @y**2)
  end
end

```

---

# The which made all the tests pass


```bash
$ ruby darts_test.rb  
Run options: --seed 33650

# Running:

.............

Finished in 0.001264s, 10284.8101 runs/s, 10284.8101 assertions/s.
13 runs, 13 assertions, 0 failures, 0 errors, 0 skips
```
