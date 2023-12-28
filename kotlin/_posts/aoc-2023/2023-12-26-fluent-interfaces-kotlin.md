---
layout: post
title: Fluent Interfaces with Kotlin
author: Junilu Lacar
banner-img: /assets/images/aoc2023/07-camels/camel-cards-3.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 370
banner-height: 370
---
Kotlin has some really nice features that make it easy to write clear, expressive code. These features help you create more fluent interfaces and domain-specific languages (DSL) or mini-languages that can improve the code's clarity.

In this second installment of a planned series of articles, I'll show how I used some of those features as I continued to refactor my initial solution to the [Day 7 puzzle](https://adventofcode.com/2023/day/7) of [Advent of Code (AoC) 2023](https://adventofcode.com/2023).

You can see the full code and its change history in [my GitHub repo for AoC 2023](https://github.com/jlacar/aoc-2023-kotlin/blob/main/src/Day07.kt).

## A fluent interface for Camel Cards

The [Wikipedia entry for "fluent interface"](https://en.wikipedia.org/wiki/Fluent_interface) starts with this short paragraph:
> In software engineering, a **fluent interface** is an [object-oriented API](https://en.wikipedia.org/wiki/Object_oriented_design) whose design relies extensively on [method chaining](https://en.wikipedia.org/wiki/Method_chaining). Its goal is to increase code legibility by creating a [domain-specific language](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL). The term was coined in 2005 by Eric Evans and [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler_(software_engineer)).

In [another article](/coding/2023/12/22/aoc-day7-part1-overview.html), I mentioned how a DSL can help align people's understanding by giving them a common set of words and phrases to use for communicating ideas. The more aligned a program is to the language of the domain, the less translation needs to happen between one set of terms and another, i.e., from techie-speak to nontechie-speak and vice versa.

A DSL can also make it easier for developers to organize their thoughts and ideas in the program by suggesting logical groupings of software components, business rules, processes, and data. This can lead to more coherent and cohesive code and designs.

### A short introduction to the Camel Cards domain

The full description and rules for Camel Cards can be found on the [AoC 2023 Day 7 puzzle page](https://adventofcode.com/2023/day/7). For our purposes here, you just need to know that Camel Cards is like Poker, except with a few twists. There are different **types of hands**, like **five of a kind** and **full house**. Hands are **ranked** relative to each other according to **rules** that govern how **strength** is assigned to each hand. **Winnings** are calculated based on a hand's **rank** and the **bid** made for it. 

The emphasized words and phrases in the previous paragraph are specific to the domain of the Camel Cards puzzle and help define a domain-specific language for it.

### The non-fluent solution

Listing 1 below shows the code I used to earn two more gold stars for AoC 2023 with correct answers to the Day 7 puzzle. As an added bonus, I thought the code was quite clear and expressive because it used language from the puzzle's description. 

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
#### **Listing 1**. The code to be refactored to a more fluent interface
____

I was quite happy with this code until I realized that the call to `totalWinnings()` could be written in a more fluent style, one that would probably be a more idiomatic way to write it in Kotlin. 

Time for some refactoring. 

Before we do that though, let's get something straight about the term "refactoring."

### You keep using that word. I don't think it means what you think it means

As a technical term whose use has spread far and wide, refactoring suffers quite a bit from what Martin Fowler calls [semantic defusion](https://martinfowler.com/bliki/SemanticDiffusion.html). This is unfortunate, so I try to do my part to correct the [common malapropism of refactoring](https://martinfowler.com/bliki/RefactoringMalapropism.html) and practice the technique in the way that Fowler actually means for it to be done. 

Fowler writes this about mischaracterizing _any_ act of changing code as "refactoring," as people who misuse the term often do:

>  If somebody talks about a system being broken for a couple of days while they are refactoring, you can be pretty sure they are not refactoring...
> 
> Refactoring is a very specific technique, founded on using small behavior-preserving transformations (themselves called refactorings). If you are doing refactoring your system should not be broken for more than a few minutes at a time...

I want to emphasize some things Fowler says there about refactoring:

1. _It's founded on **small** transformations_. Refactoring is done in small steps. Usually, smaller than what you think "small" is. [Renaming](https://www.jetbrains.com/help/idea/rename-refactorings.html) is usually a small change that can be done in one step with most modern IDEs that support automated refactoring. More complex refactorings, like [replacing conditionals with polymorphism](https://refactoring.guru/replace-conditional-with-polymorphism), typically cannot be performed in one step, requiring several interconnected steps to complete. Unfortunately, most programmers have neither the patience nor the discipline to do this safely, so these kinds of refactorings are performed less frequently and the opportunity to improve the design often missed.

2. _It's founded on **behavior-preserving** transformations_. Refactoring does not change a program's externally observable behavior. Refactoring properly and safely means the program should pass all its tests _before_ and _after_ the change is made to it.

3. _Your system **should not be broken** for more than a few minutes at a time_. I would even go farther and assert that ideally, it shouldn't be broken at all. Sometimes, however, having the program in a broken state for a few minutes can expedite the work. I would advise you to be careful in doing this: the longer the code stays broken, the longer it takes to get useful feedback from it. It's usually better to revert the breaking changes and try again before things get worse.

Keeping with the discipline of refactoring as described above can lead to Test and (Commit or Revert). TCR is a workflow [introduced a few years ago by Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864). I'll take a more detailed look at this TDD-related alternative workflow in a future article.

### Mapping out where we want to go

When I go on a road trip, I don't just start driving in any random direction. When I head out, I usually have a good idea of the general direction I'm going. Modern technology has made road trips more or less worry-free: just enter the destination address into your phone's GPS app and you'll get turn-by-turn directions all the way there. You even get warned about hazards and detours that might slow you down.

Modern IDEs like IntelliJ IDEA and Eclipse have come a long way to make refactoring easier and safer. Many small refactorings can be easily performed automatically and safely with a few keystrokes or mouse clicks. Unfortunately, larger and more complex refactorings that involve multiple steps still require a fair amount of skill, experience, intuition, and often, serendipity to get the code to a better place.

Large refactorings can be quite challenging, especially when the transitions from one step to the next are not very obvious. To stay on track and maintain a general sense of direction, I'll typically sketch a path in the code. When doing TDD, I use tests to do these sketches. 

Luckily, in this case there weren't that many steps needed to refactor the code to a fluent interface. It was enough to imagine the end state and use comments to guide me to the final destination and keep me from wandering off track as I slowly worked my way toward it. 

Listing 2 below shows the comments I added to guide my refactoring journey.

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
#### **Listing 2**. Using comments to map out intent and guide refactoring
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

Note that the only difference so far is the call to `totalWinnings()`. The rest of the call chain remains the same. We'll deal with those parts later. Right now, our focus is on using the extension function that calculates total winnings as the terminal operation in the call chain.

We run the tests that were already passing before and confirm we haven't broken anything. Great, we can now apply the same change to `part2()`. Doing so makes `totalWinningsOLD()` unused so we can safely delete it. After tidying up, we get the code shown below in Listing 5.

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

```
#### **Listing 5**. After completing Step 1 and tidying up
____

### Step 2 - Refactoring the code to make it consistently fluent

We can now shift our attention to the next bit of non-fluency in the call chain: the call to `sortedWith(...)`. This is a general-purpose function provided by the Kotlin Standard Library and it's currently sitting between the two domain-specific ideas of `plays` and `totalWinnings()`. This alternating shift of context from domain-specific to general and back to domain-specific makes the call chain's fluency inconsistent.

To make the chain more consistently fluent, we'll define another extension function to use instead of `sortedWith()`. This new extension function, which we'll name `rankedWith()` as our refactoring map suggests, will serve as an alias for the general-purpose name. 

The receiver type for `rankedWith()` is the type of the `plays` object, `List<CamelCardPlay>`. Its return type needs to be `List<CamelCardPlay>` because we're chaining it with `totalWinnings()`.

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

Note that we've declared the parameter for `rankedWith()` as a `Comparator<in CamelCardPlay>`, the type of the `compareBy()` expression currently passed to `sortedWith()`. The `rankedWith()` function will take the same parameter. 

I won't go into the details of the generic `Comparator<in CamelCardPlay>` declaration but if you're curious, you can [read more about Kotlin generics and declaration site variance](https://kotlinlang.org/docs/generics.html#declaration-site-variance).

Taking another small step, we try `rankedWith()` in `part1()` to see if it works. It does, so we make the same change to `part2()`. We then tidy up and get the code shown in Listing 7 below.

The end, as we mapped it out when we began this refactoring journey, is now in sight.

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

private fun List<CamelCardPlay>.rankedWith(
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)   

```
#### **Listing 7**. Tidied up code after replacing `sortedWith()` with `rankedWith()`
____

### Step 3 - Extracting to an explaining variable

The story the code tells now is still a little inconsistent. The `compareBy {...}` parts are once again calls to a general-purpose function provided by the Standard Kotlin Library. We'd like to use a domain-specific term in its place to make the story told by the call chain completely fluent and completely match the code we sketched out at the beginning.

We can do this by [extracting the expression to an explaining variable](https://refactoring.guru/extract-variable).

Again, we try it with `part1()` first and see all tests pass before we make a similar change to `part2()`.

The code now says exactly what our guiding comments at the top say. We've achieved our refactoring goal and can now delete those guiding comments. After tidying up, we end up with the code in Listing 8 below.

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
#### **Listing 8**. The tidied up code, refactored to a fluent interface
____

### Optimize for reading first 

I'm sure there will be someone who will question the value of this refactoring because it ended up with more lines of code, implying that having more code is a bad thing. 

Certainly, simplicity favors less code, but more code is not necessarily always a bad thing. Look through [Martin Fowler's books on refactoring](https://martinfowler.com/books/refactoring.html) and you'll see a few examples like this, particularly in the first long refactoring example he gives at the beginning of the book. 

The primary objective is readability, and sometimes that comes at the cost of brevity. I think it's misguided to make judgements based solely on the number of lines of code or even the number of function or method calls made. 

In his recently released book, [_Tidy First?_](https://tidyfirst.substack.com/), Kent Beck wrote:
> The biggest cost of code is the cost of reading and understanding it, not the cost of writing it.

Our main priority as developers should be to make the code as easy to read and understand as possible. Optimizing for performance comes later, and only when there's clear and compelling evidence that the program's performance is not acceptable. 

Remember what [Sir Tony Hoare](https://en.wikipedia.org/wiki/Tony_Hoare) (popularized by [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth)) said:
> Premature optimization is the root of all evil.

As developers, we generally suck at using gut feeling and intuition for performance tuning. Any decision to optimize for performance at the cost of readability should be based on quantitative measures. Use a profiler to gather empirical data that clearly shows a problem. Profiling will help you find where the true performance bottlenecks are, and they're usually not where you _think_ they are. 

Make readability and understandability the priority. Optimize for performance later, only when necessary, and only when the major bottlenecks are identified through an objective and quantitative analysis of the problem.

____
## Conclusion

I hope you've learned something about Kotlin's features that allow us to help the code tell a clearer and more relatable story with fluent interfaces. There are a few more things that can be done to improve the code's fluency and readability but I'll leave that to you as an exercise.

Until next time, happy coding!
____