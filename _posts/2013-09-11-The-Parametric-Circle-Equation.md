---
layout: perma_post
summary: Explains how to use the parametric circle equation to draw a circle.
---

The parametric circle equation is a simple but expensive circle drawing algorithm.

![Illustration of right angled triangle within a circle.]({{site.url}}/img/parametric_circle.png)

Consider the above image.
Given a radius `R`, and an angle `t`, the coordinates of point `P`, `(x, y)` can be calculated.

Using trigonometry:

	sin(t) = y / R
	y = R * sin(t)

	cos(t) = x / R
	x = R * cos(t)

Using these equations, the coordinates for any point on the circle can be calculated, given an angle `t`.
The general process followed by the algorithm is to calculate a number of points on the circle, joining each subsequent point to the last calculated (i.e., connect the dots to make the overall shape).

The only question left is what the value of `t` should be.

The simplest method is to simply calculate the `x` and `y` values for _every_ possible value of `t` (i.e., for every whole number in the range `[0, 360]`).
However, this is an overly expensive method, especially for smaller circles.

An alternative is to define a step size, `step`.
`t` can then be initialised to zero, and incremented by `step` each iteration.

Generally, the value of `step` is calculated in terms of a number of desired points.
This allows the programmer to easily set the 'smoothness' of the circle whilst also ensuring each line segment used to draw the circle is of an equal length.
The equation for this is as follows:

	step = (2 * pi) / num_points

`2 * pi` is used due to the fact that computers work in radians, and `2 * pi` in radians is equal to `360` degrees.

Thus far the algorithm will work for a circle of any radius, but assumes that the centre point is `(0, 0)`.
Drawing a circle at a midpoint defined by `(midX, midY)` is a simple matter of offsetting the `x` and `y` coordinates, giving the final equations for `x` and `y`:

	y = midY + (R * sin(t))

	x = midX + (R * cos(t))

## Algorithm ##

The parametric circle drawing algorithm is as follows:

	step = (2 * pi) / num_points

	StartPolygon();
	for (t = 0; t < (2 * pi); t += step):
		x = midX + (R * cos(t))
		y = midY + (R * sin(t))

		addPoint(x, y);

	EndPolygon();

The algorithm assumes a method `addPoint(x, y)` is available, which adds a coordinate `(x, y)` to a list of points defining a polygon.
Also assumed is the existence of methods `StartPolygon()` and `EndPolygon()`, which indicate an area of code in which any points added related to the same geometric shape.

