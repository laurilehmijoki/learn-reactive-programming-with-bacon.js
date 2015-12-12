---
title: Introduction
layout: default
---
# Streams – an introduction to reactive programming

Welcome to studying reactive programming.

## When to use reactive programming – a checklist

* Your program models asynchronous phenomena
* Your program is easy to think in terms of events
* You would like to refine low-level events into high-level user interfaces

## When *not* to use reactive programming – another checklist

* Your program does not interact with the user's input devices such as keyboard
  or mouse
* Your program is not dependent on network events such as WebSocket messages
* Your team does not have time or motivation to study a new programming style

## The difference between promises and streams

Let's start by comparing promises and
streams. Here we assume that you have used the promise abstraction (jQuery has
promises, for example).

There are several abstractions that attempt to make it easier to deal with
asynchronous code. The promise and stream abstractions are one of them. An
important difference between these two abstractions is that promises deal with
singular results, where as streams deal with multiple results.

In other words, a promise says that it will give you back one asynchronous
result. A stream says that it will provide you with potentially infinite number
of values.

In this tutorial we are going to use [Bacon.js](https://baconjs.github.io/).
Bacon.js is a reactive programming library that implements the stream
abstraction.

## Hands on! Creating streams with Bacon.js

Bacon.js [provides](https://github.com/baconjs/bacon.js#creating-streams) a set
of functions that let you create streams of values (or event streams, as they
are often called).

Let's use the Chrome console as our JavaScript environment. Open the Developer
Tools on this page (on Mac, press `⌥+⌘+J`).

Then create a stream:

    numberStream = Bacon.sequentially(500, [1,2,3])

You've just defined a stream of numbers 1, 2 and 3, which will appear with the
interval of 500 milliseconds. Contrary to many promise implementations, Bacon.js
is lazy. This means that the stream is not evaluated unless it has a
side effect. Our stream does not yet have one. Let's define it:

    numberStream.onValue(number => {
      console.log(number)
    })

Above, we used the `onValue` function, which told the `numberStream` that it's
services are needed by the side-effecting `console.log` function.

### Exercise – create a stream

Create a stream that emits the value "hello" after 1 second. Then define a side
effect that prints that value into the console.

Tip: [here](https://github.com/baconjs/bacon.js#creating-streams) you can find
the function that creates a delayed stream.

## Next: combinators

We have now taken a look at the stream abstraction. We've also gotten our hands
warm with Bacon.js.

Next, let's dive into the world of [combinators](combinators.html).
