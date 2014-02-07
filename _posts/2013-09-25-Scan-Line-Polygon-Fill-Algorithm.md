---
layout: post
summary: Colouring in lessons, for your computer.
---

The scan-line polygon fill algorithm is used to colour in any closed shape.
The algorithm uses the concept of scan lines, being the horizontal lines that make up a digital display.

Two data structures are central to the algorithm: an 'all edges table' (`ET`), and an 'active edge table' (`AET`).
`ET` stores a list of the edges defining the polygon to colour in, whereas `AET` holds the edges that intersect with the current scan line.
All edges start in `ET`, and move to `AET` as they are 'hit' by the scan line.

### Initialising Variables ###

#### Initialising the Scan Line ####

Scan lines are typically drawn from the bottom to the top of the screen.
So, simply initialise the scan line to the lowest y-value 'touched' by the polygon (i.e., the minimum y-value of the first entry in the sorted `ET`).


#### Initialising ET ####

For each non-horizontal edge defining the outline of the polygon, store:

* the minimum y-value of the two endpoints
* the maximum y-value of the two endpoints
* the x-value that corresponds with the minimum y-value
* `1/m`, where `m` is the gradient of the edge

Sort `ET` in ascending order, by minimum y-value.
If two edges have the same minimum y-value, sort them in ascending order by x-value.

We do this so that we can efficiently move entires from `ET` to `AET`.
Because the scan line starts at the bottom of the screen and moves up, we can trigger the movement of an edge from one table to another when the scan line y-value is equal to the minimum y-value for an edge.
If the edges are sorted by minimum y-value, this can be done a lot faster.

We ignore horizontal edges to ensure they get drawn.
If they were left in `ET`, they would trigger a parity change at every step (see 'Parity', below), hence would break the drawing algorithm.

#### Initialising AET ####

To initialise `AET` simply move all edges  with the lowest minimum y-value from `ET` to `AET`.
As `ET` is sorted, this should not be an expensive operation.

#### Parity ####

In order to know when to colour pixels, the algorithm uses the concept of 'parity'.
A variable `parity` is defined, and initialised to `even`.

At each step on the scan line (from left to right), the value of parity is checked.
If parity is odd, the current pixel is coloured; if parity is even, the pixel is left alone.

As a scan line is drawn, collisions with a polygon edge trigger a reversal of parity (i.e., even to odd, odd to even).
This means that with parity initialised to even, the first collision with the polygon (i.e., when the scan line 'enters' the polygon) will start drawing, by flipping parity to odd.
The second collision (i.e., when the scan line 'leaves' the polygon) will stop drawing, by changing parity back to even.

### Algorithm Steps ###

After initialisation, the following steps describe the fill algorithm:

1. Colour all pixels between pairs in the `AET`.

	For example, if the `AET` contained four entries, with x-values `[1, 7, 8, 19]`, the pixels in the range `x = [1, 7)` and `x = [8, 19)` should be coloured.
	As the scan line is horizontal, its current y-value can be used.

2. Increase the scan line y-value by 1 (i.e., move the scan line up).

3. Remove any edges in the `AET` where the maximum y-value is equal to the current scan line y-value.

4. Update the x-intercepts for each edge in the `AET`.

	For each edge in the `AET`, the x-value represents the x-coordinate at which the scan line will intercept that edge.
	So, initially `x` is set to the coordinate corresponding to the minimum value of `y` for that edge, as that is when the edge will first intercept the scan line.

	However, as the scan line moves up we need to update the value of `x` accordingly.

	We can figure this out by using the equation for the gradient of a line, `m`, at the `k`th point in that line:

		m = (y(k + 1) - y(k)) / (x(k + 1) - x(k))

	Because the scan line always moves up by `1` each iteration, we know that:

		y(k + 1) = y(k) + 1

	By substituting this into the equation for `m`, we can find the general formula for incrementing `x`:

		m = (y(k + 1) - y(k)) / (x(k + 1) - x(k))
		  = (y(k) + 1 - y(k)) / (x(k + 1) - x(k))
		  = 1 / (x(k + 1) - x(k))

		m * (x(k + 1) - x(k)) = 1
		x(k + 1) - x(k) = 1 / m
		x(k + 1) = x(k) + (1 / m)

	So, to increment `x`, we add `1 / m` to it's current value.
	Conveniently, this value is stored in `AET`.

5. Move any edges from `ET` to `AET` if the scan line y-value is equal to the edge's minimum y-value.

6. Sort `AET` by x-value to avoid complications caused by crossing lines.

