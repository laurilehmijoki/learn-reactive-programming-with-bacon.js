---
title: Introduction
layout: default
---
# Streams – an introduction to reactive programming

Welcome to studying reactive programming. Let's start by comparing promises and
streams. Here we assume that you have used the promise abstraction (jQuery has
promises, for example).

## The difference between promises and streams


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

First, let's install an environment for hacking with streams. Type the following
commands into your command-line console (we assume that you have already
installed the latest [Node.js](https://nodejs.org/)):

    mkdir hacking-with-streams
    cd hacking-with-streams
    npm init --force
    npm install --save --save-exact baconjs
    node # This will start the Node.js REPL

You are now in the Node.js REPL (i.e., Read Evaluate Print Loop).

Now, load Bacon.js:

    const Bacon = require('baconjs')

Then create a stream:

    const numberStream = Bacon.sequentially(500, [1,2,3])

You've just defined a stream of numbers 1, 2 and 3, which will appear with the
interval of 500 milliseconds. Contrary to many promise implementations, Bacon.js
is lazy. This means that the stream is not evaluated unless it has a
side-effect. Our stream does not yet have one. Let's define it:

    numberStream.onValue(number => {
      console.log(number)
    })

Above, we used the `onValue` function, which told the `numberStream` that it's
services are needed by the side-effecting `console.log` function.

### A note on side effects and combinators

In Bacon.js, combinators are functions that take in another function.
Combinators pass each value in the stream to that function, and the function
should return a value. The type of the return value depends on the combinator. The
combinator always results in a new stream.

You should **not** run any side effects in combinators. Use `onValue` or
`onError` if you need to run a side effect.

Examples of combinators are `map`, `filter` and `flatMap`.

## Exercise 1 – create a stream

Create a stream that emits the value "hello" after 1 second. Then define a side
effect that prints that value into the console.
