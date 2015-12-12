---
title: flatMap
layout: default
---

# The `flatMap` combinator

## The general idea of computation containers

The idea behind `flatMap` captures a fundamental programming concept of
computation container. Below are examples of containers:

|**container**|**computation**|**example**|
|list|item in list|`[1,2,3]`
|dictionary|key-value pair|`{myValue: true}`
|string|character|`'hello'`
|promise|asynchronous value|`Promise.resolve(42).delay(500) // Bluebird`
|stream|many asynchronous values|`Bacon.sequentially(500, [1,2,3])`

The concept of computation container allows us to handle the computation and the
container separately. This results in greater flexibility.

## Changing the structure of a stream with `flatMap`

We can think of a stream as a container for asynchronous values. With the `map`
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

Above, we changed a stream of strings into a stream of HTTP responses. We could
not have done that with `map`, because `map` only allows us to change the values
*in the stream*. In contrast, `flatMap` allowed us to change the structure of
the stream from strings to HTTP responses.

### Exercise: `flatMap`

Create a stream of numbers:

    numbersStream = Bacon.fromArray([1,2,3])

Then transform the stream such that the resulting stream will emit the values `1
second(s)`, `2 second(s)` and `3 second(s)` every one second.

## Error handling in stream programming

Because streams are computation containers, they allow us to neatly separate
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
