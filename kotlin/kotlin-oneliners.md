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

Starting from Java 8, you can do pretty much the same thing with streams and functional interfaces. The best examples I
found are the ones by [The Mighty Programmer](https://themightyprogrammer.dev/snippet/frequency-map-java). This is what
s/he has:

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

As I learned from Lesson #1 in this series, we can always do it better in Kotlin, even though we think we've done great
already.

What if you wanted to count differently, like numbers that are greater than 5 or by first letter of each word? Well, it
turns out that it only takes a few tweaks to bubble up the flexibility of the `groupingBy()` function from
the `frequencyMap()` function.

In the first version of `frequencyMap()`, we hard-coded the key selector used by `groupingBy()` to `{ it }`, which
serves the same purpose as `Function.identity()` in the Java version. We want to open our interface back up to expose
the flexibility the `groupingBy()` function allows out of the box. As it turns out, that's pretty easy to do with
information we can get from the `groupingBy()`
[API documentation](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/grouping-by.html).

Let's create a different implementation and add a second parameter, a `keySelector`, which is what the `groupingBy()`
function expects. We'll then change the return type of the function accordingly.

This is what we have for the second version:
```kotlin
fun <T, K> frequencyMap2(things: Iterable<T>, keySelector: (T) -> K ): Map<K, Int> =
    things.groupingBy(keySelector).eachCount()
```

With this change, the function call now becomes this:
```kotlin
    println(frequencyMap2(listOf(1,2,2,3,7,8,1,0,3,1)) { it })
    println(frequencyMap2(listOf("three", "one", "two", "three", "three", "two")) { it })
```

Since we now pass in the key selector `{ it }` as a parameter, we have to add it to call. As I mentioned
before, `{ it }` is the equivalent of `Function.identity()` in Java, which is to say "Treat the element itself as the
thing we want to count." In Kotlin, if the last argument in a function call is a lambda expression, you'll be told to
put it outside of the parentheses.

```kotlin
   // non-idiomatic: lambda argument should be moved outside of parentheses
   frequencyMap2(someList, { it })

   // preferred
   frequencyMap2(someList) { it }   
```

The output is the same as before. Sure, the code is a little more verbose, but we now also specify different 
key selectors, like this:

```kotlin
    println(frequencyMap2(listOf(1,2,2,3,7,8,1,0,3,1)) 
        { if (it < 5) "less than 5 " else "greater or equal to 5" } )

    println(frequencyMap2(listOf(3,5,15,7,30,12,10,25,45,20,18,7,50,11,9))
        { when {
            it % 15 == 0 -> "fizzbuzz"
            it % 5 == 0 -> "buzz"
            it % 3 == 0 -> "fizz"
            else -> it.toString()
        } } )

    println(frequencyMap2(listOf("three", "one", "two", "three", "three", "two")) 
        { it[0] } )
```

This is the output:
```text
{less than 5=8, greater or equal to 5=2}
{fizz=4, buzz=5, fizzbuzz=3, 7=2, 11=1}
{t=5, o=1}
```
Super cool, right? Sorry, Java, there's talk on the street is that there's a new, cooler kid in town.

See all of that in action here: https://replit.com/@jlacar/FrequencyMaps

### More examples 

(Coming soon)

## From left field: Packing tips for frequent travellers

On a somewhat related note, if you like finding ways to pack more into less, try using
the [Army Roll](https://youtu.be/fuD-ZZydsVg_) method of packing cloths. As a pre-pandemic frequent traveller, I found
it quite effective in fitting a week's worth of cloths into one carry-on bag. And if you hate paying extra for
checked-in bags, you can always do a courtesy check at the gate. Those are always free as long as your bag is small
enough to fit in the overhead compartment.

### [<< Previous article](aoc-learnings.md)

{% include post-footer.md %}