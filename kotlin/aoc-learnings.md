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
the window size and Kotlin will do the heaving lifting of sliding that window over the collection and return a list of
snapshots, where each snapshot is itself a list of elements in the collection that fell within the sliding window.

In Kotlin, the code above could be written using `windowed()` like this:

```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2).count { it.first() < it.last() }
```

Neat, huh? Nine lines of brute force code boiled down to one line of exquisitely elegant expressiveness.

The function expression that comes after `count` can be written with explicit parameters, too:

### No, even shorter (and better maybe?)
```kotlin
fun timesIncreased(values: List<Int>): Int = values
    .windowed(size = 2).count { a, b -> a < b }
```

This is definitely shorter than the previous version. I'll let you decide if this is better or not; maybe it's a matter
of preference at this point. I like this version, too, but for some reason, I stuck with `it`.

## Lesson #2 - You can `.also { print(it) }` to debug

_See what I did there? ;)_

In Java, quick-and-dirty debugging can be done with `System.out.println()`. This can make code messy and harder to read
because you often resort to some really ugly things just to take a peek at what's going on in your code. Plus, it's not
always easy to take a peek in the middle of things while it's happening, like in the middle of an expression, for
example. For those kind of things, you usually have to fire up the debugger. Most of the time, I'd rather not.

Kotlin has a very handy and elegant (of course) way to do this more cleanly and keep the messiness under control while
also giving you a way to get into nooks and crannies that you wouldn't normally able to access in Java. 

They're called [scoped functions](https://kotlinlang.org/docs/scope-functions.html)
and the one that's really useful for debugging, literally in the moment, is the `also` scoped function. This little
beauty allows you take a look at just about anything that's happening anywhere, even in the middle of an expression.

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
consecutive elements, because I specified `size=2`. The second line displays the result which is the value returned by
the `count` function. 

The nice thing about the `also` scope function is that it doesn't interfere with the expression it's added to at all:
it's basically a passthrough operation. It's a great way to spy on expressions as they are being evaluated.

## More lessons to come (of course)

I'll continue to update this article as I we go through Advent. Stay tuned and in the meantime, stay safe!