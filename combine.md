---
title: combine
layout: default
---

# Joining streams with the `combine` combinator

`combine` allows you to handle events from two different streams.

    greetingsStream = Bacon.later(1000, 'hello')
    personsStream = Bacon.later(2000, 'Aatami')

    greetingsStream
      .combine(
        personsStream,
        (greeting, person) => `${greeting}, ${person}!`
      )
      .onValue(message => console.log(message))
    // Will print "hello, Aatami!" after two seconds

A stream created with the `combine` combinator will emit an event whenever one
of underlying streams emit an event.

## Exercise

You have two electronic rulers. Both of them are equiped with a sensor that
can measure the current length of the ruler. Each ruler emits its current length
as an event.

Let's assume that our electronic rulers are aligned in a right angle:

    ruler 1
    |
    |
    |   AREA
    |
    |------------------
                      ruler 2

Define a stream that emits the area defined by the two rulers.

You can use the following streams to simulate the event streams from the rulers:

    ruler1Length = Bacon.sequentially(500, [12, 15, 17, 20])
    ruler2Length = Bacon.sequentially(500, [22, 28, 40, 55])

## Next: `merge`

Next, we'll look at [`merge`](merge.html), which provides another way of joining
streams.
