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
`things`, which can be either an `Array<T>` or `Iterable<T>`, the result `thingCounts` will be a `Map<T, Int>`.

### Comparing Kotlin with Java

You can do pretty much the same thing with Java 8 with the streams and functional interfaces. The best examples I could 
were the ones by [The Mighty Programmer](https://themightyprogrammer.dev/snippet/frequency-map-java). This is what s/he
has:

```java
import java.util.stream.*;
import java.util.*;
import java.util.function.*;
```
```java

<T> Map<T, Long> frequencyMap(Stream<T> elements) {
return elements.collect(
  Collectors.groupingBy(
    Function.identity(),
    HashMap::new, // can be skipped
    Collectors.counting()
  )
);
}
```

If you think that's nice, check out how you'd do it in Kotlin:

```kotlin
fun <T> frequencyMap(things: Iterable<T>): Map<T, Int> =
    things.groupingBy { it }.eachCount()
```
With that, you can do things like this:
```
    println(frequencyMap(listOf(1,2,2,3,7,8,1,0,3,1)))
    println(frequencyMap(listOf("three", "one", "two", "three", "three", "two")))
```
This is the output:
```text
{1=3, 2=2, 3=2, 7=1, 8=1, 0=1}
{three=3, one=1, two=2}
```

### But wait, you can still do better

As I learned from Lesson #1 in this series, we can always do it better in Kotlin, even though we think we done great
already.

What if you wanted to count differently, like numbers that are greater than 5 or by first letter? Well, it turns out
that it only takes a few tweaks to bubble up the flexibility of the `groupingBy()` function. And it's kind of intuitive
to figure out if you study the API documentation a little bit.

In the first version of `frequencyMap()`, we hard-coded the key selector used by `groupingBy()` to `{ it }`. If we want
to open it back up and give it the flexibility that `groupingBy` has out of the box, we have to expose that as
well.

Turns out, that's pretty easy to do. Let's create a different implementation and add a second parameter, a `keySelector`
which the [groupingBy()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/grouping-by.html)
function expects. Then we change the return type accordingly. Then we pass the keySelector to `groupingBy()`.

This is what we have for the second version:
```kotlin
fun <T, K> frequencyMap2(things: Iterable<T>, keySelector: (T) -> K ): Map<K, Int> =
    things.groupingBy(keySelector).eachCount()
```
We now have to do this:
```kotlin
    println(frequencyMap2(listOf(1,2,2,3,7,8,1,0,3,1)) { it })
    println(frequencyMap2(listOf("three", "one", "two", "three", "three", "two")) { it })
```
This gives the same output as before. It's a little more verbose but we now can also do this:
```kotlin
    println(frequencyMap2(listOf(1,2,2,3,7,8,1,0,3,1)) 
        { if (it < 5) "less than 5 " else "greater or equal to 5" } )

    println(frequencyMap2(listOf("three", "one", "two", "three", "three", "two")) 
        { it[0] } )
```
This is the output:
```text
{less than 5=8, greater or equal to 5=2}
{t=5, o=1}
```
Super cool, right? Sorry, Java but there's a cooler kid in town.

See all of that in action here: https://replit.com/@jlacar/FrequencyMaps

### More examples 

(Coming soon)

## Packing tips for frequent travellers

On a somewhat related note, if you like finding ways to pack more into less, try using
the [Army Roll](https://youtu.be/fuD-ZZydsVg_) method of packing cloths. As a pre-pandemic frequent traveller, I found
it quite effective in fitting a week's worth of cloths into one carry-on bag. And if you hate paying extra for
checked-in bags, you can always do a courtesy check at the gate. Those are always free as long as your bag is small
enough to fit in the overhead compartment.

### [<< Previous article](aoc-learnings.md)

{% include post-footer.md %}