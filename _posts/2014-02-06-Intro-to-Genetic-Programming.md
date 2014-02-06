---
layout: perma_post
summary: Basic ideas behind genetic programming.
---

Genetic programming (GP) is a class of [evolutionary algorithm](https://en.wikipedia.org/wiki/Evolutionary_algorithm) concerned with generating a computer program to perform some task.
[A Field Guide to Genetic Programming](http://www.gp-field-guide.org.uk/) describes the technique as follows:

	"[genetic programming] automatically solves problems without requiring the user to know or specify the form or structure of the solution in advance"

Equipped with a set of actions, queries, and a method for assessing program performance, genetic programming attempts to generate increasingly effective programs to solve a problem.

# Process #

Genetic programming begins with a set of randomly generated candidate solutions, and runs over a number of iterations.
During each iteration the performance of each candidate isevaluated and each is assigned a 'fitness' value.
If the fitness measure is reasonable, the higher a fitness value is the better the candidate performed.

Before each subsequent iteration an entirely new candidate population is created.
This is done using the previous as a basis by selecting candidates according to their fitness and applying genetic operations.

The general process is shown in the following flow chart from [The GP Tutorial](http://www.geneticprogramming.com/Tutorial/):

![Genetic programming process]({{site.url}}/img/gp_flowchart.png)

The process as given requies a number of items be defined:

* Population size

Population size determins the number of candidate programs to be tested each iteration.
If too small candidate selection will be detrimentally limited; if too large good solutions may take overly long to become dominant.

* Fitness function

The fitness function provides a way to quantify the suitability of a candidate solution.
This measure has a large effect on the outcome of genetic programming, as it is a driving force when generating new candidate populations.

* Termination criteria

Defines when genetic programming should stop.
Once met the best agent (either from all or the most recent iteration) is returned as the solution.
Generally set as a desired fitness value or number of iterations.

* Probability of genetic operations

To create new candidate populations genetic operations are applied to candidates from the previous population.
Each operation - reproduction, crossover and mutation - is associated with a probability, representing the chance it will be applied.

Genetic programming is generally described as being a _stochastic_ process - the initial population is set randomly, genetic operations are selected randomly, and the operations themselves involve randomness.
This characteristic can be responsible for improving results compared to strictly deterministic methods, as random elements increase the scope of candidate solutions.

## Selection Methods ##

Selections methods are formal methods by which candidates from the previous iteration are chosen when regenerating the population.
As such, the method used has a large impact on the final solution.

Two well-known methods are:

* __Roulette Wheel__

Each candidate is assigned a probability proportional to its fitness and selection is weighted by the assigned probabilities.
As such, candidates with higher fitness values are more likely to be selected, but all candidates have some chance.

* __Tournament__

A number of candidates are selected completely at random, and the one with the highest fitness is selected.

## Program Representation ##

Programs generated using genetic programming are typically represented as tree structures, where each node represents a single action or conditional.
This is for a number of reasons:

* Conditional statements

Branching in tree-like structures allows for easy representation of `if...then` statements.

* Loops

Loops are easily represented in tree-like structures, as they can be created simply by linking two existing nodes.

* Self-contained branches

Each branch in a program tree represents a self-contained, syntactically correct program.
Because of this restructuring a tree (i.e., by applying genetic operations) will not affect the syntactic correctness of the program.
This is not true for other representation systems, and this characteristic greatly reduces the complexity involved in implementing genetic programming.

## Genetic Operations ##

* __Reproduction__

The simplest genetic operation - the selected candidate is directly copied into the new population.

* __Crossover__

Two candidates are selected.
A _crossover point_ is randomly selected in the program tree of each candidate.
The subtree at the crossover point in the first candidate is removed and replaced with a copy of the subtree at the crossover point in the second candidate, and the resulting program tree is placed in the new population.

Generally crossover is given the highest probability of occurrence.

* __Mutation__

A single candidate is selected.
Similar to the crossover operation a _mutation point_ in the program tree is randomly selected.
The subtree at this point is removed and 'regrown' using the same random process used for the very first population.

Mutation is typically assigned a very low probability of occurrence (e.g., 1 in 1000) to avoid introducing a detrimental level of randomness into the solution space.

-----

For a practical application of genetic programming, see [this post]({% post_url 2014-02-06-Goal-Oriented-Adversarial-Movement-Behaviours-with-Genetic-Programming %}).

