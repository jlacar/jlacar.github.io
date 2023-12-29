---
layout: post
title: Heuristics for Refactoring
author: Junilu Lacar
banner-img: /assets/images/refactoring/heuristics-monks-3d-isometric.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 370
banner-height: 370
---
The question of when to refactor seems to be a hot topic these days so I decided to throw my two cents into the hat.

I use a more heuristic approach to refactoring, guided by a few basic rules and my intuition. I realize this may not seem very helpful, but if you would humor me a bit and stick around as I explain my process, I think you just might get something useful out of it.

Let's start with a puzzle that's best solved heuristically.

### A non-programming example of using heuristics

The Monk's Journey is a puzzle that can be solved heuristically. It goes something like this:
>A monk goes on a pilgrimage to a monastery at the top of a mountain. There is only one narrow path up the mountain to the monastery. The path winds randomly with many dips and rises. At the crack of dawn on the first day, the monk starts walking up the mountain from its base. He walks at varying speeds and stops multiples times to rest and eat. He finally arrives at the monastery at sunset. He meditates all night and completes his pilgrimage.
>
>The next day, again at the crack of dawn, the monk starts back down the mountain. Again, his pace varies and he stops multiple times along the way to rest and eat. He still manages to arrive at the base of the mountain just as the sun sets.
> 
>Prove that there is a single point on the path that the monk passes at exactly the same time on both days.

## Heuristics

What exactly are heuristics anyway? Wikipedia gives us [this definition](https://en.wikipedia.org/wiki/Heuristic):
> In [psychology](https://en.wikipedia.org/wiki/Psychology), heuristics are simple, efficient rules, either learned or inculcated by evolutionary processes. These psychological heuristics have been proposed to explain how people make decisions, come to judgements, and solve problems. These rules typically come into play when people face complex problems or incomplete information.

Incomplete information is exactly what we seem to have with the Monk's Journey puzzle. It's challenging, if not impossible, to solve with math (I didn't even try). And yet, the solution is easy to understand, logical, and quite elegant in its simplicity using heuristics. See if you can come up with a simple proof before you skip to end to read the solution.

## Simplicity

To me, simplicity is the key to knowing when to start and stop refactoring. It's my number one heuristic. If code isn't simple enough for me to quickly understand it, I will try to refactor it immediately.

Kent Beck's Four Rules of Simple Design serve as my guide to simplicity. I started using them after reading Corey Haines' book, "[Understanding the Four Rules of Simple Design](https://leanpub.com/4rulesofsimpledesign)". 

It's a short book based on Corey's experience with facilitating hundreds of [Global Day of CodeRetreat](https://coderetreat.org) events at which developers come together for a whole day of deliberate practice. I'll tell you more about this event in another article. If you haven't already read Corey's book, I highly recommend you do, soon. I wish I had read it sooner because it added so much to my practice of TDD, design, and refactoring.

Kent Beck's four rules of simple design can be summarized as follows:

>The code
>1. Must pass all its tests
>2. Must clearly express its intent
>3. Contains no duplication
>4. Has the fewest / smallest elements

You may find these rules expressed a little differently elsewhere but this is how I interpret and use them, based mainly on Corey's interpretation of them in his book.

## First, there must be tests

Rule #1 is that all tests pass. To do that, there have to be tests in the first place and they have to be passing. Without these, refactoring is risky, unless you're only doing the simplest and safest refactorings of them all, renaming things. 

Renaming is usually safe as long as you're using an IDE that can automatically find and replace all references to the thing you're renaming. If you're renaming manually, make sure you know the scope of the name. The bigger the scope, the riskier it is to rename manually. This is why I like to interpret Rule #4 as "small" rather than "fewest". A local variable with limited scope is much safer and easier to rename manually that a global variable.

Tests provide a safety net for refactoring. If the change you make breaks something, one or more tests should fail to warn you about it. You can then revert the change and try something else. It's all about getting fast feedback about anything you do.

## Second, gain clarity and understanding.

If you're sitting there looking at the code for more than a minute trying to understand what it's doing, it's time to refactor. I have it down to understanding the code in less than thirty seconds. This means that each section of code I need to understand has to be small enough to be consumable in half a minute. Again, I lean on Rule #4, code that has small elements, small chunks, small methods.

Context also matters, so the more helpful and contextual information the surrounding code provides, the better. It's also best if I don't have to scroll up or down away from the main chunk of code I'm looking at in order to understand the context it's working in. Flipping from one file to another is the worst in terms of creating context. If I have to do this a lot, I tend to want to start refactoring.

That's Rule #4, smallest and fewest elements. By now, you might be wondering why this rule isn't ranked higher in order of importance. It's because it's dependent on writing code that adheres to Rules #2 and #3. Code that expresses itself clearly tends to be small. There's only so many concepts I can hold in head at any given moment. Prioritizing for clarity usually leads to smaller code. Likewise with eliminating duplication.

## Clarity and expressiveness improve understanding, pay down debt

Code that expresses its intent clearly leads to better understanding of what's going on in the program. Clear, intention-revealing code makes it easier to reason about its behavior. 

Names, in particular, play an important role in making the code expressive and focused on intent. I watch for names that are more about implementation, like data structures or specific algorithms used. Details like this should be buried in the implementation levels of code. In high-level functions and methods, the code should express its _intent_. This leads to code that's resilient to changes in implementation.

Ward Cunningham posted [a video on YouTube](https://youtu.be/pqeJFYwnkjE?si=HSlw635tQ4C0gquS) way back in 2009 to explain how he came up with the Debt Metaphor, the idea that gave rise to the term "technical debt." If you listen carefully to what Ward says in that video, you'll realize that "debt" is essentially "a lack of understanding reflected in the code." That is, code that has technical debt is code that lacks clarity of intent.

The more clearly the code expresses its intent, the better and easier it is for anyone reading the code to gain a clear and correct understanding of its behavior. Whenever I find myself staring at the code struggling to understand what it's doing, I take it as a sign that it needs to be refactored for clarity.

This is what "paying back the debt" is about. I want to take any understanding I have in my  head and put it back in the code. I think of this as "paying it forward" to my colleagues who will have to read and maintain the code in the future. And to my your future self, when I have to come back and work on the code again.

## Coherence and Cohesion

Another thing that triggers refactoring for me is code that is lacking cohesion and coherence.

Coherent code makes sense when I read it. Coherent code tells me a story. Rule #2 is key to making code make sense and tell a good story.

Cohesive code makes it easy to for me to understand the context in which the code operates. It makes it easy to see dependencies and things that are related to the behavior I'm  interested in.

Cohesive code doesn't make me flip from one file to another, trying to find the dots that connect to each other. If I have to open up two or more files in order to understand a certain bit of behavior, then I start looking for ways to gather all that information in one easily-accessible place. I'll usually find things that have been duplicated, a violation of Rule #3. I also usually find things that are too big, a violation of Rule #4. 

Whatever the case may be, I know I can stop refactoring when it's not so much of a pain anymore to connect the dots and have a coherent and cohesive idea in my head that is reflected in what I'm reading in the code. 

## Good things lead to other good things

All of the above are the basic things I look for when I'm reading and working with code. Everything else I do seems to stem from these basic things. Without clarity, coherence, and cohesion, it's hard for me to gain an understanding of the code's behavior. Without a good test suite to provide a safety net, it's hard for me to start refactoring and repaying the debt of understanding owed to the code. The tests give me confidence to refactor ruthlessly, and the confidence to proceed courageously.

I know these aren't concrete, step-by-step instructions on when to start and stop refactor. That's why I consider them heuristics, fuzzy rules that I have learned through experience and have internalized with practice. I guess it's what we developers like to call "intuition" which is really nothing more than experience whispering in our ear reminding us of those other times we've encountered similar situations.

## Heuristic Solution to The Monk's Journey

The Monk's Journey puzzle would be difficult to prove mathematically given the absence of critical information about exact start and end times, speed of walking, and the number and duration of stops the monk made on each day. It's all fuzzy information, which is where heuristics come into play.

Imagine _two_ monks instead of one: one monk going up the mountain, the other one going down. Both monks start their respective journeys up or down the mountain on the same day, at about the same time (dawn), and finish at about the same time (sunset). The exact start and end times don't really matter. What matters is that there's overlap between the two treks.

Now, imagine yourself as a third monk watching the other two monks as they walk on the path. Imagine also that you can see both monks at all times. At some point in the day, there will be a point on the path where the two monks meet. _That_ place and time is the answer to the puzzle.

Remember, you were only asked to prove that there _is_ such a place and time, not where and when it is exactly.

Simple, right? Logical, easy to understand, and elegant, too, in my opinion.

## Conclusion

Thanks for staying with me to the end. I hope you got something useful from this.

Until next time, happy coding!

## About the banner image

The banner image for this article was generated by AI on [bing.com](https://www.bing.com/images/create?FORM=GENILP) using the prompt "a hill, two monks, one monk going up, the other going down, 3-D game isometric". I didn't even mention the third monk and yet, there he is, watching the other two. Pretty amazing, right?