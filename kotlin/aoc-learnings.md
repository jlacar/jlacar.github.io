# Things I learned about Kotlin from Advent of Code

_(December 2021)_

This year, I'm participating in the [Advent of Code](https://adventofcode.com/2021/), a wonderful event that started in
2015 and has grown exponentially since. Little wonder because it's a great place to find challenging and fun ways to
practice and learn. AoC happens every December during Advent (duh!) from December 1st to the 25th. Just head on over to
their website to learn more about it.

It has been a while since I first learned Kotlin and I haven't done anything much in it recently so this was a perfect
way to brush up on it. In retrospect, that was a great decision. I'm having a lot of fun re-learning Kotlin and
appreciating just how enjoyable it is to program in it. I'm also discussing the solutions with my friends
at [CodeRanch](https://coderanch.com) in the [Programming Diversions](https://coderanch.com/f/71/Programming) forum.

## Lesson #1 - Kotlin is terse but extremely expressive

_**There's usually a better and shorter way to say it in Kotlin. No, even better and shorter.**_

Case in point: Day 01 of AoC had us solving a relatively simple counting problem. Given a series of numbers, count how
many times the series increases from one number to the next. For example, the sequence `0,1,2,1,3,4,4,5` increases 5
times: 0-1, 1-2, 1-3, 3-4, and 4-5.

My first solution worked and earned me my first gold star but it was brute force and ugly:

```kotlin
fun timesIncreasedBrute(nums: List<Int>): Int {
    var last = 0
    var count = 0
    nums.forEach {
        if (it > last) {
            count += 1
        }
        last = it
    }
    return count - 1
}
```

Of course, that could be written in a much shorter and clearer way.

### Solving it with `windowed()`

The Kotlin `windowed()` function allows you to take snapshots of a collection with a sliding window. You just specify
the window size and Kotlin will do the heavy lifting of sliding that window over the collection and return a list of
snapshots, where each snapshot is itself a list of elements in the collection that fell within the sliding window. By
default, the window will slide one element at a time but you can specify a different step size. For this problem, the
default step size of 1 was exactly what I needed.

In Kotlin, the code using `windowed()` is straightforward and clear:

```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2).count { it.first() < it.last() }
```

The function after count is a predicate that acts as a filter for the items that will be counted. Only the elements that
satisfy the condition will be counted.

Neat, huh? Nine lines of brute force code boiled down to one line of exquisitely elegant expressiveness.

### No, even shorter (and better maybe?)

The function expression that comes after `count` can be written with explicit parameters, too:

```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2).count { a, b -> a < b }
```

This is definitely shorter than the previous version. I'll let you decide if this is better or not; maybe it's a matter
of preference at this point. I like this version, too, but for some reason, I stuck with `it`.

## Lesson #2 - You can `.also { print(it) }` to debug

_See what I did there? ;)_

In Java, a quick-and-dirty way to debug code is with `System.out.println()` statements. However, this can create a lot
of clutter in your code. Also, you often end up doing some really ugly things just to take a peek at what's going on in
your code like creating lots of temporary variables. And it's not always easy to take a peek while in the middle of
doing something, like evaluating intermediate calculation results in the middle of an expression. For that, you have
to fire up the debugger and start stepping through the code. If at all possible, I'd rather not resolt to that.

Kotlin has a very handy and elegant (of course) way to do that more cleanly and keep the messiness under control while
also giving you a way to get into nooks and crannies that you wouldn't normally be able to access in Java. The best
thing about it is that it makes the debugger even less relevant, which is awesome.

They're called [scoped functions](https://kotlinlang.org/docs/scope-functions.html)
and the one that's really useful for debugging, literally in the moment, is the `also` scoped function. This little
beauty allows you take a peek at just about anything that's happening anywhere, even in the middle of an expression.

Here's an example of how to get a window (pun intended) into what was happening in the code we had earlier.

```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2)
        .also { println(it) } // inline debug statement!
    .count { it.first() < it.last() }
        .also { println(it) } // inline debug statement!

timesIncreased(listOf(0, 1, 2, 1, 3, 4, 4, 5))
```

Here's what the output looks like:

```text
[[0, 1], [1, 2], [2, 1], [1, 3], [3, 4], [4, 4], [4, 5]]
5
```

Notice how I indented the `also`s and put them on separate lines. This makes it easy to comment them out or delete them
later when I'm done debugging.

The first line of the output gives you an idea of what `windowed()` does. In this case, it creates a list of pairs of
consecutive elements, because I specified `size=2`. The second line displays the value returned by the `count` function.

The nice thing about the `also` scoped function is that it doesn't interfere with the expression it's added to at all:
it's basically a passthrough operation. It's a great way to spy on expressions as they are being evaluated.

### Use string interpolation with `println()` for cleaner, clearer messages

Those debug statements can be made even clearer and you can do it much more cleanly than you could with Java. Just use
Kotlin's handy
dandy [string interpolation](https://kotlinlang.org/docs/java-to-kotlin-idioms-strings.html#concatenate-strings).

```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2)
        .also { println("windowed(2): $it") } // inline debug statement!
    .count { it.first() < it.last() }
        .also { println("timesIncreased() == $it") } // inline debug statement!

timesIncreased(listOf(0, 1, 2, 1, 3, 4, 4, 5))
```

Here's what the output looks like now:

```text
windowed(2): [[0, 1], [1, 2], [2, 1], [1, 3], [3, 4], [4, 4], [4, 5]]
timesIncreased() == 5
```

Pretty useful, right? You could `also` say `it`'s pretty _and_ it's useful. (I'm a dad so naturally, I think I'm so
punny)

## Lesson #3 - Use the built-in `check()` function for quick testing

Another thing I learned about was the built-in `check()` function, which takes a boolean expression and throws
an `IllegalStateException` if the argument evaluates to `false`. This is kind of a poor man's `assert` which I
know the [Kotlin standard library](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/assert.html) also provides. 

The jury is still out on this one for me but it came out of the box with the  
[Advent Of Code Kotlin Template](https://github.com/kotlin-hands-on/advent-of-code-kotlin-template) on GitHub so I just
rolled with it.

In the template, `check()` was used to see if your solution matched what the problem's example said
to expect. Out of the box, this what you got in the template:

```kotlin
 // test if implementation meets criteria from the description, like:
 val testInput = readInput("DayXX_test")
 check(part1(testInput) == 1)

 val input = readInput("DayXX")
 println(part1(input))
 println(part2(input))
```

You'd change the number in the condition to match the problem's example. Presumably, the idea is that you'd first
develop a solution and try it out with the example data. Then if it matches, you could go ahead and solve the problem
using the actual data set, which is much larger than the example data.

Using `check` with `println` and `also` was convenient both while developing the solution and when refactoring
the code afterwards. Once I got my solution verified and earned my gold star, I would do this:

```kotlin
  println(part1(testInput).also { check(it == 37) }) // from the example

  ...
  println(part1(input).also { check(it == 5608) }) // verified solution
  println(part2(input).also { check(it == 20299) }) // verified solution
```

This way, I would know right away if any refactoring I attempted on the already working solution had screwed something
up and I could then revert to the last working version of the code.

## More lessons to come

I'll continue to update this article throughout Advent. Stay tuned and in the meantime, stay safe!

{% include post-footer.md %}