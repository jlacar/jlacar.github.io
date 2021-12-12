# Frequency maps and other Kotlin one-liners

_(December 2021)_

> _"I think it's wrong that only one company makes the game Monopoly."_—Steven Wright

When it comes to comedy, there aren't many comedians out there who can pack irony and comedic genius into a one-liner as
Steven Wright does. If programming languages were comedians, Kotlin would be a Steven Wright: there aren't many
languages out there that can pack as much logic and expressiveness into a one-liner as Kotlin does.

Doing [Advent of Code 2021](https://adventofcode.com/) in [Kotlin](https://kotlinlang.org) has given me a renewed
appreciation for its expressiveness. I've already mentioned the `windowed()` and `also()` functions
in [the first post of this series](aoc-learnings.md). In this post, I'll show you a few more one-liners I've used to
solve the AoC problems.

Let's start with creating a frequency map with one line of Kotlin.

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

That's nice, but check out how you'd do that in Kotlin:

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

As I learned from Lesson #1 in this series, we can always do it better in Kotlin, even though we've done pretty well
already.

What if you wanted to count differently, like count how many numbers are greater than 5 or count the frequency of the
first letters of each word? Well, it turns out that it only takes a few tweaks to bubble up the flexibility of
the `groupingBy()` function from the `frequencyMap()` function.

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

We now have to pass we have to explicitly pass in `{ it }` as the key selector to say "Treat the element itself as the
thing to count."

Here's another thing I learned: if the last argument in a function call is a lambda expression, you should put it
outside the parentheses. I'm editing this post in IntelliJ IDEA and it's giving me warning which I've added as a comment 
in the code listing below.

```kotlin
   // IDEA message: Lambda argument should be moved outside of parentheses
   frequencyMap2(someList, { it })

   // preferred
   frequencyMap2(someList) { it }   
```

The output is the same as before. Yes, the code is a little more verbose, but we can now also specify different key
selectors, like this:

```kotlin
    println(frequencyMap2(listOf(1,2,2,3,7,8,1,0,3,1)) 
        { if (it < 5) "less than 5 " else "greater or equal to 5" } )

    println(frequencyMap2(listOf("three", "one", "two", "three", "three", "two"))
        { it[0] } )

    println(frequencyMap2(listOf(3,5,15,7,30,12,10,25,45,20,18,7,50,11,9))
        { when {
            it % 15 == 0 -> "fizzbuzz"
            it % 5 == 0 -> "buzz"
            it % 3 == 0 -> "fizz"
            else -> it.toString()
        } } )
```

Check out that third example.

This is the output:
```text
{less than 5=8, greater or equal to 5=2}
{t=5, o=1}
{fizz=4, buzz=5, fizzbuzz=3, 7=2, 11=1}
```

### What about other types?

After experimenting with this a bit, I realized that arrays and strings weren't supported by the function I had so far.
No biggie because the standard library gives us examples how to overload this for Arrays and Strings, or more
appropriately maybe, CharSequences. I don't know if this is the best way to do it, but it seems to work:

```kotlin
fun <T, K> frequencyMap2(arrOfThings: Array<T>, keySelector: (T) -> K): Map<K, Int> =
   arrOfThings.groupingBy(keySelector).eachCount()

fun <K> frequencyMap2(charSequence: CharSequence, keySelector: (Char) -> K): Map<K, Int> =
   charSequence.groupingBy(keySelector).eachCount()
```
With those overloads, I can now do this:
```kotlin
// arrays
println(frequencyMap2(arrayOf(3,4,5,3,4,5,1,2,3)) { it } )  

// Output: {3=3, 4=2, 5=2, 1=1, 2=1}

// strings
println(frequencyMap2("supercalifragilisticexpealidocious") { it })

// Output: {s=3, u=2, p=2, e=3, r=2, c=3, a=3, l=3, i=6, f=1, g=1, t=1, x=1, d=1, o=2} 

println(frequencyMap2("SuperCaliFragilisticExpealidocious") 
   { if (it.isUpperCase()) "UPPER" else "lower" } )

// Output: {UPPER=4, lower=30}
```
### See it in action

You can run these examples yourself here: [https://replit.com/@jlacar/FrequencyMaps](https://replit.com/@jlacar/FrequencyMaps)

### Ruthless Refactoring

I should have mentioned from the start that a function like `frequencyMap` isn't really necessary; I could have just
as easily inlined it wherever I needed a frequency map. These work just as well out of the box:
```kotlin
   println(listOf(1,2,2,3,7,8,1,0,3,1).groupingBy { it }.eachCount())

   println(listOf(1,2,2,3,7,8,1,0,3,1).groupingBy { 
       if (it < 5) "less than 5 " else "greater or equal to 5" }.eachCount())

   println(arrayOf(3,4,5,3,4,5,1,2,3).groupingBy { it }.eachCount())

   println("supercalifragilisticexpealidocious".groupingBy { it }.eachCount())

   println("SuperCaliFragilisticExpealidocious".groupingBy { 
       if (it.isUpperCase()) "UPPER" else "lower" }.eachCount())
```

There are a couple of reasons I wrote an intermediate `frequencyMap()` function. 

First, I wanted to have a more apples-to-apples comparison with Java. Given the Java example I cited, I wanted to give
Java a fair shake, even though it seems clear that Kotlin is the head-to-head winner when it comes to brevity and
clarity.

Second, and perhaps more compelling for me, is that I'm kind of obsessed with writing expressive code. My gut reaction 
to something like `things.groupingBy { it }.eachCount()` is to refactor it to be more expressive. For me, a function
like `frequencyMap()` expresses the intent more clearly, even though the straight-up one-liner is already pretty sweet.

But that's just me. Feel free to by-pass the extra layer of abstraction if you feel it doesn't add much value for you.
For me, anything that makes the code more expressive is generally more of a good thing than bad.

## Lesson #5: Using `flatMap()`, `last()`, and `count()`

I fell behind on solving the puzzles this week because of work- and life-related stuff. As I'm writing this section,
it's a couple of hours into Day 12 and I am just about to start looking into Part 2 of the Day 8 problem. Before I do
that, I wanted share my solution for Part 1.

Part 1 of AoC Day 8 is essentially about counting words that have a specific length. The input consists of a list of
strings, each one containing fourteen words of different lengths. To complicate things, the fourteen words are in two
groups, the first group of ten words separated by " | " from the second group of four words. The task is to count how
many words in the second group, the one with four words, have a length of either 2, 3, 4, or 7.

Solving this in Kotlin was pretty easy. The hardest part was looking through the API documentation to verify
that `flatMap()` worked like I thought it does.

### What does it mean to "flatten" something?

I seldom use `flatMap()` but it was perfect for this problem. Essentially, it unnests or "flattens" a nested data
structure. For example, if you have a `List<List<String>>`, flattening that would give you a `List<String>`. That's
exactly what I needed to solve Day 8, Part 1.

### Day 8, Part 1 of the Advent of Code 2021 (Spoiler Alert!)

Here's my solution:
```kotlin
    val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

    fun part1(input: List<String>): Int = input
            .flatMap { it.split(" | ").last().split(" ") }
            .count { it.length in lengthsOfDigits1478 }
```

Once again, Kotlin allowed me to pack a lot of logic and expressiveness into just one line of code. Sure, it's a chain of
operations but it's still just one line of code. Well, okay, two lines if you include the one for `lengthsOfDigits1478`.

Here's how that works.

Each line in the input looks something like this: `be cfbegad cbdgef fgaecd cgeb fdcge agebfd fecdb fabcd edb | fdgacbe cefdb cefbgd gcbe`. For Part 1, we're only
want the words after the "|" separator. The `it.split(" | ")` separates each line in the input into its two parts. 

We can get to the second part, the substring after the "|", with the `last()` function. It returns the last element of
any collection of things.

Alternatively, we could have used `lastIndex()` but that would make the expression more complicated than it needs to be.
Since all we're interested in is the last element, `last()` is the shortest and quickest way to get to it.

We could have also done it using indexed access, like so:
```kotlin
it.split(" | ")[1] // gives back the second group of words
```

Because of Kent Beck's Rule #2 of Simple Design: the code must clearly
express its intent, I prefer the more expressive `last()` over `[1]`.

Now that we've isolated the second part of each line in the input, we need to split it again, this time to
separate the four words from each other. For that, we use `split(" ")`. At this point, we have a `List<List<String>>`. 

> SIDEBAR: When working with a long chain of calls, it's not always easy to figure out what you have at a specific point in the expression. I found that if you use the handy dandy `also()` function, you can get IDEA to tell you exactly what you have at that point. Just insert `.also { it }` where you want to check what kind of thing you have and then hover over `it`: IDEA will then show you a hint that tells you exactly what `it` is.

We're really only interested in the Strings as a collective, not as nested list of lists. The `flatMap()` function
lets us convert the nested `List<List<String>>` into a "flattened" `List<String>`.

For example, if you have `[[fdgacbe, cefdb, cefbgd, gcbe], [fcgedb, cgb, dgebacf, gc], [cg, cg, fdcagb, cbg]]`,
`flatMap`-ping this gives us `[fdgacbe, cefdb, cefbgd, gcbe, fcgedb, cgb, dgebacf, gc, cg, cg, fdcagb, cbg]`

From here, getting a `count()` based on word length is straightforward.

### Just say what you need to say, and no more.

Initially, I had this, which I thought was pretty darn good:
```kotlin
    fun part1(input: List<String>): Int = input
            .flatMap { it.split(" | ").last().split(" ") }
            .filter { it.length in lengthsOfDigits1478 }
            .count()
```

But of course, Lesson #1 says you can always say it better and shorter in Kotlin. IntelliJ IDEA quietly reminded me that
I was saying too much and suggested I merge the `filter { predicate }.count()` call chain to `count { predicate }`,
which of course totally makes sense. Why say more when you can do it with less? Steven Wright would be proud.

## From left field: Packing tips for frequent travellers

On a somewhat related note, if you like finding ways to pack more into less, try using
the [Army Roll](https://youtu.be/fuD-ZZydsVg_) method of packing cloths. As a pre-pandemic frequent traveller, I found
it was a great way to fit a week's worth of cloths into one carry-on bag. And if you hate paying extra fees for
checked-in bags, you can always courtesy check your carry-on at the gate. That's always been free as long as your bag is
small enough to fit in the overhead compartment.

### [<< Previous article](aoc-learnings.md)

{% include post-footer.md %}
