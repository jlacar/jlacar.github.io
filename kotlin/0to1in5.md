# From Zero to One in Five minutes

In the previous article, I showed how I solved Part 1 of the Day 8 problem in the Advent of Code.

Here, I'll go into some details of the process I went through in coming up with that solution. I wanted to do this
partly because I've kind of been flexing on my friends at JavaRanch who have been sharing their Java solutions. I'm not
trying to be a jerk or show off, but I can't help feel that by showing my Kotlin solution, it does come off as some
serious peacocking. That's the furthest thing from my mind and I hope that by sharing my process, it'll help you learn
something about Kotlin and how to quickly solve problems with it.

To recap the problem, we basically need to count how many words in a list had lengths of either 2, 3, 4, or 7. There's
an interesting story behind that but if you want to know what it is, go to the Advent of Code Day 8 problem and read it
for yourself.

## Baby steps, baby.

One of the problems I try to avoid is something I see all the time with developers I coach: they seem to want to solve
the problem all at one go. I call it "eyes too big for the stomach" syndrome. Invariably, this doesn't go well and the
solutions are messy and confusing, if you can even get to a solution that actually works.

Instead of trying to solve a complex problem all at once, I like to break it down into smaller, more easily digestible
problems. That's the approach I took with this problem: break it down and step into the solution, one baby step at a time.

### Baby step #1: split each input line in two

Each input line has two groups of words: a group of ten words separated by " | " from the second group of four words.
The first baby step I chose to make was to split the input strings into these two groups. That can be done with split("
| "). Using the lessons I learned about using also(), this was my first cut:

```kotlin
fun result(input: List<String) = input
    .map { it.split(" | ") }

val testInput = readInput("Day08_test")
result(testInput).also(::println)
```

> Developer Note: I use `also(::println)` because I already learned from previous days that it's an easy way to debug my code. When I get to the final solution, all I have to do is tack on also { check(...) } to add a safety net against regressions.

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

Great, it's clear that I've got separation of the two groups of words working now.

### Baby step #2: extract the second group of words.

This one is pretty easy: just use the `last()` function. We could use `[1]` but that's not as expressive.

```kotlin
fun result(input: List<String) = input
    .map { it.split(" | ").last() }

val testInput = readInput("Day08_test")
result(testInput).also(::println)
```

> Developer Note: I didn't specify an explicit return type for the result() function because I don't want to have to keep changing it as I build out the solution. Kotlin can infer the return type and that will carry over to the rest of the code that uses the function. If I don't say it, Kotlin will know how to interpret it. Then I can just check if what I expect matches what Kotlin actually does. This gives me rapid feedback about my understanding of the solution that I'm growing.

This is the output:

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

### Baby step #3: split into individual words

Again, this one is pretty easy: we step in another split(), this time on " ":

```kotlin
fun result(input: List<String) = input
    .map { it.split(" | ").last().split(" ") }

val testInput = readInput("Day08_test")
result(testInput).also(::println)
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

Now we have a List<List<String>>.

Next baby step.

### Baby step #4: flatten it

This was the hard part. I had to spend a couple of minutes looking through flatMap() API documentation to make sure it
does what I think it does. But a minute or two later, I had this:

```kotlin
fun result(input: List<String) = input
    .flatMap { it.split(" | ").last().split(" ") }

val testInput = readInput("Day08_test")
result(testInput).also(::println)
```

Here, too, I get the expected output:

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
Next.

### Baby step #5: count 'em'

This one is super easy. Just filter on the criteria and use `count()`.
```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .filter { it.length in digitLengths }
    .count()

val testInput = readInput("Day08_test")
result(testInput).also(::println)
```
This got me the expected output of 26.

### Baby step #6 - protect against regressions

So now that I had the solution, I added my guardrail to protect me from myself:

```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .filter { it.length in digitLengths }
    .count()

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```

Now that we have guard rail, let's move on to refactoring.

### Refactoring #1 - simplify the call chain

As noted in the previous article, IDEA was telling me that I could simplify the call chain. Ok.

```kotlin
val digitLengths = listOf(2, 3, 4, 7)

fun result(input: List<String) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in digitLengths }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```

Running again, I see I didn't break anything. Awesome! Great, IDEA, thanks for the tip!

### Refactoring #2 - rename to clearly express intent

I don't know if I succeeded with this one but anyway, I went with this:

```kotlin
val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

fun result(input: List<String) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in lengthsOfDigits1478 }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }
```
Still works. Next refactoring.

### Refactoring #3 - eliminate scaffolding

Now I need to pull the solution together and clean up the code. So far, I had this:

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
Merging the two and eliminating `result()`, I got this:
```kotlin
val lengthsOfDigits1478 = listOf(2, 3, 4, 7)

fun part1(input: List<String>) = input
    .flatMap { it.split(" | ").last().split(" ") }
    .count { it.length in lengthsOfDigits1478 }

val testInput = readInput("Day08_test")
result(testInput).also(::println).also { check(it == 26) }

val input = readInput("Day01")
println(part1(input))
```
One final run and I get my solution result, which for my data set, is 421

## Close out: add the final solution guardrail

Finally, I add a guardrail for my final solution, to support further refactoring.
```kotlin
val input = readInput("Day01")
part1(input).also(::println).also { check(it == 421) } // verified solution
```

That's it. It took way longer for me to write about it that to actually do it. Done in real time, the whole process took
about 5 minutes (maybe less).

### [<< Previous article](kotlin-oneliners.md)

{% include post-footer.md %}