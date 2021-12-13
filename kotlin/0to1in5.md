# From Zero to One in Five minutes - How to Eat an Elephant in Kotlin

This is the third of a series of posts about things I've learned about Kotlin while doing
the [Advent of Code 2021](https://adventofcode.com). In the [previous post](kotlin-oneliners.md), I shared my one-liner
solution to Part 1 of the Day 8 problem. In this post, I'll go over the process I followed to get that solution.

I had fallen a few days behind on the puzzles and only got around to starting on the Day 8 problem earlier today, just a
couple of hours into Day 12. I was prepared to spend an hour or so working on a solution but was pleasantly surprised
when I got the answer in about five minutes.

I posted my one-liner solution
to [the Day 8 Solutions thread at CodeRanch](https://coderanch.com/t/747889/AoC-Day-Solutions-Spoilers) but felt a
little [_diyahe_*](https://hinative.com/en-US/questions/145497) about the brevity of my solution compared to some of the
ones my friends at the ranch had shared. But I was still very happy to have found yet another way to show how expressive
Kotlin programs can be.

To ease my feelings of _diyahe_, I wanted to share the details of how I ate what I thought was going to be an
elephant-sized problem with relative ease in a very short amount of time.

> *_diyahe_ is a Filipino term that can be used to refer to that feeling that's not exactly shame but more of that awkward embarrassment when you feel bad about making someone feel bad.

## Baby steps, baby.

When I solve a non-trivial problem, I typically start by breaking it down to smaller, more manageable problems that I
can attack one at a time. This way I can usually avoid being overwhelmed by complexity. It's that whole eating an
elephant one bite at a time thing. This problem was no different.

First, I started with the input, which was a list of strings read from a file.

## Baby step #1: split each input line in two

Each line in the input was made up of two groups of words: a group of ten words separated by " | " from a second group
of four words. If you want to know the whole story behind this peculiar format, go to the Advent of Code website and
read the Day 8 problem.

The first baby step I chose to make was splitting each line in the input into these two parts. That can be done
with `split(" | ")`. I expected this to produce a `List<List<String>>` where each nested list of strings would have two
elements, one for each part.

I learned from previous days that the `also()` function is a great debugging tool so I used it to see what the
intermediate result was. This was my first cut:

```kotlin
fun result(input: List<String>) = input
    .map { it.split(" | ") }

val testInput = readInput("Day08_test")
result(testInput).also(::println)
```

> Developer Note: Eventually, once I found the solution, I would tack on an `.also { check() }` after `.also(::println)` to act as a guardrail against regressions. This is a pattern I had established while solving the problems in previous days.

This is what the output looked like using the data from the problem example (formatted for clarity):

```text
[[be cfbegad cbdgef fgaecd cgeb fdcge agebfd fecdb fabcd edb, fdgacbe cefdb cefbgd gcbe],
 [edbfga begcd cbg gc gcadebf fbgde acbgfd abcde gfcbed gfec, fcgedb cgb dgebacf gc], 
 [fgaebd cg bdaec gdafb agbcfd gdcbef bgcad gfac gcb cdgabef, cg cg fdcagb cbg], 
 [fbegcd cbd adcefb dageb afcb bc aefdc ecdab fgdeca fcdbega, efabcd cedba gadfec cb], 
 [aecbfdg fbg gf bafeg dbefa fcge gcbea fcaegb dgceab fcbdga, gecf egdcabf bgf bfgea], 
 [fgeab ca afcebg bdacfeg cfaedg gcfdb baec bfadeg bafgc acf, gebdcfa ecba ca fadegcb], 
 [dbcfg fgd bdegcaf fgec aegbdf ecdfab fbedc dacgb gdcebf gf, cefg dcbef fcge gbcadfe], 
 [bdfegc cbegaf gecbf dfcage bdacg ed bedf ced adcbefg gebcd, ed bcgafe cdgba cbgef], 
 [egadfb cdbfeg cegd fecab cgb gbdefca cg fgcdab egfdb bfceg, gbdfcae bgc cg cgb], 
 [gcafb gcf dcaebfg ecagb gf abcdeg gaef cafbge fdbac fegbdc, fgae cfgab fg bagce]]
```

Great, I got what I expected, now on to the next step.

## Baby step #2: extract the second group of words.

The next step was to focus on the second group on each line, the group that had four words. For this, I used the
`last()` function. I could have used `[1]` but that's not as expressive. Now we have this:

```kotlin
fun result(input: List<String>) = input
    .map { it.split(" | ").last() }
```

> Developer Note: I didn't specify an explicit return type for the `result()` function because I didn't want to have to keep changing it as I built out the solution. By letting Kotlin infer the return type, I could check my understanding of what I had written so far. If Kotlin inferred the type I expected it to infer, then I knew I understood what was going on. Otherwise, there was some kind of misunderstanding on my part that I needed to clarify immediately so I could avoid moving forward with the wrong idea.

This is the output after adding `.last()` to the solution:

```text
[fdgacbe cefdb cefbgd gcbe, 
 fcgedb cgb dgebacf gc, 
 cg cg fdcagb cbg, 
 efabcd cedba gadfec cb, 
 gecf egdcabf bgf bfgea, 
 gebdcfa ecba ca fadegcb, 
 cefg dcbef fcge gbcadfe, 
 ed bcgafe cdgba cbgef, 
 gbdfcae bgc cg cgb, 
 fgae cfgab fg bagce]
```

Cool. Exactly what I wanted. Next baby step.

## Baby step #3: split into individual words

This was also straightforward: I step in `split(" ")` because the words are separated by a space. I now had this:

```kotlin
fun result(input: List<String>) = input
    .map { it.split(" | ").last().split(" ") }
```

This is the output:

```text
[[fdgacbe, cefdb, cefbgd, gcbe], 
 [fcgedb, cgb, dgebacf, gc],
 [cg, cg, fdcagb, cbg],
 [efabcd, cedba, gadfec, cb],
 [gecf, egdcabf, bgf, bfgea],
 [gebdcfa, ecba, ca, fadegcb],
 [cefg, dcbef, fcge, gbcadfe],
 [ed, bcgafe, cdgba, cbgef],
 [gbdfcae, bgc, cg, cgb],
 [fgae, cfgab, fg, bagce]]
```

This was as expected and now we have a `List<List<String>>`.

Next baby step.

## Baby step #4: flatten it

This was the hard part. I had to spend a couple of minutes looking through the `flatMap()` API documentation to make
sure it does what I thought it does. A minute or two later, I had this:

```kotlin
fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
```

This, too, gave the expected output:

```text
[fdgacbe, cefdb, cefbgd, gcbe, 
 fcgedb, cgb, dgebacf, gc, 
 cg, cg, fdcagb, cbg, 
 efabcd, cedba, gadfec, cb, 
 gecf, egdcabf, bgf, bfgea, 
 gebdcfa, ecba, ca, fadegcb, 
 cefg, dcbef, fcge, gbcadfe, 
 ed, bcgafe, cdgba, cbgef, 
 gbdfcae, bgc, cg, cgb, 
 fgae, cfgab, fg, bagce]
```

Oh yeah, next step, baby.

## Baby step #5: `count()` 'em, Jack.

This one was super easy. Part 1 needed me to count how many of these words had specific lengths. These words were
supposed to correspond to digits (0-9) on a display and some of them had unique lengths by which you could identify the
digit being represented. The unique lengths were 2 (for a word representing 1), 3 (for a word representing 4), 4 (for 7)
, and 7 (for 8). 

If that's clear as mud to you, just read the problem at the AoC website to get the whole story because
they probably tell it far better than I can.

Anyway, to get a count of words that were any of those lengths, I could apply a `filter` and then `count()` how many there were
that satisfied the predicate.

This time, I was confident enough with what I was doing to take two steps instead of the usual one. But first, I defined
the criteria as an immutable list since it was essentially a constant.

```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .filter { it.length in digitLengths }  // step 5a
    .count()                               // step 5b
```

This finally got me the expected output of 26, the answer to the problem using the example data.

Almost there...

## Baby step #6 - protect against regressions

The first thing to do after arriving at the solution was to add `.also { check(it == 26) }` as a guardrail to protect me
from myself, in case I messed anything up while refactoring.

```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .filter { it.length in digitLengths }
    .count()

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```

With that guardrail in place, I could safely move on to refactoring.

## Refactoring #1 - simplify the call chain

IDEA was telling me that I could simplify the call chain. In other words, `filter()` was redundant and could be
eliminated.

```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in digitLengths }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```

Didn't break anything with this, so I moved on to the next refactoring.

## Refactoring #2 - rename to clearly express intent

The `digitLengths` name seemed a little too general so I decided to rename it to express its purpose better. I don't
know if I succeeded because you have to know from reading the problem that the digits 1, 4, 7, and 8 corresponded to
words that were 2, 3, 4, and 7 characters long, respectively. But I went with this anyway:

```kotlin
val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in lengthsOfDigits1478 }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```

Still works. Next refactoring.

## Baby step #7 - eliminate scaffolding

Now I needed to pull the solution together and claim my gold star. So far, I only had confirmation that the solution
worked with the sample data. I still didn't have the solution to the problem using the actual data set I was given.

All this while, I had been working with the `result()` function to incrementally build out a solution. It was
essentially scaffolding I used to build confidence and understanding in what I was trying to do. Now that I was
confident I had the right solution, it was time to tear down the scaffolding and solve the problem for real.

This is what my work in progress looked like:

```kotlin
val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in lengthsOfDigits1478 }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }

fun part1(input: List<String>): Int {
    return input.size
}

val input = readInput("Day01")
println(part1(input))
```

The `part1()` function was still doing what it does out of the template box. So I made `part1()` mirror `result()`, then
redirected execution to `part1()`. This made `result()` redundant and therefore safe to delete. After the quick cleanup,
I now had this:

```kotlin
val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

fun part1(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in lengthsOfDigits1478 }

val testInput = readInput("Day08_test")
part1(testInput).also(::println).also { check(it == 26) }

val input = readInput("Day01")
println(part1(input))
```

One final run and I had my Day 8 Part 1 solution, which was 421 for my data set. With the final answer to Part 1 copied
to my clipboard, I pasted it back into the Advent of Code website and claimed my gold star.

"What? Done already?" I thought, pleasantly surprised. Cool.

## Close out: add the final guardrail for the solution

Finally, I added another guardrail, `.also { check(it == 421) }`, to fully protect myself from regressions during any
future attempts to refactor.

```kotlin
val input = readInput("Day01")
part1(input).also(::println).also { check(it == 421) } // verified solution
```

That's it.

It took me longer to write about it than to actually do it. In real time, the whole process took about five minutes,
maybe even less. What seemed like an elephant at first turned out to be a mouse. Well, maybe a few small mice.

I don't know if you could call that process TDD, but it certainly was incremental and involved a lot of feedback. And
that, my friends, is how I ate (what I thought was) an elephant in Kotlin: one small bite at a time, with generous
helpings of feedback and checks of my understanding.

### [<< Previous article](kotlin-oneliners.md)

{% include post-footer.md %}