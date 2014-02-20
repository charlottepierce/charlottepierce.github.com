---
layout: post
title: Intro to L-Systems
summary: Basic concepts behind Lindenmayer systems.
---

L-systems are a type of formal language.
Introduced by [Aristid Lindenmayer](http://en.wikipedia.org/wiki/Aristid_Lindenmayer) in 1968 to study the development of simple organisms, they evolved over time to model larger structures such as plants.

The concept centres on __rewriting__, where complex objects or shapes are described as an evolution of a simple initial object.
Typically this approach requires two elements:

* an _initiator_: the initial object or shape
* a set of _productions_: the rules for evolving the initiator

The productions are sometimes referred to as _rewriting rules_, as they literally define how the initial shape is rewritten over time.

## The Snowflake Curve ##

One of the most well-known examples of a shape made using rewriting rules is the __Koch Construction__, or snowflake curve.
Proposed by [Helge von Koch](http://en.wikipedia.org/wiki/Helge_von_Koch) in 1905, the initiator is evolved by recursively replacing each straight line with a second shape, known as a _generator_.
This is similar to the concept of productions, as with both methods the original shape is effectively rewritten with increasing complexity.
However, unlike l-systems the the generator must implicitly be arranged (i.e., rotated and resized) so that its end points match those of the straight line it is replacing.

![Basic evolution of the Snowflake curve]({{site.url}}/img/lsystems/koch_construction.png)

## String Rewriting ##

Both the initiator and productions are represented as simple strings, where each character represents a specific drawing command.
As such, each evolution of a shape - beginning with the initiator - is literally a set of instructions describing how it can be drawn.

Productions are written in the form `a -> X`, meaning the sequence `X` should replace every instance of the command `a`, where `a` is a _single_[^1] drawing command.
When evolving a shape each production is applied in parallel.
For example, starting with the initiator `b` and the productions

* `a -> ab`
* `b -> a`

the following evolution would take place:

	b
	a
	ab
	aba
	abaab
	abaababa

## Drawing  ##

The drawing instructions used by the l-system grammer as described below are for a [turtle graphics](http://en.wikipedia.org/wiki/Turtle_graphics) system.
A _turtle_ object is given a state consisting of:

* position
* heading (i.e., direction it is facing)
* step size, `d`
* angle increment, `a`

After being initialised with a position and direction, the turtle can be instructed to move around a canvas either marking the 'page' or leaving it blank.

### Commands ###

The following commands can be used to 'instruct' a turtle, forming the vocabulary of l-systems.

* `F`: move forward by length `d`
* `f`: move forward by length `d` without drawing anything
* `+`: rotate left by angle `a`
* `-`: rotate right by angle `a`
* `[`: push the current turtle state onto a stack
* `]`: pop and apply the last turtle state; will probably 'teleport' the turtle

## Evolving Shapes ##

<!-- applying production rules; choosing between multiple production rules  -->
To evolve a shape, simply apply the productions.
For example, beginning with the initiator `F-F-F-F` and the production rule `F -> F-F+F+FF-F-F+F` we get the following evolution:

	F-F-F-F
	F-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F
	F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F+F-F+F+FF-F-F+FF-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F+F-F+F+FF-F-F+FF-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F+F-F+F+FF-F-F+FF-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F+F-F+F+FF-F-F+FF-F+F+FF-F-F+F-F-F+F+FF-F-F+F-F-F+F+FF-F-F+F+F-F+F+FF-F-F+F

Assuming an angle increment of 90 (i.e., `a = 90`), this evolution describes the following series of shapes:

![Sample evolution]({{site.url}}/img/lsystems/simple_evolution.png)

### Conflicting Rules ###

-----

[^1]: _Pseudo l-systems_ allow multiple drawing commands to form a production predecessor.

