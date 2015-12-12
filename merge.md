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

## Exercise

Let's assume that you have a chat application where the user can enter text or
browse the messages by scrolling the current window.

Create a stream that indicates whether the user is active or not. A user is
considered inactive if she hasn't typed or scrolled in 1 second.
