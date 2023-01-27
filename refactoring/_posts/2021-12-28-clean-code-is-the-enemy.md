---
layout: post
title: Clean Code is the Enemy of Better Code
excerpt-separator: <!---more--->
---
Don't get carried away with the idea of clean code, sometimes doesn't suck as much is good enough.

<!---more--->

_Perfect is the enemy of good_ —Voltaire

_Good is the enemy of slightly less terrible_ —Arlo Belshee

_For anything to change, someone has to start acting differently._ —Chip Heath and Dan Heath in _Switch_

In his "[Master Legacy Code Incrementally](https://youtu.be/9YxQzjtPdtw)" presentation, Arlo Belshee gives some
extremely valuable advice about refactoring and unit testing. I have discovered many of the same things in my own
practice and what Arlo says is particularly validating for me because my outlook on refactoring has evolved along the
same lines that he describes.

This is the desert island takeaway of Arlo's talk:

> _Good fails with technical debt. We just can't afford it; we can't get there in one step. Menders set their sights lower, and by setting their sights lower, we can repair damaged codebases in place while adding features. We can perform that miracle._

## The future is now and it ain't too bright

Since it was published on YouTube just a little over a year ago, the video of Arlo's presentation has garnered a piddly
333 views (as of this writing), at least three of which are mine. That is quite pathetic and disappointing if you ask
me. But far from being a mark against Arlo and the validity of his message, those low numbers are a woeful indictment of
our industry.

In stark contrast, a search for "react tutorials" turns up many videos that have significantly more views—in the
hundreds of thousands and millions—many of them published more recently. A search for videos on refactoring reflects the
same disturbing trend: far more developers are interested in new and popular technologies than there are developers who
want to learn about techniques like refactoring and unit testing. The former, people who create things, are the "makers"
that Arlo mentions. The latter, people who, unlike the makers, find unraveling knots in gnarly balls of yarn soothing,
are the "menders."

As this trend of makers outnumbering menders continues—and it will probably grow—I fear new generations of developers
are doomed to keep making the same mistakes past generations have made. I also think the related problems we'll all have
to deal with are going to keep getting more complex and more impactful. The future, I'm afraid, doesn't look bright at
all.

## A Ray of Hope

There might still be hope.

If you're reading this, you're likely to be part of the solution, not the problem. You're probably a mender yourself, or
someone who guides menders, or aspires to turn more makers into menders. I won't claim to have the answers; that would
be too presumptuous. These are just ideas, but I think they're good ideas to try. I hope you do, and I hope you find
them useful.

## Messy code is inevitable; living with it is unacceptable

Bob Martin used to have a bit where he asks his audience "Who has ever had to work with messy code?" to which,
invariably, many hands are raised. Bob then asks "Then why did you write it?" Most people laugh at that, probably
because it's true: we are often our own worst enemy.

I think who made the mess matters less than what we do about it. A better question to ask, therefore, is "Why did you
leave it like that?"

Writing messy code is inevitable; it's a natural consequence of the nature of knowledge work and the inescapable fact
that we know far less early on than we do later. Sadly, most developers will just move on to the next task as soon as
they get their code to work, leaving messy code behind without so much as an afterthought or eyelid batted.

Dealing with the same messy code over and over again is not something we have to accept as a developer's lot in life.
It's unacceptable. We should do better. We _can_ do better.

## Why don't developers refactor more often?

What prevents developers from refactoring their code more often, as a matter of course? The answers to that question
seem to be consistently along these lines:

1. Not enough time / too much pressure to deliver
2. Too risky / fear of breaking things or introducing bugs
3. Lack of training/skills in refactoring, good engineering
4. Lack of motivation/incentive to do it

There are many more reasons you could cite but these four are probably the most fundamental. Before we dissect each of
these, let's take a closer look at a common underpinning: the need for Clean Code.

## Clean Code sets a high bar; maybe too high for most.

Robert Martin's book, _Clean Code_, is arguably the touchstone for writing good quality and relatively problem-free
code. If only everyone followed all of its advice, our code would be in much better shape and our work would be much
easier. Yet, here we are, twelve years since it was published, and most of us are still struggling with buggy and messy
code as much as ever, if not more.

Here's the sad fact we need to face as an industry: clean code is just too high of a bar. It's a great ideal to aspire
to but if it's the standard we have to meet, we're setting ourselves up for disappointment and, ultimately, failure.

As Arlo said, we need to set our sights lower. We need to aim lower than Good, for code that's Slightly Less Terrible
and Less Shitty.

## Ain't nobody got time for that!

"Later, if and when we have more time" is probably the biggest lie we habitually tell ourselves about refactoring. We
know there's little to no chance we'll be able to get to it later. That time hardly ever comes. Well, I probably
shouldn't say hardly ever because it does eventually come, just not under the circumstances we hoped it would nor with
the desired outcomes.

Time for refactoring is often forced on us under the least ideal conditions, when things are desperate, when both the
stakes and risks are high, and the survival of the product and the company that pays us to make it is on the line. We've
all seen that movie: Never enough time to refactor it now, always time enough to fix it in production later.

Deep down we know, probably more than anyone else, that Sweet Brown was wrong: there's always time for that, just not in
the way you hoped and usually by then, it's too late.

## High risk, low reward

Another bit that Bob does is where he says "Have you ever looked at some code and said 'Ugh, that's a mess! We should
fix that!' and then in the next breath say, 'I'm not touching it!'"

"Why?" Bob continues, "because if you do, and you break it, it'll be yours!"

Again, chuckles all around, because it's true. We live in fear of our code, afraid to make even the slightest change
lest we break something and get saddled with the burden of the consequences. Rather than being masters of the code,
we're slaves to it and its whims and idiosyncrasies.

We acquiesce to the code and its poor design. We learn how to live with its many warts and smells, no matter how awful
they are. "It's not that bad, we can live with it," we tell ourselves.

We figure out how to navigate all the tricky parts and avoid pitfalls and tripwires. We become experts in its
topography, and become practiced at jumping through all the various hoops its gnarly design puts in front of us.

We put up guardrails and gates to avoid nasty surprises and prevent well-meaning but naive newcomers from inadvertently
messing things up. Sure, making changes is much harder, and total cost of ownership goes through the roof. But at least
uncertainty and risk are kept to a minimum and we're still moving along, albeit at a much slower pace. That's a good
thing, right?

Arlo said something very interesting in his presentation:

> Products make lots and lots of money or they make almost no money at all. And if you're making lots of money, you can afford a slight increase in costs. If you're making no money at all, you can't afford the costs anyway, so who cares? The result is that cost doesn't matter very much, but bugs, uncertainty, and risk, those kill products.

This is the cage we build for ourselves, the road we pave with all good intentions, gladly paying the toll to avoid
uncertainty and risk. The costs, as Arlo said, are affordable. Moving at a snail's pace is fine. Until it isn't.

## The Atomic Option

We're also too greedy. We're impatient. Our eyes are too big for our stomachs. If not that, we're too exacting for our
own good. We want our efforts to mean something. We don't want to waste time on things that have no immediate or
significant impact. Again with the good intentions and the road they pave.

Many think that refactoring has to be a big effort, that it's too important to be as simple as renaming something or
extracting a few lines of code to a private method. To many, those little gestures seem trivial, borderline wasteful
even. They look like gold plating, something to satisfy a pedantic itch.

"It's bookish," is a comment that often gets my goat. No, it's not. It's as practical as it can be.

Read James Clear's _Atomic Habits_. That's what refactoring is: an atomic habit. Taken individually, any small
improvement you make to the code seems inconsequential. Taken as a whole and compounded over time, 1% better every day
turns out to be almost 38% better over a year.

## Our school of hard knocks

I've experienced this on a legacy product my team and I once inherited and maintained for almost eleven years. When we
got it, there were no unit tests. The code was horrible. No, it was horrific. We were scared to touch the code because
any small change we made had a disproportionately large blast radius. Maintenance was a slog and debugging was a
nightmare. Deployments were a big deal and lasted hours, often going into the wee hours of the morning. Many Friday
evenings were given up and weekend plans postponed or cancelled.

As frustrated developers are wont to do, we bitched and moaned, and cursed our predecessors. We wished them interesting
lives and multiple visits by Karma. If only we knew where they lived...

Finally, we had to face the fact that as cathartic as complaining felt in the moment, it really did nothing to ease our
pain or improve our situation. So we started taking back control. We committed to a protracted Boy Scout Rule campaign
and with a few small steps, steeled determination, and a fully supportive manager behind us, we embarked on our journey.

It took us two years, but after many group programming sessions over WebEx—mobbing wasn't a thing yet, much less virtual
mobbing—we eventually gained a reasonable measure of control over the code. Once again, we, the developers were the
masters. Well, not of all the code, but at least the parts we had to touch most often. Those parts were brought to heel.
We even got some dependency injection and automated system and smoke tests going.

Best of all, we were having fun as a team again. We were continuously learning to get better. Our sole tester was
writing smoke tests in Fitnesse which he ran on deployment night with a mere click of a button. Five minutes later, we'd
be done. Our deployments went from high ceremony to almost perfunctory. Pager duty was a free hundred bucks extra in
your paycheck every few weeks.

Read on to the end and I'll tell you what we did that just might work for you, too.

## If more training is the key, we're doomed

I hear many teams complain that they're not given the requisite training on refactoring, TDD, pairing, unit testing,
yada, yada, yada. "We don't know how to do it! We tried it and it's too hard. It doesn't work for us."

Is the key to better code really just a matter of know-how? Do we just need to train more developers on clean code, good
engineering practices, and their practical applications? If that's the biggest obstacle to making improvements, I'm
afraid we're fighting a losing battle. As far as I can tell, there just aren't enough competent coaches and trainers out
there to get to everyone. Or to teach everything that teams feel would make a difference.

Schools and academia aren't much help either. Sure, there are a few educators out there who keep up with industry
practices but the majority of those teaching the next generation of software professionals are still teaching things
that students will have to unlearn when they start working in the real world. But that's a whole 'nother story.

## They're just not that into it

What's really troubling is the sneaky suspicion that there aren't many developers who will invest the time and effort
needed to learn all there is to know about refactoring, testing, and all these other good engineering practices.

In a somewhat testy exchange on LinkedIn about technical debt and refactoring, an ex-colleague wrote:

> "I'm saying ditch the magic theology talk, stop citing the thinkers, most developers want to support their families and go home at 4pm - not be craftspeople or whatever the word is now. If your statement requires reading and agreement of an entire book; rethink it."

"The thinkers" he referred to were most likely Ron Jeffries and Ward Cunningham, whom I have a propensity to quote. But
as disappointing as That Guy's statement may be to anyone who holds themselves to a higher standard, it's hard to
refute. It also resonates with what my wife, who is a Scrum Master at a large financial institution, often tells me: "
There aren't many developers who will spend as much time and effort to learn and practice all the things that you do."
She does have a point, and as much as I hate to admit it, so does my ex-colleague.

## Intrinsic or extrinsic motivation?

For me, it's not a question of either/or; teams need both intrinsic and extrinsic motivators to start improving their
situations.

These days, more managers seem to understand the need to give their teams the space and the air cover to fail and learn
from failure. They understand the need to create and nurture environments for their teams that support rather than
suppress desired behaviors and practices. They know the importance of providing proper coaching, mentoring, on-the-job
training, and continuous learning opportunities. They are willing to fight for budgets that include good tooling and
enabling infrastructure.

But developers need to do their part and step up to the plate, too. They need to let go of old habits and be willing to
venture out of their comfort zones, be willing to fail, and be comfortable with learning from failure.

They need to set aside their egos. Nothing impedes learning and improvement more than thinking that there's nothing
anyone can teach you about how to get better at doing your work, that maybe the things you've learned and come to depend
upon for success are outdated, outmoded, and in dire need of replacement.

## Our keys to Slightly Less Terrible on the road to Better Code

So, how did my team and I survive eleven years of maintaining a legacy app that didn't have automated tests, was very
poorly written and designed, and still manage to make keep our product viable and our sanity more or less intact? (The
hair situation was a total loss for me though. Ah well, you win some, you lose some.)

First, you need good people on your team, people who are willing and eager to learn, who can set aside their egos and
accept that no matter how many years they've been in this business, they still have plenty of room left to grow. For the
most part, I lucked out in this department. I had a great team of learners and doers.

If you need to lose or replace people, have the courage and intestinal fortitude to make the choice. It's better to lose
someone who isn't a fit than try to make it work against all odds. There's no time for that and it's not fair to anyone.
If you do boot someone off the island, do it with kindness and empathy.

Second, you need management to play their part. I've already gone over what's involved there.

Third, learn to be okay with ugly. Things are going to get worse before they get better. Don't aim for Good, much less
Perfect. Be okay with Slightly Less Terrible but don't be okay with living with Shitty Code. 

And by "shitty" I mean code that's unclear, complex, confusing, hard to understand, slows you down, or makes you scrunch your face up
or scratch your head and wonder "WTF?!" 

That's it. 

Anything else is just ugly and can wait until after you [make the code CLEAR](./clear-code.md)er.

Fourth, be a team. Mob, or as they prefer to say these days, ensemble or whatever the verb form of that is. The best way
to learn something is to try to teach it. Teach each other. Retrospect after every collaboration session, not just at
the end of each iteration. Share your experience with other teams. Learn from others, too.

Tell the story of the code. Keep telling it to each other until all your stories are consistent and it
makes sense to everyone. 

Use tests as your story board. Experiment. Don't agonize over what the best option is to take. Try them all, write them
down. See each option in code and call it for what it is. Practice radical candor and remember to be kind. Make any
criticism about the code, not the person writing it.

Be willing to spend five minutes choosing a good name for something. Again, don't agonize over which of the different
choices is best. Try each choice out in the code and let each one stand or fall on its own merit. It'll pay off, I
promise.

Follow [Kent Beck's Four Rules of Simple Design](https://martinfowler.com/bliki/BeckDesignRules.html). These are really good guidelines; all kinds of goodness and important principles spring from them.

Learn your tools: Rename, Extract, and Compose. These are the main ones we used. Arlo has
a [few more good ones](https://arlobelshee.com/the-core-6-refactorings/). Honestly, I think Rename and Extract are
enough for any team to hit the Slightly Less Terrible target. Compose is just a bunch of Renames and Extracts but use it
anyway. It's a force multiplier for Rename and Extract.

Speaking of Compose, don't hesitate to SLAP your code
around. [Single Level of Abstraction Principle](http://principles-wiki.net/principles:single_level_of_abstraction), that
is. Slowing down to understand code is much more costly in the long run than the overhead of a few extra frames on the
call stack. Remember, premature optimization is also a good intention and we all know where that road leads.

Commit to writing [CLEAR code](./clear-code.md). Telling the story of the code helps with that. Use your eyes, ears, and
sense of time as well. You can't literally smell code problems, but you can certainly use your other senses to spot
them. My CLEAR Code article has more on this.

Arlo said that the most common cause of bugs is illegible code. I absolutely agree with that. So does Ward. Understand
that technical debt, at its core, is really about a lack of understanding. Have everybody watch the video and
understand [what Ward meant when he coined the Debt Metaphor](https://youtu.be/pqeJFYwnkjE).

And lastly, listen to the thinkers, no matter how geeky or fanboi-ey that may seem or what Some Guys will say. 

Ward Cunningham, Ron Jeffries, Martin Fowler, Kent Beck, Bob Martin, Alistair Cockburn, James Grenning, Andy Hunt, Bill
Wake, Tim Ottinger, Joshua Kerievski, Michael Feathers, Nat Pryce and Steve Freeman, Corey Haines, J.B. Rainsberger,
Neal Ford, Venkat Subramaniam, The Gang of Four, Eric Evans, they were all our teachers. So were a bunch of other folks they know, too many of them to
name here right now. Look around and you'll know who they are.

That's about it. 

No wait, there's one last thing, and I'm embarrassed I didn't close with this in the version I first posted. And I'm not kissing up to him because he happened to be one of the first people to read this and endorse it. 

I'm dead serious when I say it's an injustice that there aren't a million more views of Arlo's video about menders and setting the bar lower. If you haven't seen it yet, [go and watch Arlo's video now](https://youtu.be/9YxQzjtPdtw). You owe it to yourself and your fellow developers.

And if you've managed to read to this point, know that you probably have what it takes to take back control of your code like we did. It takes patience, and perseverance. It takes a willingness to wait for all those 1%-or-less improvements to get to critical mass and really start paying off. Stick with it. It will come. Maybe sooner than you think.  

Ok, now I'm done. Anything else I haven't mentioned you'll probably discover along the way anyway.

## Promises to keep, and many miles to go...

Like I said before, I won't presume to claim that what worked for us will also work for you. You have your own context
and that's what makes you different. But we're not all that different, I don't think. If what you've read above
resonates with you, then we probably have a lot more in common than not.

So buckle up and knuckle up. It's going to be a bumpy ride. That's a given. But if you keep at it and don't lose
faith, you'll get there, too. And maybe, just maybe, after many miles of going, you'll finally get a chance to catch up
on your sleep.

Good luck!

## References

Belshee, A. (May 2016). _The Core 6 Refactorings_. Blog post. [https://arlobelshee.com/the-core-6-refactorings/](https://arlobelshee.com/the-core-6-refactorings/)

Belshee, A. (December 2020). _Master Legacy Code Incrementally_. YouTube video. [https://youtu.be/9YxQzjtPdtw](https://youtu.be/9YxQzjtPdtw)

Clear, J. (2018). _Atomic Habits_. Penguin Random House, LLC.

Cunningham, W. (Feb 2009). _Debt Metaphor_. YouTube video. [https://www.youtube.com/watch?v=pqeJFYwnkjE](https://www.youtube.com/watch?v=pqeJFYwnkjE)

Fowler, M. (March 2015). _BeckDesignRules_. Blog post. [https://martinfowler.com/bliki/BeckDesignRules.html](https://martinfowler.com/bliki/BeckDesignRules.html)

(Oct 2021). _Single Level of Abstraction Principle_ (Oct 2021), Web article. [http://principles-wiki.net/principles:single_level_of_abstraction](http://principles-wiki.net/principles:single_level_of_abstraction)

Lacar, J. (Dec 2021). _How to write CLEAR Code and get better at refactoring_. Mindful Practice Blog. [https://jlacar.github.io/refactoring/clear-code.html](https://jlacar.github.io/refactoring/clear-code.html)

{% include post-footer.md %}
