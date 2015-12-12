---
title: merge
layout: default
---

# Joining streams with the `merge` combinator

`merge` creates a stream that will emit events from multiple streams.

    dogSounds = Bacon.sequentially(500, ['bark!', 'wuff!'])
    catSounds = Bacon.sequentially(500, ['meow', 'MEOOOOW'])

    dogSounds.merge(catSounds).onValue(animalSound => {
      console.log('Animal says', animalSound)
    })

## Exercise 1

Let's assume that you have a chat application where the user can enter text or
browse the messages by scrolling the current window.

Create a stream that emits a value whenever the user scrolls or types text. Name
this stream as `userActivity` – we'll re-use it in the next exercise.

You can use the following streams to simulate the user's scroll and and text
inputs:

    scrollPosition = Bacon
      .sequentially(500, [0, 12, 44, 124, 155, 101])
      .delay(1500)
    textInput = Bacon.sequentially(600, ['hello', 'friends'])

## Exercise 2

Create a stream that indicates whether the user is active or not. A user is
considered inactive if she hasn't typed or scrolled in 1 second.
