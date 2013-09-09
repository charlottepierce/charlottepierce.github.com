---
layout: perma_post
summary: Deriving Bresenham's line algorithm with optimisations to avoid floating point calculations.
---

Bresenham's line algorithm is a graphics algorithm for approximating a straight line between two end-points.
Its popularity over other such algorithms is in its efficiency, as it avoids expensive floating point calculations.

The 'plain' algorithm only works for lines with a gradient in the range `[0, 1]` (i.e., non-steep lines; where the change in `x`, `dx`, is greater than the change in `y`, `dy`).
The algorithm is written in the context of a graphics system where there origin, `(0, 0)`, is located in the *top-left corner*.
However, no changes need to be made for graphics systems where the origin is at the bottom-left.
It is also assumed that the starting point of the line, `x0`, is to the left (i.e., is less than) the endpoint.

To skip straight to the final algorithm, scroll down.
Alternatively, continue on to the derivation.

## Derivation ##

The equation of a line in terms of `y` can be defined as:

	y = f(x) = mx + b

Where `m` is the slope of the line, and `b` is the y-intercept.

This equation can be transformed, as follows:

	y = (dy/dx)x + b
	(dx * y) = (dy * x) + (dx * b)
	0 = (dy * x) - (dx * y) + (dx * b)

	f(x, y) = 0 = Ax + By + C
	where
		A = dy
		B = -dx
		C = (dx * b)

It follows that for any pair `(x, y)`:  
if `(x, y)` is __on__ the line, `f(x, y) = 0`  
if `(x, y)` is __off__ the line, `f(x, y) != 0`  

* if `f(x, y) < 0`, `(x, y)` is on the '*negative* half-plane'
* if `f(x, y) > 0`, `(x, y)` is on the '*positive* half-plane'

From the previously calculated point `(xp, yp)`, we need to determine if the next point on the line is at `(xp + 1, yp)` or `(xp + 1, yp)`.
That is to say, we need to find out if `y` moves *across* or *down and across*.

__Note:__ in cases where the origin is at the *bottom-left*, this would be testing whether `y` moved *across* or *up and across*.

We can choose between these two options by testing the value of the line, `f(x, y)`, in terms of a midpoint, `M`, which is located half-way between the two candidate points.

	M = (xp + 1, yp + 0.5)

	f(M) = f(xp + 1, yp + 0.5)

If `f(M) > 0`, select the candidate `(xp + 1, yp + 1)`, because the midpoint is in the positive plane, hence the line must pass *under* the midpoint, meaning is it closer to this option.

If `f(M) < 0`, select the candidate `(xp + 1, yp)`, because the midpoint is in the negative plane, hence the line must pass *over* the midpoint, meaning it is closer to this option.

![Illutration of candidate selection using midpoints]({{site.url}}/img/bresenhams.png)

To avoid having to calculate `f(M)` all the time, we introduce the notion of a decision variable.

The 'decision' variable, `D`, keeps track of the current value of `f(M)`.
Its initial value can be calculated as the difference between the value of `f(x, y)` at the first end point of the line (x0, y0), and the first midpoint:

	D = f(x0 + 1, y0 + 0.5) - f(x0, y0)

Using the line equation, `f(x, y) = 0 = Ax + By + C`, this can be simplified to:

	D = [A(x0 + 1) + B(y0 + 0.5) + C] - [Ax0 + By0 + C]
	  = [A(x0 + 1) + B(y0 + 0.5) + C] - Ax0 - By0 - C
	  = Ax0 + A + By0 + 0.5B + C - Ax0 - By0 - C
	  = Ax0 -Ax0 + A + By0 - By0 + 0.5B + C - C
	  = A + 0.5B

`D` can then be used to determine which candidate point to select next, using the same logic as earlier:

* if `D > 0`, select `(xp + 1, yp)`
* if `D < 0`, select `(xp + 1, yp + 1)`

As we calcuate points on the line, we need to update the value of `D` to correspond to the next midpoint.
The way in which the value changes depends on which candidate point was chosen (i.e., if `y` was incremented or not).

We can calculate the increment for `D`, `dInc`, as a general increment based on the last midpoint.
To make the calculations easier, we'll do this in terms of the first and second midpoints, `M1` and `M2`, where `dInc = f(M2) - f(M1)`.

Regardless of the first 'direction' chosen for `y`, the first midpoint, `M1`, can be defined as:

	M1 = (x0 + 1, y0 + 0.5)

The value of `M2`, however, depends on the direction chosen.

So, if `y` __was not incremented__:

	M2 = (x0 + 2, y0 + 0.5)

	dInc = f(x0 + 2, y0 + 0.5) - f(x0 + 1, y0 + 0.5)
	     = [A(x0 + 2) + B(y0 + 0.5) + C] - [A(x0 + 1) + B(y0 + 0.5) + C]
	     = Ax0 + 2A + By0 + 0.5B + C - [Ax0 + A + By0 + 0.5B + C]
	     = Ax0 + 2A + By0 + 0.5B + C - Ax0 - A - By0 - 0.5B - C
	     = Ax0 - Ax0 + 2A - A + By0 - By0 + 0.5B - 0.5B + C - C
	     = A

Which, going back to the original definitions of `A`, `B`, and `C`, is equal to `dy`.

If `y` __was incremented__:

	M2 = (x0 + 2, y0 + 1.5)

	dInc = f(x0 + 2, y0 + 1.5) - f(x0 + 1, y0 + 0.5)
	     = [A(x0 + 2) + B(y0 + 1.5) + C] - [A(x0 + 1) + B(y0 + 0.5) + C]
	     = Ax0 + 2A + By0 + 1.5B + C - [Ax0 + A + By0 + 0.5B + C]
	     = Ax0 + 2A + By0 + 1.5B + C - Ax0 - A - By0 - 0.5B - C
	     = Ax0 - Ax0 + 2A - A + By0 - By0 + 1.5B - 0.5B + C - C
	     = A + B

Which, going back to the original definitions of `A`, `B`, and `C`, is equal to `dy + (-dx)`, which is `dy - dx`.

So, during each iteration of the algorithm, based on whether `y` was incremented or not, we increment `D` by either `dy` or `dy - dx`.

## Algorithm ##

Assuming the existence of a method `setPixel(x, y)`, which colours a pixel at the location `(x, y)`, the following algorithm is the general form of Bresenham's line algorithm for drawing a straight line between two points `(x0, y0)` and `(x1, y1)`:

	dx = x1 - x0
	dy = y1 - y0

	a = dy
	b = -1 * dx
	# do not need to calculate the value of 'c', as it is never used

	d = a + 0.5b
	dInc = a # increment for 'd' if 'y' was not incremented
	yInc_dInc = a + b # increment for 'd' if 'y' was incremented

	y = y0
	for (x = x0; x <= x1; ++x):
		setPixel(x, y)
		# increment 'y' and update decision variable as appropriate
		if d > 0:
			y += 1
			d += yInc_dInc
		else:
			d += dInc

However, notice that there is still some floating point calculations, specifically in the initialisation of `d`.
To avoid this we can multiply all values of `d` by `2`.
This includes the initialisation of `d`, and all potential increments to `d`.

We also need to handle lines which fall outside of the constraints defined earlier, being that the line must not be too steep, and that `x0 < x1`.
Applying both of these changes, we get the final algorithm, which handles all cases, using only integers:

	dx = x1 - x0
	dy = y1 - y0

	isSteep = abs(dy) > abs(dx) # check if the line is steeper than a slope with a graident of [0, 1]

	if isSteep:
		# line is steep, swap the roles (i.e., values) of x and y
		swap(x0, y0)
		swap(x1, y1)

	if x0 > x1:
		# start point isn't to the left - swap the end points
		swap(x0, x1)
		swap(y0, y1)

	a = dy
	b = -1 * dx
	# do not need to calculate the value of 'c', as it is never used

	d = 2(a + 0.5b)
	dInc = 2 * a # increment for 'd' if 'y' was not incremented
	yInc_dInc = 2 * (a + b) # increment for 'd' if 'y' was incremented

	y = y0
	for (x = x0; x <= x1; ++x):
		if isSteep:
			setPixel(y, x) # swap the x and y values back so the line will draw properly
		else:
			setPixel(x, y)

		# choose next value for 'y' and update decision variable as appropriate
		if d > 0:
			y += 1
			d += yInc_dInc
		else:
			d += dInc

