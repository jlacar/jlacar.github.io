---
layout: post
title: Fluent Interfaces with Kotlin
author: Junilu Lacar
banner-img: /assets/images/aoc2023/07-camels/camel-cards-3.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 400
banner-height: 400
---
In this second installment of a planned series of articles, I'll demonstrate how I created a simple fluent interface and domain-specific language (DSL) in Kotlin. I'll do this in the context of solving the [Day 7 puzzle](https://adventofcode.com/2023/day/7) of [Advent of Code (AoC) 2023](https://adventofcode.com/2023).

You can see the full code in [my GitHub repo for AoC 2023](https://github.com/jlacar/aoc-2023-kotlin/blob/main/src/Day07.kt).

## A fluent interface for Camel Cards

The [Wikipedia entry for "fluent interface"](https://en.wikipedia.org/wiki/Fluent_interface) starts with this short paragraph:
> In software engineering, a **fluent interface** is an [object-oriented API](https://en.wikipedia.org/wiki/Object_oriented_design) whose design relies extensively on [method chaining](https://en.wikipedia.org/wiki/Method_chaining). Its goal is to increase code legibility by creating a [domain-specific language](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL). The term was coined in 2005 by Eric Evans and [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler_(software_engineer)).

In [another article](/coding/2023/12/22/aoc-day7-part1-overview.html), I mentioned how a DSL can help align people's understanding by giving them a common set of terms and phrases to use for communicating ideas. The more aligned a program is to the language of the domain, the less translation needs to happen between one set of terms and another, i.e., from techie-speak to nontechie-speak and vice versa.

A DSL can also make it easier for developers to organize their thoughts and ideas in the program by suggesting logical groupings of logic and data. This can lead to more coherent and cohesive code.

### A short introduction to the Camel Cards domain

As an overview, Camel Cards is like Poker, except with a few twists. There are also different **types of hands**, like **five of a kind** and **full house**. Hands are **ranked** relative to each other, and **winnings** are calculated based on the hand's **rank** and the **bid** made with it.

Please go to the [AoC 2023 Day 7 puzzle page](https://adventofcode.com/2023/day/7) to read the full description and rules of Camel Cards.

### The non-fluent solution

I was quite happy with the solution I had ended up using to submit my answers to the Day 7 puzzle. Not only did it produce the correct answers that earned me a couple of gold stars, I thought the code was quite clear and expressive. 

This is what I had in the main solution class:
```kotlin
class Day07(private val plays: List<CamelCardPlay>): AocSolution() {

    override val description = "Day 7 - Camel Cards"
        
    override fun part1(): Int = 
        totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))

    override fun part2(): Int = 
        totalWinnings(plays.sortedWith( compareBy { it.jokerStrength } ))

    private fun totalWinnings(rankedPlays: List<CamelCardPlay>): Int =
        rankedPlays.mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

    companion object {
        fun using(input: List<String>) = Day07(
                plays = input.map {
                           val (hand, bid) = it.split(" ")
                           CamelCardPlay(hand, bid.toInt())
                        }
            )
    }
}
```
#### **Listing 1**. The code to be refactored to a fluent interface
____

As I said, I was quite happy with the code until I realized that the call to `totalWinnings()` could be written in a more fluent style. A more fluent style would probably be a more idiomatic way to write the Kotlin code. 

Time for some refactoring. Before we do that though, let's get something straight about the term "refactoring."

### You keep using that word. I don't think it means what you think it means

The term refactoring suffers quite a bit from what Martin Fowler calls [semantic defusion](https://martinfowler.com/bliki/SemanticDiffusion.html). This is unfortunate, so I try to do my part to correct the [common malapropism of refactoring](https://martinfowler.com/bliki/RefactoringMalapropism.html) and do it in the way that Fowler describes it. 

Fowler writes this about mischaracterizing any act of changing code as "refactoring":

>  If somebody talks about a system being broken for a couple of days while they are refactoring, you can be pretty sure they are not refactoring...
> 
> Refactoring is a very specific technique, founded on using small behavior-preserving transformations (themselves called refactorings). If you are doing refactoring your system should not be broken for more than a few minutes at a time...

I want to emphasize some things Fowler says about refactoring:

1. _It's founded on **small** transformations_. Refactoring is done in small steps. Usually, smaller than what you think "small" is. [Renaming](https://www.jetbrains.com/help/idea/rename-refactorings.html) is usually a small change that can be done in one step with most modern IDEs that support automated refactoring. [Replacing conditionals with polymorphism](https://refactoring.guru/replace-conditional-with-polymorphism) typically cannot be done in one step, requiring several interconnected steps to complete. Unfortunately, most programmers have neither the patience nor the discipline to do this safely, so this refactoring is seldom performed and the opportunity to improve the design often passed up.

2. _It's founded on **behavior-preserving** transformations_. Refactoring does not change a program's externally observable behavior. That means that to refactor properly and safely, the program should pass all its tests _before_ any change is made to it and continue to pass all its tests _after_ the refactoring is done.

3. _Your system **should not be broken** for more than a few minutes at a time_. I would even go farther and assert that ideally, it shouldn't be broken at all. Sometimes, however, having your program in a broken state for a few minutes can expedite the work. I would advise you to be careful in doing this: the longer the code stays broken, the longer it takes to get useful feedback from it. It's usually better to revert your changes and try again before you make things worse.

Keeping with the discipline of refactoring as described above can lead you to Test and (Commit or Revert). TCR is a workflow [introduced a few years ago by Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864). I'll take a more detailed look at this alternative workflow relative to TDD in a future article.

### Mapping out where we want to go

When I go on a road trip, I don't just start driving in any random direction. When I head out, I have a good idea of the general direction I'm going. These days, it's easy. I just enter an address in my GPS app and I'll get turn-by-turn guidance all the way to my destination.

Likewise, when embarking on a non-trivial refactoring, I find it useful to have a general sense of where I'm going. Unfortunately, for anything larger and more complicated than the simple refactorings my IDE can do automatically, I don't get the kind of step-by-step guidance a GPS app gives me. I have to figure out how to string many small refactoring steps together so I can reach my final destination. This can be challenging, especially when the way is unclear.

To help me stay on track, I'll typically sketch a path in the code. When I'm doing TDD, I use tests to do these sketches. Luckily, for this problem it was enough to use comments to remind me of my final destination, so I didn't get off track as I refactored my way toward it. Listing 2 below shows the comments I added to guide my refactoring.

```kotlin
// try to make the code tell its story more fluently, like this...
// override fun part1() = plays.rankedWith(normalRules).totalWinnings()
// override fun part2() = plays.rankedWith(jokerRules).totalWinnings()

// ...instead of this
override fun part1(): Int =
    totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))

override fun part2(): Int = 
    totalWinnings(plays.sortedWith( compareBy { it.jokerStrength } ))
```
#### **Listing 2**. Mapping out our intent, to guide refactoring
____

### Step 1 - Use an extension function to add to the call chain

Kotlin's [extension functions](https://kotlinlang.org/docs/extensions.html) give us a way to easily extend the functionality of an existing class, even one we can't edit. This comes in really handy for creating DSLs and fluent interfaces. 

Extension functions are defined just like normal functions except we prefix the name with the receiver type, the type whose behavior we're extending. 

In this case, we want to extend the behavior of `List<CamelCardPlay>`, so we're going to create an extension function with that as its receiver.

```kotlin
// ...instead of this
override fun part1(): Int = 
    totalWinningsOLD(plays.sortedWith( compareBy { it.normalStrength } ))

override fun part2(): Int = 
    totalWinningsOLD(plays.sortedWith( compareBy { it.jokerStrength } ))

private fun totalWinningsOLD(rankedPlays: List<CamelCardPlay>): Int =
    rankedPlays.mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

// *new* - extension function 
private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()
                                                         
```
#### **Listing 3**. Adding a new extension function
____

Note that I renamed the old `totalWinnings()` function to facilitate switching to the new extension function later.

Remembering to take small steps, we first try the new function with `part1()`:

```kotlin
// try to make the code tell its story more fluently, like this...
// override fun part1() = plays.rankedWith(normalRules).totalWinnings()
// override fun part2() = plays.rankedWith(jokerRules).totalWinnings()

// Step 1 - move call to totalWinnings() to the end of the call chain
override fun part1(): Int = 
    plays.sortedWith( compareBy { it.normalStrength } ).totalWinnings()

// ...instead of this
//override fun part1(): Int = 
//    totalWinningsOLD(plays.sortedWith( compareBy { it.normalStrength } ))

```
#### **Listing 4**. Trying out the new extension function
____

Note that the only difference so far is the call to `totalWinnings()`. The rest of the call chain remains the same. We'll deal with those parts later. Right now, our focus is on using the extension function that calculates the total winnings for all the `CamelCardPlay`s in the puzzle input.

We run the tests that were already passing before and confirm we haven't broken anything. Great, we can now apply the same change to `part2()`. Doing so makes `totalWinningsOLD()` unused so we can safely delete it:

```kotlin
// try to make the code tell its story more fluently, like this... 
// override fun part1() = plays.rankedWith(normalRules).totalWinnings()
// override fun part2() = plays.rankedWith(jokerRules).totalWinnings()

// Step 1 - move call to totalWinnings() to the end of the chain
override fun part1(): Int = 
    plays.sortedWith( compareBy { it.normalStrength } ).totalWinnings()

override fun part2(): Int = 
    plays.sortedWith( compareBy { it.jokerStrength } ).totalWinnings()

// *new* - extension function 
private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

```
#### **Listing 5**. Completing Step 1 and tidying up
____

### Step 2 - Refactoring the code to make it consistently fluent

We can now shift our attention to the next bit of non-fluency in the call chain: the call to `sortedWith(...)`. This is a general-purpose function provided by the Kotlin Standard Library and it's currently sitting between the two domain-specific ideas of `plays` and `totalWinnings()`. This makes the call chain inconsistently fluent.

We'll define another extension function to make it more fluent. Since the `sortedWith()` parts work, the new extension function will only serve as an alias that allows us to use a more domain-centric name in place of the general name. 

The receiver type for this extension function, which we'll call `rankedWith()`, is the type of the `plays` object, `List<CamelCardPlay>`. Likewise, its return type also needs to be `List<CamelCardPlay>` because we're chaining it with `totalWinnings()`.

```kotlin
// try to make the code tell its story more fluently, like this...
// override fun part1() = plays.rankedWith(normalRules).totalWinnings()
// override fun part2() = plays.rankedWith(jokerRules).totalWinnings()

override fun part1(): Int = 
    plays.sortedWith( compareBy { it.normalStrength } ).totalWinnings()

override fun part2(): Int = 
    plays.sortedWith( compareBy { it.jokerStrength } ).totalWinnings()

private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

// *new* - extension function to alias sortedWith()
private fun List<CamelCardPlay>.rankedWith(): List<CamelCardPlay> =
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)   

```
#### **Listing 6**. Adding the `rankedWith()` extension function
____

Note that the new `rankedWith()` extension function takes a parameter of type `Comparator<in CamelCardPlay>`. I won't go into the details of this but if you're curious, read up on [Kotlin generics and declaration site variance](https://kotlinlang.org/docs/generics.html#declaration-site-variance).

Taking another small step, we try it with `part1()` first to see if it works. It does, so we make the same change to `part2()`. Then we tidy up.

```kotlin
// try to make the code tell its story more fluently, like this... 
// override fun part1() = plays.rankedWith(normalRules).totalWinnings()
// override fun part2() = plays.rankedWith(jokerRules).totalWinnings()

override fun part1(): Int = 
    plays.rankedWith( compareBy { it.normalStrength } ).totalWinnings()

override fun part2(): Int = 
    plays.rankedWith( compareBy { it.jokerStrength } ).totalWinnings()

private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

// *new* - extension function
private fun List<CamelCardPlay>.rankedWith(
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)   

```
#### **Listing 7**. Replacing `sortedWith()` with `rankedWith()`
____

The end, as we mapped it out when we began, is now in sight.

### Step 3 - Extracting to an explaining variable

The story the code tells now is still a little inconsistent. The `compareBy {...}` parts are once again calls to a general-purpose function provided by the Standard Kotlin Library. We'd like to use a domain-specific term in its place to make the story told by the call chain completely fluent.

We can do this by [extracting the expression to an explaining variable](https://refactoring.guru/extract-variable).

Again, we try it with `part1()` first and see all tests pass before we make a similar change to `part2()`. 

The code now says the same thing the comments at the top say. We've achieved our refactoring goal and can delete those guiding comments. After tidying up, we end up with this:

```kotlin
class Day07(private val plays: List<CamelCardPlay>): AoCSolution() {

    override val description = "Day 7: Camel Cards"

    override fun part1(): Int {
        val normalRules: Comparator<CamelCardPlay> = compareBy { it.normalStrength }

        return plays.rankedWith(normalRules).totalWinnings()  // more fluent!
    }

    override fun part2(): Int {
        val jokerRules: Comparator<CamelCardPlay> = compareBy { it.jokerStrength }
        
        return plays.rankedWith(jokerRules).totalWinnings()  // more fluent!
    }

    private fun List<CamelCardPlay>.rankedWith(
        comparator: Comparator<in CamelCardPlay>
    ): List<CamelCardPlay> = sortedWith(comparator)

    private fun List<CamelCardPlay>.totalWinnings(): Int =
        mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

    companion object {
        fun using(input: List<String>) = Day07(
                plays = input.map {
                            val (hand, bid) = it.split(" ")
                            CamelCardPlay(hand, bid.toInt())
                        }
            )
    }
}
```
#### **Listing 8**. The tidied up code with a more fluent interface
____

### Optimize for reading first 

I'm sure there will be someone who will question the value of this refactoring because it ended up with more lines of code, implying that having more code is a bad thing. 

Certainly, simplicity favors less code, but more code is not necessarily always a bad thing. Look through [Martin Fowler's books on refactoring](https://martinfowler.com/books/refactoring.html) and you'll see a few examples like this, particularly in the first long refactoring example he gives at the beginning of the book. 

The primary objective is readability, and sometimes that comes at the cost of brevity. I think it's misguided to make judgements based solely on the number of lines of code or even the number of function or method calls made. 

In his recently released book, [_Tidy First?_](https://tidyfirst.substack.com/), Kent Beck wrote:
> The biggest cost of code is the cost of reading and understanding it, not the cost of writing it.

Our main priority as developers should be to make the code as easy to read and understand as possible. Optimizing for performance comes later, and only when there's clear and compelling evidence that the program's performance is not acceptable. 

Remember what Sir Tony Hoare (popularized by Donald Knuth) said:
> Premature optimization is the root of all evil.

As developers, we generally suck at using gut feeling and intuition for performance tuning. Any decision to optimize for performance at the cost of readability should be based on quantitative measures. Use a profiler to gather empirical data that clearly shows a problem. Profiling will help you find where the true performance bottlenecks are, and they're usually not where you _think_ they are. 

Make readability and understandability the priority. Optimize for performance later, only when necessary, and only when the major bottlenecks are identified through an objective and quantitative analysis of the problem.

____
## Conclusion

I hope you've learned something about Kotlin's features that allow us to help the code tell a clearer and more relatable story with fluent interfaces. There are a few more things that can be done to improve the code's fluency and readability but I'll leave that to you as an exercise.

Until next time, happy coding!
____