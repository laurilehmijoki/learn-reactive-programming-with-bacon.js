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
functions. Combinators solve the same problem as the composition pattern in
object-oriented programming.

Let's express a combinator with a Scala-like type signature:

    combinator: ((A) => B): Observable[B]

(`Observable` is the class in Bacon.js that defines the `onValue` method.)

For example, this is how we can use the `map` combinator:

    Bacon.once(1).map(number => number * 2)

The type signature for the `map` combinator above is this:

    map: ((Number) => Number): Observable[Number]

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

## A note on side effects in combinators

You should **not** run any side effects in combinators. Use `onValue` or
`onError` if you need to run a side effect.

