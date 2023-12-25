---
layout: post
title: Advent of Code 2023, Day 7 - Refactoring Camel Cards for Fluency
author: Junilu Lacar
banner-img: /assets/images/aoc2023/07-camels/camel-cards-1.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 400
banner-height: 400
---
I've noticed a bit of a buzz on my social-vine lately about code talking, and us listening to what it's saying. Of course, that's just a figure of speech, what my high school English teacher, Ms. Dinopol, taught us is an [anthropomorphization](https://dictionary.cambridge.org/dictionary/english/anthropomorphize), where we give human characteristics to something non-human.

Developers have a proclivity for anthropomorphizing because it's useful in many ways. It's a way to cope with the many complex and abstract ideas rolling around in our heads all the time. It helps us solve difficult problems. [Rubber ducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging) immediately comes to mind as a good example of this. Fluent APIs are another.

## A Fluent Interface for Camel Cards

The Wikipedia entry for [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface) starts with this:

> "a **fluent interface** is an [object-oriented](https://en.wikipedia.org/wiki/Object_oriented_design) [API](https://en.wikipedia.org/wiki/Application_Programming_Interface) whose design relies extensively on method chaining. Its goal is to increase code legibility by creating a [domain-specific language](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL). The term was coined in 2005 by Eric Evans and [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler_(software_engineer)).

To be honest, I never knew that it was Evans and Fowler who coined the term before I saw it in Wikipedia. You'd think I would have known because I even mentioned them both the other day in [the first installment of this series](/coding/2023/12/22/aoc-day7-part1-overview.html) of articles, in the context of a fluent API. 

Of course, it makes sense that _they_ would come up with the term because a fluent API typically comes out of [refactoring](https://refactoring.com/) code to make it more readable, a Fowler thing, by using a [ubiquitous language](https://martinfowler.com/bliki/UbiquitousLanguage.html) to make it relatable, an Evans thing. I guess this is the universe telling me something again. See what I did with the anthropomorphism?

### There's always one more refactoring...

Yesterday, I was looking at [my solution](https://github.com/jlacar/aoc-2023-kotlin/blob/main/src/Day07.kt) to the [Advent of Code 2023, Day 7 puzzle](https://adventofcode.com/2023/day/7) feeling quite pleased with how it turned out. As a reminder, the puzzle asks us to find the **total winnings** of the ranked set of Camel Card plays as given by the puzzle input. This is what I had written:
```kotlin
override fun part1(): Int = totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))

override fun part2(): Int = totalWinnings(plays.sortedWith( compareBy { it.jokerStrength } ))

private fun totalWinnings(rankedPlays: List<CamelCardPlay>): Int =
    rankedPlays.mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()
```
I was happy with how these lines of [Kotlin](https://kotlinlang.org) read because it used language from the puzzle. Then it dawned on me that there's a more _fluent_ way to say it.

## Refactoring for Fluency

Putting the call to `totalWinnings()` at the end of the operation call chain would be more consistent with a fluent style, and maybe even a more idiomatic way to write it in Kotlin.

Falling back on the API-first approach, I sketched out how the code could tell its story more fluently (again with the anthropomorphizing), as comments:
```kotlin
override fun part1(): Int = totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))
// the code could say...  = plays.rankedWith(normalRules).totalWinnings()

override fun part2(): Int = totalWinnings(plays.sortedWith( compareBy { it.jokerStrength } ))
// ...this more fluently  = plays.rankedWith(jokerRules).totalWinnings()

private fun totalWinnings(rankedPlays: List<CamelCardPlay>): Int =
    rankedPlays.mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()
```
How do we refactor to the more fluent version of the code? As with all refactoring, it's best if we do it one baby step at a time.

### Step 0 - Have an idea of where you're going

Last time, I used the analogy of taking a road trip to explain my thought process when doing TDD. You don't just drive in a random direction, you have to know in what general direction you're going. The comments I put under the code clarified the direction I intended to take the code as I refactored. But instead of saying it as something I was going to do to the code, I anthropomorphized and framed it as what _the code_ could say more fluently. Read the comments again: they're not about me, they're about _the code_.

"Listening to the code" involves shifting your perspective from what _you_ want to say to what _the code_ wants to say.

Nothing is written in stone either. The end result expressed in the comment is still tentative and it's entirely possible we'd end up with something different. As we make changes, we'll keep listening to the code to see if we're on the right track and make any necessary course corrections.

Did you noticed the fair amount of anthropomorphization there? I'll stop calling out examples of my using that language device now but I hope you'll start to recognize when I'm doing it.

### Step 1 - Lay out the new beside the old

Sticking with the road analogy, the first refactoring step is similar to how a new road is built parallel to the old road it's meant to replace. Doing this minimizes inconvenience to travelers and keeps traffic flowing normally until the new road is ready to use. The alternative is to fix the old road in place, closing some or all lanes and diverting traffic to a detour. The latter approach is more disruptive and causes more grief. 

Likewise when refactoring, it's better to avoid simply tearing up the code and trying to fix it in place. In "[Refactoring Malapropism](https://martinfowler.com/bliki/RefactoringMalapropism.html)," Martin Fowler writes:

>  If somebody talks about a system being broken for a couple of days while they are refactoring, you can be pretty sure they are not refactoring...
> 
>  Refactoring is a very specific technique, founded on using small behavior-preserving transformations (themselves called refactorings). If you are doing refactoring your system should not be broken for more than a few minutes at a time...

Keeping to this discipline can lead you to TCR, a recent development from Kent Beck. TCR stands for [Test and (Commit or Revert)](https://xp123.com/articles/tdd-tcr-commits/). I'll look deeper into TCR in a future article.

Our next step, therefore, is to add a new function before we do anything about the old one. If the new function works, we'll switch to it immediately. If it breaks the program, we'll keep the old and try again with something else.

Kotlin's [extension functions](https://kotlinlang.org/docs/extensions.html) come in handy in this situation. Extension functions allow us to add functionality to an existing class, even ones we can't edit, without having to create a subclass. 

An extension function is defined just like a normal function except we prefix the name with the receiver type, the type we're extending. In this case, the type we're extending is `List<CamelCard>` so we'll create a new extension function, `totalWinnings()`, for it. 
```kotlin
override fun part1(): Int = totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))
// the code could say...  = plays.rankedWith(normalRules).totalWinnings()

override fun part2(): Int = totalWinnings(plays.sortedWith( compareBy { it.jokerStrength } ))
// ...this more fluently  = plays.rankedWith(jokerRules).totalWinnings()

// this becomes unused and can be safely deleted
private fun totalWinningsOLD(rankedPlays: List<CamelCardPlay>): Int =
    rankedPlays.mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()

// extension function allows the code to tell its story more fluently
private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum() 
```
Note that I've renamed the old function to facilitate switching to the new one. We can now try the new extension function with `part1()`:
```kotlin
override fun part1(): Int = plays.sortedWith( compareBy { it.normalStrength } ).totalWinnings()
// override fun part1(): Int = totalWinnings(plays.sortedWith( compareBy { it.normalStrength } ))
// the code could say...  = plays.rankedWith(normalRules).totalWinnings()
```
We see that all tests still pass so we apply the same change to `part2()`. Again, all the tests pass. 

We can now safely delete the `totalWinningsOLD()` function to get this:
```kotlin
override fun part1(): Int = plays.sortedWith( compareBy { it.normalStrength } ).totalWinnings()
// the code could say...  = plays.rankedWith(normalRules).totalWinnings()

override fun part2(): Int = plays.sortedWith( compareBy { it.jokerStrength } ).totalWinnings()
// ...this more fluently  = plays.rankedWith(jokerRules).totalWinnings()

// extension function allows the code to tell its story more fluently
private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum() 
```
We're now one step closer to our intended destination.

### Step 2 - Refactoring for consistency

If we stopped here, we'd have a working program but the code still tells its story somewhat inconsistently.

`sortedWith()`, a general purpose function provided by the Kotlin Standard Library, is sitting between two domain-specific ideas expressed by `plays` and `totalWinnings()`. The call chain goes from domain-specific to general, then back to domain-specific. Remember, a fluent interface uses domain-specific terms so alternating between domain and generic isn't consistently fluent.

If we could replace `sortedWith()` with a domain-specific term like `rankedWith()`, the code would tell a consistently fluent story. Let's keep refactoring and help the code do this.

### Step 3 - Using extension functions to alias other functions 

Since the `sortedWith()` part already works, we really only need to assign it an alternative domain-specific name. Extension functions are also very handy for this. Let's see how we can assign `rankedWith()` as an alias for `sortedWith()` using an extension function.

First, we rough out the extension function:
```kotlin
private fun List<CamelCardPlay>.rankedWith() : List<CamelCardPlay> {
    return sortedWith { it.normalStrength }
}
```
Since we'll be calling `rankedWith()` on the `plays` object, the extension function receiver needs to be `List<CamelCardPlay>`. Likewise, `totalWinnings()` needs a `List<CamelCardPlay>` as its receiver, so `rankedWith()` needs to return a `List<CamelCardPlay>`. This is consistent with the types involved in the `sortedWith()` call.

`sortedWith()` takes a `Comparator` argument, so we declare that too:
```kotlin
private fun List<CamelCardPlay>.rankedWith(
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)
```
I won't go into the details of the `in` in the generic type declaration of `Comparator<in CamelCardPlay>` right now. If you want to know more, look up [Kotlin generics and declaration site variance](https://kotlinlang.org/docs/generics.html#declaration-site-variance). Note also that it's now a [single-expression function](https://kotlinlang.org/docs/idioms.html#single-expression-functions).

Let's try our new extension function alias for `sortWith()` in `part1()`. Remember: step small, mess up small. We see it works so we change `part2()` to say the same thing. Great, everything still works!

We now have this:
```kotlin
override fun part1(): Int = plays.rankedWith( compareBy { it.normalStrength } ).totalWinnings()
// the code could say...  = plays.rankedWith(normalRules).totalWinnings()

override fun part2(): Int = plays.rankedWith( compareBy { it.jokerStrength } ).totalWinnings()
// ...this more fluently  = plays.rankedWith(jokerRules).totalWinnings()

// ...

private fun List<CamelCardPlay>.rankedWith(
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)
```
That's another small step toward a more fluent interface.

### Step 4 - Extracting an explaining variable

We're on the final stretch now! There's one more thing in the call chain that can be more fluent: `compareBy { ... }`. This is again a generic function provided by the standard Kotlin library. To make it domain-specific, we extract it to an explaining variable:
```kotlin
override fun part1(): Int {
    val normalRules: Comparator<CamelCardPlay> = compareBy { it.normalStrength }
    
    return plays.rankedWith(normalRules).totalWinnings() 
    // the code could say...  = plays.rankedWith(normalRules).totalWinnings()
} 
```
This works! It looks like we've arrived! We can now delete the comment that has been made redundant by the code. After applying the change to `part2()`, the code now tells a very fluent story.

```kotlin
override fun part1(): Int {
    val normalRules: Comparator<CamelCardPlay> = compareBy { it.normalStrength }

    return plays.rankedWith(normalRules).totalWinnings()
}

override fun part2(): Int {
    val jokerRules: Comparator<CamelCardPlay> = compareBy { it.jokerStrength }

    return plays.rankedWith(jokerRules).totalWinnings()
}

private fun List<CamelCardPlay>.rankedWith(
    comparator: Comparator<in CamelCardPlay>
): List<CamelCardPlay> = sortedWith(comparator)

private fun List<CamelCardPlay>.totalWinnings(): Int =
    mapIndexed { rank, play -> (rank + 1) * play.bid }.sum()
```

## Conclusion - The benefits of giving the code a voice

I don't know if I've convinced you to start thinking of code as something that can tell a story and express its own will. I know seeing code this way has helped me in numerous ways over the years. Here's a short list:

1. It helped me adopt an [egoless programming](https://en.wikipedia.org/wiki/Egoless_programming) mindset. When I think of code as its own entity, I can separate my "self" (ego) from it. Once an idea is expressed by the code, it has to stand on its own merits. With less ego attached to the code, I can take criticism of the code more objectively. This makes me less likely to get defensive and it makes collaborative development like pair and mob programming much better. It also opens up many doors to refactoring because with objectivity, I can be ruthless in refactoring the code.
2. I often tell people to read the code out loud, to give it a voice. There's something about actually hearing an idea expressed out loud versus having our "inner voice" say it in our heads. Our inner voice subconsciously provides context to help us understand things. This happens in the background and can lead to some insidious problems. An external voice, especially one that's not our own, doesn't do that. It has to provide more context, meaning, and clarity to the idea for it to be understood properly. People often quote Llewellyn Falco's assertion that "for an idea to go from your head to the computer, it MUST go through someone else's hands." I have found the converse to be also true: For an idea to go from the computer to your head, it MUST go through someone else's mouth.
3. Thinking of the code as someone who has something to say helps me build sensitivity and empathy for how others might interpret the code. When I imagine myself sitting across the table from the code as it explains itself to me, I can also imagine someone else sitting in my place. Will they understand what the code is saying if they didn't know what I know about it? If not, how can the code tell its story better so they would?

There are a few more things I could cover but this is getting pretty long and it's Christmas morning, so I'll end here for now.

Advent of Code 2023 officially ends today and I'm still working on Day 10, still blogging about Day 7. I don't mind because I've gained a lot of good insights and learned more Kotlin.

Have a Blessed and Merry Christmas, everyone!
