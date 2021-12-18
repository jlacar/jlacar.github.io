# Make your code CLEAR and Conversational

I first heard the idea of code telling a story from Kent Beck
in [SE Radio Episode #167](https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/)
. Kent was explaining how a test should have an arc of a story, with a clear beginning, middle, and end. Ever since
then, I have incorporated that idea in my conversations about design and code. I often say things like "Make the code
tell a story" and "What story is the code telling us?" and "Listen to the code's story. Is it clear to you what's going
on?"

## Technical Debt Detector

I've paired this concept of code telling a story with the idea of a User Story. I try to make my code's story match the
User Story as closely as possible. When I start programming for a new User Story, I'll say "Ok, let's tell this story in
the code."

I've tied this kind of perspective to technical debt, with the meaning of that term taken from Ward Cunningham's
explanation of The Debt Metaphor, where debt is essentially, the gap between what the code says and what it should say.
Or as Tim Ottinger put it recently, the difference between what know now and what we knew when we wrote the code.

I have found that this difference or gap of understanding in the code can be detected more easily by reading the code
out loud. Essentially, telling the story of the code is a good device by which you can find gaps in the understanding
that the code embodies. The more gaps and misunderstandings in the code, the more technical debt you have.

## How does that work?

For me, reading the code out loud, which I'll refer to hereon as "telling the code's story," is an additional channel of
communication by which I can reconcile what the code says and I what I think it should say.

If I get a different idea from someone telling the code's story as I'm trying to read along with it, I'll raise my
hand (so to speak) and say "Wait, that's not what the story is. From what I'm reading it's..." and then go on and tell
the code's story as I understand it.

In a pairing or ensembling situation, this is a great way to get alignment on what the shared understanding is. If there
are any misalignments, they are discussed, a consensus reached on how it should be expressed, and then that consensus
immediately reflected in the code, usually by Renaming and moving things around so the code tells its story the way we
agreed it should.

## More Examples of Conversationally CLEAR Code

```kotlin
    val knownSignalLengths = mapOf(1 to 2, 4 to 4, 7 to 3, 8 to 7)

    fun MutableList<Set<Char>>.setKnownSignalPatterns(signals: List<Set<Char>>) {
        knownSignalLengths.forEach { (digit, length) ->
            this[digit] = signals.first { it.size == length } }
    }
    
    fun MutableList<Set<Char>>.deduceSegments(signals: List<Set<Char>>, selectors: Map<Int, (Set<Char>) -> Boolean>) =
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
        .first { it.size == 5 && it !in decoder.slice(setOf(2,3)) }
    
    /* signals with size == 6 : (6, 9, 0) */
    
    fun deduce6(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
        .first { it.size == 6 && (it subtract decoder[7]).size == 4 }
    
    fun deduce9(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
        .first { it.size == 6 && (it - (decoder[4] union decoder[7])).size == 1 }
    
    fun deduce0(signals: List<Set<Char>>, decoder: List<Set<Char>>) = signals
        .first { it.size == 6 && it !in decoder.slice(setOf(6,9)) }

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