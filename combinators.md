---
title: Combinators
layout: default
---

# Transforming streams with combinators

When we work with streams, we often want to refine the values in a stream with
some logic. Let's see how combinators in Bacon.js enable refinement.

In Bacon.js, combinators are functions that take in another function.
Combinators pass each value in the stream to that function, and the function
should return a value. The type of the return value depends on the combinator. The
combinator always results in a new stream.

In other words, combinators let us build complex structures with simple
functions. Combinators solve the same problem as the composition
pattern in object-oriented programming.

For example, this is how we can use the `map` combinator:

    doubledValue = Bacon.once(1).map(number => number * 2)

The type signature for the `map` combinator above is this:

    map :: (a -> b) -> Observable b

Here is how the type signature reads: `map` takes in function `a -> b`. The
function `a -> b` accepts a value `a`, and it will return a value `b`. The `map`
function will use that function, and it will return an `Observable` on the value
`b`.

## Commonly used combinators in Bacon.js

* `map` – transforms the value in the stream with a function
* `filter` – rejects values in the stream
* `flatMap` – spawns a stream with a new source
* `combine` – combines the latest values of the two streams or properties using a two-arg function
* `merge` – merges two streams into one stream that delivers events from both

There are many other combinators in Bacon.js, but the above ones are the
fundamental combinators. Once you understand them, you can rather easily
understand all the other combinators that you encounter.

### Exercise: the `map` combinator

Create a stream that sequentially emits the numbers 1, 2 and 3. Then transform
that stream with the `map` combinator such that the resulting stream will emit
the values 1, 8 and 27.

### Exercise: the `filter` combinator

Again, Create a stream that sequentially emits the numbers 1, 2 and 3. With the
help of the `filter` combinator, let through only values that are even.

## Next: the `flatMap` combinator

The `flatMap` combinator captures a fundamental programming concept. [Let's take
a closer look at it.](flatMap.html)
