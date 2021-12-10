# Frequency maps and other Kotlin one-liners

_(December 2021)_

> _"I think it's wrong that only one company makes the game Monopoly."_—Steven Wright

If programming languages were comedians, Kotlin would be a Steven Wright. There aren't many comedians out there who can
pack irony and comedic genius into a one-liner like Steven Wright does. When it comes to programming, there aren't many
languages out there that can pack as much logic and expressiveness into a one-liner as Kotlin does.

Brushing up on [Kotlin](https://kotlinlang.org) while doing [Advent of Code 2021](https://adventofcode.com/) has given
me a great appreciation for Kotlin's ability to express the solution to a problem with so little code. I've already
mentioned the `windowed()` and `also()` functions in [the first post of this series](aoc-learnings.md). In this post,
I'll show you how to easily create frequency maps with one line in Kotlin.

## Lesson #4 - Creating frequency maps with `groupingBy()` and `eachCount()`

### TL;DR

```kotlin
val thingCounts = things.groupingBy { it }.eachCount()
```

That's it! 

No, seriously, that's all you need to get a frequency map of each unique value in `things`. So, given
`things` is either an `Array<T>` or `Iterable<T>`, the result `thingCounts` will be a `Map<T, Int>`.

```kotlin
thingCounts.forEach { println("$it.key -> $it.value") }
```
or more simply

### More examples 

smaller parts. I know it can be a String because you can do this:

```kotlin

```

can be any collection or iterable object, like a String.

## Packing tips for frequent travellers

On a somewhat related note, if you like finding ways to pack more into less, try using
the [Army Roll](https://youtu.be/fuD-ZZydsVg_) method of packing cloths. As a pre-pandemic frequent traveller, I found
it quite effective in fitting a week's worth of cloths into one carry-on bag. And if you hate paying extra for
checked-in bags, you can always do a courtesy check at the gate. Those are always free as long as your bag is small
enough to fit in the overhead compartment.

{% include post-footer.md %}