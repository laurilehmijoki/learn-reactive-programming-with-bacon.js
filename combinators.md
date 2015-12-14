---
title: Combinators
layout: default
---

# Transforming streams with combinators

When we work with streams, we often want to refine the values in a stream with
some logic. Let's see how combinators in Bacon.js enable refinement.

Combinators let us build complex structures with simple functions. Combinators
solve the same problem as the composition pattern in object-oriented
programming.

For example, this is how we can use the `map` combinator:

    doubledValue = Bacon.once(1).map(number => number * 2)

## Commonly used combinators in Bacon.js

* `map` – transforms values in the stream
* `filter` – rejects values in the stream
* `flatMap` – spawns a stream with a new source
* `combine` – combines the latest values of two streams using a two-arg function
* `merge` – merges two streams into one stream that delivers events from both

There are many other combinators in Bacon.js, but the above ones are the
fundamental combinators. Once you understand them, you can rather easily
understand all the other combinators that you encounter.

### Exercise: the `map` combinator

Create a stream that sequentially emits the numbers 1, 2 and 3. Then transform
that stream with the `map` combinator such that the resulting stream will emit
the values 1, 8 and 27.

### Exercise: the `filter` combinator

Again, create a stream that sequentially emits the numbers 1, 2 and 3. With the
help of the `filter` combinator, let through only values that are even.

## Next: the `flatMap` combinator

The `flatMap` combinator captures a fundamental programming concept. [Let's take
a closer look at it.](flatMap.html)
