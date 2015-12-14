---
title: flatMap
layout: default
---

# The `flatMap` combinator

## The general idea of computation contexts

The idea behind `flatMap` captures a fundamental programming concept of
computation context. Below are examples of contexts:

|**context**|**computation**|**example**|
|list|item in list|`[1,2,3]`
|dictionary|key-value pair|`{myValue: true}`
|string|character|`'hello'`
|promise|asynchronous value|`Promise.resolve(42).delay(500) // Bluebird`
|stream|many asynchronous values|`Bacon.sequentially(500, [1,2,3])`

The concept of computation context allows us to handle the computation and the
context separately. This results in greater flexibility.

## An excursion: computation contexts in other programming environments

The idea of computation contexts is not particular to stream programming. Once
you understand the idea, you can benefit from it in various contexts.

### Scala

    $ scala
    scala> Seq(1,2,3).flatMap(num => Seq(num, num * 2))
    res0: Seq[Int] = List(1, 2, 2, 4, 3, 6)

### Haskell

    $ ghci
    Prelude> [1,2,3] >>= (\num -> [num, num * 2])
    [1,2,2,4,3,6]

Next, let's turn back to `flatMap`.

## Changing the structure of a stream with `flatMap`

We can think of a stream as a context for asynchronous values. With the `map`
combinator we can transform the value *in* the stream, but what if want to
change the structure of the stream? We need to use `flatMap`.

Let's imagine that we have a stream of strings from a text input field (think of
a search field on a web application). Now we'd like to transform each of those
strings into a database query:

    searchInputs = Bacon.fromArray(['h', 'hel', 'hello'])
    databaseResults = searchInputs.flatMap(textInput => Bacon.fromPromise(
      fetch(`https://httpbin.org/response-headers?result=Results for ${textInput}`)
        .then(response => response.json())
    ))
    databaseResults.onValue(result => {
      console.log('HTTP response', result)
    })

Above, we transformed a stream of strings into a stream of HTTP responses. We
could not have done that with `map`, because `map` only allows us to change the
values *in the stream*. In contrast, `flatMap` allowed us to spawn a new stream
from each event in the `searchInputs` stream.

### Exercise: `flatMap`

Create a stream of numbers:

    numbersStream = Bacon.fromArray([1,2,3])

Then transform `numbersStream` such that the resulting stream will emit the values `1
second(s)`, `2 second(s)` and `3 second(s)` every one second.

### Exercise: implement `map` with the help of `flatMap`

### Exercise: implement `filter` with the help of `flatMap`

## Error handling in stream programming

Because streams are computation contexts, they allow us to neatly separate
successful computations from failed ones.

Let's consider the search example we introduced above. What if the HTTP request
fails? How will an error manifest in a stream? Let's change the hostname
`httpbin.org` to `foobar.org` and see what happens.

    searchInputs = Bacon.fromArray(['h', 'hel', 'hello'])
    databaseResults = searchInputs.flatMap(textInput => Bacon.fromPromise(
      fetch(`https://foobar.org/response-headers?result=Results for ${textInput}`)
        .then(response => response.json())
    ))
    databaseResults.onValue(result => {
      console.log('HTTP response', result)
    })

Notice how the `HTTP response` string did not appear in the console log. This is
because the [`fetch`](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
promise resolved into a failure, and `Bacon.fromPromise` translated that failure
into an error event in the `databaseResults` stream.

Let's add a handler for the error events:

    searchInputs = Bacon.fromArray(['h', 'hel', 'hello'])
    databaseResults = searchInputs.flatMap(textInput => Bacon.fromPromise(
      fetch(`https://foobar.org/response-headers?result=Results for ${textInput}`)
        .then(response => response.json())
    ))
    databaseResults.onError(errorMessage => {
      console.log('Got error', errorMessage)
    })

Above, we defined the `onError` handler instead of the success-handler
`onValue`.

## Next: the `combine` combinator

We learned that `flatMap` allows us to define new event sources. [Let's see how
we can combine streams](combine.html).
