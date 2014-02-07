---
layout: post
summary: Deriving the midpoint circle drawing algorithm (an extension to Bresenham's line algorithm) with optimisations to avoid floating point calculations.
---

The midpoint circle drawing algorithm is a graphics algorithm for approximating the pixels needed to draw a circle given a radius and a centre coordinate.
It is an extension to [Bresenham's line algorithm](http://malformed-bits.com/2013/09/08/Deriving-Bresenham%27s-Line-Algorithm.html).

The algorithm calculates all points for the circle in the first (i.e., the north to north-east) [octant](http://en.wikipedia.org/wiki/Octant_(plane_geometry)).
All remaining points can be found by translating these coordinates.

![First octant]({{site.url}}/img/octant.png)

To skip straight to the final algorithm, scroll down.
Alternatively, continue on to the derivation.

## Derivation ##

The equation of a circle in terms of `x`, `y` and a radius , `r` is:

	square(x) + square(y) = suqare(r)

This equation can be transformed into:

	f(x, y) = 0 = square(x) + square(y) - suqare(r)

It follows that for any pair `(x, y)`:  
if `(x, y)` is __on__ the line, `f(x, y) = 0`  
if `(x, y)` is __off__ the line, `f(x, y) != 0`  

* if `f(x, y) < 0`, `(x, y)` is on the '*negative* half-plane'
* if `f(x, y) > 0`, `(x, y)` is on the '*positive* half-plane'

To simplify the problem, we initially assume that the centre of the circle to be drawn is at `(0, 0)`.
The real centre points can be reintroduced later as an offset to the final result.

Starting from the point directly above the centre (i.e., `(0, R)`), we move clockwise calculating points on the circle until we reach a 45^o angle (i.e., when `x = y`).
This is illustrated in the image above.

Within this area, `x` will _always_ increment by 1.
We need to figure out if `y` will move _across_ (i.e.,  if `y = y`), or if `y` will move _across and down_ (i.e., if `y = y - 1`).
Selecting between these two candidates is the same as in Bresenham's algorithm: we calculate the value of `f(x, y)` at a midpoint `M`.
The value of `M` is determined in terms of the last-calculate point on the line, `(xp, yp)`.

	M = (xp + 1, yp - 0.5)

If `f(M) < 0`, `M` is in the negative half-plane of the line.
This means `M` is _below_ the line, indicating that the line is closer to the _upper_ candidate point (i.e., where `y` doesn't change).
Hence, we choose the candidate `(xp + 1, yp)` as the next point on the line.

If `f(M) > 0`, `M` is in the positive half-plane of the line.
This means `M` is _above_ the line, indicating that the line is closer to the _lower_ candidate point (i.e., where `y` changes).
Hence, we choose the candidate `(xp + 1, yp - 1)` as the next point on the line.

In other words:

* if `f(M) < 0`, `y = y`
* if `f(M) > 0`, `y = y - 1`

To avoid having to calculate `f(M)` all the time, we introduce the notion of a decision variable.

The 'decision' variable, `D`, keeps track of the current value of `f(M)`.
Its initial value can be calculated as the difference between the first midpoint, `M`, and the starting coordinate in our circle, `(0, R)`.

	D = f(M) - f(0, R)
	  = f(0 + 1, R - 0.5) - f(0, R)
	  = f(1, R - 0.5) - f(0, R)
	  = square(1) + square(R - 0.5) - square(R) - [square(0) + square(R) - square(R)]
	  = square(1) + square(R - 0.5) - square(R)
	  = 1 + square(R - 0.5) - square(R)
	  = 1 + square(R) - 0.5R - 0.5R + 0.25 - square(R)
	  = 1 + square(R) - 0.5R - 0.5R + 0.25 - square(R)
	  = 5/4 - R

`D` can then be used to determine which candidate point to select next, using the same logic as earlier:

* if `D > 0`, select `(xp + 1, yp)`
* if `D < 0`, select `(xp + 1, yp - 1)`

As we calcuate points on the line, we need to update the value of `D` to correspond to the next midpoint.
The way in which the value changes depends on which candidate point was chosen (i.e., if `y` was changed or not).

We can define the midpoint at step `k` as being:

	midpoint(k) = (x(k) + 1, y(k) - 0.5)

It follows that the midpoint at step `k + 1` is then:

	midpoint(k + 1) = (x(k + 1) + 1, y(k + 1) - 0.5)

Because we know that `x` will _always_ be incremented, we know that `x(k + 1) = x(k) + 1`.
We can use this knowledge to simplify the equation for `(midpoint(k + 1))` as follows:

	midpoint(k + 1) = (x(k) + 1 + 1, y(k + 1) - 0.5)

To calculate the general increment to apply to the decision variable `D`, `dInc`, we need to calculate the difference between `f(midpoint(k + 1))` and `f(midpoint(k))`.

	dInc = f(midpoint(k + 1)) - f(mipdoint(k))
		 = square(x(k) + 1 + 1) + square(y(k + 1) - 0.5) - square(R) - [square(x(k) + 1) + square(y(k) - 0.5) - square(R)]
		 = square(x(k) + 2) + square(y(k + 1) - 0.5) - square(R) - square(x(k) + 1) - square(y(k) - 0.5) + square(R)
		 = square(x(k) + 2) + square(y(k + 1) - 0.5) - square(x(k) + 1) - square(y(k) - 0.5)

We then need to substitute each possible value of `y(k + 1)` to find the two potential increments to `D`.

So, if `y` __was not incremented__:

	y(k + 1) = y(k)

	dInc = square(x(k) + 2) + square(y(k) - 0.5) - square(x(k) + 1) - square(y(k) - 0.5)
	     = square(x(k) + 2)  - square(x(k) + 1)
		 = square(x(k)) + 2x(k) + 2x(k) + 4 - (square(x(k)) + x(k) + x(k) + square(1))
		 = square(x(k)) + 4x(k) + 4 - (square(x(k)) + 2x(k) + 1)
		 = square(x(k)) + 4x(k) + 4 - square(x(k) - 2x(k) - 1)
		 = 4x(k) + 4 - 2x(k) - 1)
		 = 2x(k) + 3

If `y` __was incremented__:

	y(k + 1) = y(k) - 1

	dInc = square(x(k) + 2) + square(y(k) - 1 - 0.5) - square(x(k) + 1) - square(y(k) - 0.5)
	     = square(x(k) + 2) + square(y(k) - 1.5) - square(x(k) + 1) - square(y(k) - 0.5)
	     = square(x(k)) + 2x(k) + 2x(k) + 4 + square(y(k)) - 1.5y(k) - 1.5y(k) + 2.25 - (square(x(k)) + x(k) + x(k) + 1) - (square(y(k)) - 0.5y(k) - 0.5y(k) + 0.25)
	     = square(x(k)) + 2x(k) + 2x(k) + 4 + square(y(k)) - 1.5y(k) - 1.5y(k) + 2.25 - square(x(k)) - x(k) - x(k) - 1 - square(y(k)) + 0.5y(k) + 0.5y(k) - 0.25
	     = square(x(k)) + 4x(k) + 4 + square(y(k)) - 3y(k) + 2 - square(x(k)) - x(k) - x(k) - 1 - square(y(k)) + y(k)
	     = square(x(k)) + 4x(k) + 5 + square(y(k)) - 3y(k) - square(x(k)) - x(k) - x(k) - square(y(k)) + y(k)
	     = 4x(k) + 5 - 3y(k) - x(k) - x(k) + y(k)
	     = 2x(k) + 5 - 2y(k)
	     = 2(x(k) - y(k)) + 5

So, during each iteration of the algorithm, based on whether `y` was decremented or not, we increment `D` by either `2x(k) + 3` or `2(x(k) - y(k)) + 5`.

## Algorithm ##

Assuming the existence of a method `setPixel(x, y)`, which colours a pixel at the location `(x, y)`, the following algorithm represents the midpoint circle drawing algorithm for the first octant, given a radius `R`, and a centre point `(midX, midY)`.

	d = (5 / 4) - R

	y = R

	for (x = 0; x < y; ++x):
		setPixel(x + midX, y + midY) # offset the pixel according to the circle centre

		if (d < 0):
			d += (2 * x) + 3
		else:
			d += 2 * (x - y) + 5
			y -= 1

However, there are still some floating point calculations in the algorithm, specifically in the initialisation of `d`.
To avoid this, we can multiply all values of `d` by `4`, including the initialisation of `d` and the increments to `d`.

We are also only drawing the pixels for the first octant.
This can be fixed by translating the `x` and `y` coordinates to cover all octants.

Applying these two changes results in the final optimised algorithm:

	d = 5 - (4 * R)

	y = R

	for (x = 0; x < y; ++x):
		setPixel(x + midX, y + midY) # offset the pixel according to the circle centre
		# colour the pixels in the other 7 octants
		setPixel(x + midX, -y + midY) # offset the pixel according to the circle centre
		setPixel(-x + midX, y + midY) # offset the pixel according to the circle centre
		setPixel(-x + midX, -y + midY) # offset the pixel according to the circle centre
		setPixel(y + midX, x + midY) # offset the pixel according to the circle centre
		setPixel(y + midX, -x + midY) # offset the pixel according to the circle centre
		setPixel(-y + midX, x + midY) # offset the pixel according to the circle centre
		setPixel(-y + midX, -x + midY) # offset the pixel according to the circle centre

		if (d < 0):
			d += 4 * ((2 * x) + 3)
		else:
			d += 4 * (2 * (x - y) + 5)
			y -= 1

