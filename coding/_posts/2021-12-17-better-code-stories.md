---
layout: post
title: Better Code Tells a Story
author: Junilu Lacar
banner-img: /assets/images/socialcut-stories.jpg
banner-alt: Image by Social Cut on Unsplash.com
banner-width: 640
banner-height: 397
---

I first heard the idea of telling a story in code from Kent Beck in [SE Radio Episode #167](https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/). Kent was explaining how a test should have an arc of a story, with a clear beginning, middle, and end. 

Thinking of code as a story you're trying to tell adds another dimension to how you approach a problem and how you design and organize a solution around it. 

These are the Four Cs that help me tell a good story in code.
- **Context** - there should be enough information in the immediate area that around an interesting section of code that can give meaning to  a piece of code 
Coherence
Consistency
Conciseness



. These are some things I often say:

_"Make the code tell a story"_

_"What story is the code telling us?"_

_"Listen to the code's story. Is it clear to you what's going on?"_

_"Does that code talk about its intent or implementation?"_

_"How can we tell that story with this code?"_

## Technical Debt Detector

Telling stories in code also helps you find technical debt that needs to be addressed. 

Ward Cunningham, who coined [the Debt Metaphor](https://www.youtube.com/watch?v=pqeJFYwnkjE), defines technical debt as essentially any lack of understanding in the code. As Tim Ottinger puts it, technical debt is the difference between what we know now and what we knew when we wrote the code. In other words, technical debt is any gap in what the code says and what it _should_ say.

I find it easier to detect those gaps of understanding in the code's story when we read it out loud. When we read the code silently, our brains will compensate for those gaps by filling in the context. When you're listening to yourself or someone else tell the story, however, the gaps become clear.

## How does that work?

In a pairing or teaming situation, reading the code out loud is a great way to get alignment and creating shared understanding. Any disagreements on what the code's story is should be discussed until you can all agree on what the story should be. That consensus should be immediately reflected in the code.

## Examples of Conversationally CLEAR Code

```kotlin
    val knownSignalLengths = mapOf(1 to 2, 4 to 4, 7 to 3, 8 to 7)

    fun MutableList<Set<Char>>.setKnownSignalPatterns(signals: List<Set<Char>>) {
        knownSignalLengths.forEach { (digit, length) ->
            this[digit] = signals.first { it.size == length }
        }
    }
    
    fun MutableList<Set<Char>>.deduceSegments(signals: List<Set<Char>>, 
                                              selectors: Map<Int, (Set<Char>) -> Boolean>) =
        selectors.forEach { (digit, deduce) -> this[digit] = signals.first { deduce(it) } }
    
    fun MutableList<Set<Char>>.deduce5and6Segment(signals: List<Set<Char>>) {
        deduceSegments(signals, selectors = mapOf<Int, (Set<Char>) -> Boolean>(
            2 to { signal -> signal.size == 5 && (this[4] - signal).size == 2 },
            3 to { signal -> signal.size == 5 && (this[7] - signal).isEmpty() },
            6 to { signal -> signal.size == 6 && (this[7] - signal).size == 1 },
            9 to { signal -> signal.size == 6 && (this[4] - signal).isEmpty() }
        ))
    }
    
    fun MutableList<Set<Char>>.deduceRemaining(signals: List<Set<Char>>) {
        deduceSegments(signals, selectors = mapOf<Int, (Set<Char>) -> Boolean>(
            5 to { signal -> signal.size == 5 && signal !in this.slice(listOf(2, 3)) },
            0 to { signal -> signal.size == 6 && signal !in this.slice(setOf(6, 9)) }
        ))
    }
```

The intent of the above function is to deduce the mappings for 5- and 6-segment signals. This is the context from which
it is called:

```kotlin
    fun decoderFor(signals: List<Set<Char>>): List<Set<Char>> =
    buildList() {
        addAll(List(10) { emptySet() })
        setKnownSignalPatterns(signals)
        deduce5and6Segment(signals)
        deduceRemaining(signals)
    }
```

Telling the story: The `decoderFor()` function is going to:

* **_build a list_** of **_signals_** (`buildList()`)
* by first initializing a **_list of 10_** elements (`List(10)`)
* each element being a placeholder for a mapped signal (`{ emptySet() }`).
* Then, it will **_set the known signal patterns_**, (`setKnownSignalPatterns(signals)`)
* after which it can **_deduce the 5- and 6-segment signals_**, (`deduce5and6Segment(signals)`)
* then finally **_deduce the remaining signals_**. (`deduceRemaining(signals)`)

Anyone following along with the telling of the code's story can easily map what is being said to the code. If you
understand the story as told, it's easy to read along because the code is CLEAR.

Compare that with a previous revision of the code:

```kotlin
    /* signals with size == 5 : (2, 3, 5) */

fun deduce2(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 5 && (decoder[4] subtract it).size == 2 }

fun deduce3(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 5 && (decoder[4] - decoder[2] - it).size == 1 }

fun deduce5(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 5 && it !in decoder.slice(setOf(2, 3)) }

/* signals with size == 6 : (6, 9, 0) */

fun deduce6(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 6 && (it subtract decoder[7]).size == 4 }

fun deduce9(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 6 && (it - (decoder[4] union decoder[7])).size == 1 }

fun deduce0(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
    .first { it.size == 6 && it !in decoder.slice(setOf(6, 9)) }

fun MutableList<Set<Char>>.set1_4_7_8(signals: List<Set<Char>>) {
    this[1] = signals.first { it.size == 2 }
    this[4] = signals.first { it.size == 4 }
    this[7] = signals.first { it.size == 3 }
    this[8] = signals.first { it.size == 7 }
}

fun MutableList<Set<Char>>.deduce2_3_5(signals: List<Set<Char>>) {
    this[2] = deduce2(signals, this)
    this[3] = deduce3(signals, this)
    this[5] = deduce5(signals, this) // must be called last!
}

fun MutableList<Set<Char>>.deduce6_9_0(signals: List<Set<Char>>) {
    this[6] = deduce6(signals, this)
    this[9] = deduce9(signals, this)
    this[0] = deduce0(signals, this) // must be called last!
}

fun decoderFor(signals: List<Set<Char>>): List<Set<Char>> =
    buildList<Set<Char>>() {
        addAll(List(10) { emptySet() })
        set1_4_7_8(signals)  // must be called first!
        deduce2_3_5(signals)
        deduce6_9_0(signals)
    }
```

## References

[SE Radio](https://www.se-radio.net/author/dalestrok/) (September 26, 2018), [_Episode 167: The History of JUnit and the
Future of Testing with Kent
Beck_](https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/).

Cunningham, W., (February 14, 2009), [_The Debt Metaphor_](https://www.youtube.com/watch?v=pqeJFYwnkjE), YouTube video.