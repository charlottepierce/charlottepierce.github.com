---
layout: post
title: Intro to L-Systems
summary: Basic concepts behind Lindenmayer systems.
---

L-Systems are a type of formal language.
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
However, unlike l-systems the the generator must implicitly be arranged (i.e., rotated and resized) so that the end points of the generator and the straight line it is replacing match.

## String Rewriting ##

<!--axiom + rules -->

