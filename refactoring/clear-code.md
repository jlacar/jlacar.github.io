# How to Write CLEAR Code and Get Better at Refactoring

![Banner image of man reading James Clear Book Atomic Habits](/docs/assets/nubelson-fernandes-XWn6m_oxvSM-unsplash.jpeg)

In Kent Beck's four rules of simple design, Rule #2 "Reveals intention" could also be written as "Clearly expresses
intent" or simply, "Clear."

Rule #3, "Contains no duplication," has its companion acronym, DRY (Don't Repeat Yourself) which lends itself well to
cool refactoring-speak like "Let's DRY that up," which is to say "Let's eliminate that duplication."

That's pretty cool, but where is the love for Rule #2? I haven't been able to find a similar companion acronym for it.
Until now, that is. But before we get to that, let's take a closer look at this idea of "smells" in the code.

## Getting a Cue from the Code

The origin story of the term "code smell" is summed up in Chapter 3 of Martin Fowler's book _Refactoring: Improving the
Design of Existing Code_.

> If it stinks, change it. — Grandma Beck, discussing child-rearing philosophy

It's kind of ironic that we use the sense of smell to describe things that cue us in to possible problems in the code.
Because unless you have a 4D theme park ride computer that can give off malodorous puffs of air when poorly factored
code is displayed, you can't literally _smell_ code. It's just a figurative term.

There are, however, other senses that can quite literally be associated with possible problems in our code. I categorize
them as Visual, Temporal, and Audible refactoring cues. The names aren't quite as catchy or as visceral as "code smell"
but if you learn to recognize the problems associated with them, finding opportunities to refactor becomes much more
intuitive and I find it's easier to learn and remember specific catalogued code smells.

I came up with these categories because most people I've coached have a hard time reading up on the twenty-two code
smells (or twenty-four in the second edition) catalogued in Martin Fowler's refactoring book, let alone understanding
and knowing what to do about them. The task of learning, recognizing, and then dealing with all of them is just too
daunting for your average developer. In the eternal words of Sweet Brown, "Ain't nobody got time for that!"

In fact, even I haven't had time to master all of them or have them imprinted deeply enough in my memory to be able to
easily pick a specific one out of a lineup of usual suspects. On a good day, I can rattle off a dozen at best, maybe.
And to be honest, I couldn't tell you off the top of my head what a Refused Bequest looks like or even what it is. I'd
have to refer back to Chapter 3 to refresh my memory for that.

## Visual Cues

Some problems can be detected right off the bat without even reading the code. Just look at the shape of the code and
how it's organized.

![Image of Laptop with code displayed](/docs/assets/oskar-yildiz-cOkpTiJMGzA-unsplash.jpeg)

Even the color of the code in your IDE can tell you that something might be a candidate for refactoring. Modern IDEs
like IntelliJ IDEA and other similar products are a great help in this. They will subtly cue you in to a refactoring
opportunity to extract or simplify code that it has marked with a squiggly underline or flagged with a yellow light bulb
in the margin. These are usually safe refactorings, too, because the IDE can do all the heavy lifting for you.

IntelliJ IDEA will flag repeating patterns of Duplicate Code that could be extracted, even patterns that are repeated in
separate files. Otherwise, a quick scan of the code can easily help you zone in on Long Methods, Long Parameter Lists,
Large Classes, Primitive Obsessions, Switch Statements, Lazy Classes, Temporary Fields, Message Chains, Data Classes,
and last but not least, Comments (if you didn't already know, yes, comments can indicate a nearby code smell, usually
the thing being commented on).

This category of cues alone already covers half the code smells catalogued in the first edition of Fowler's book.

There are other things that can visually indicate problems. Developers use whitespace to create "paragraphs" in their
code to separate different responsibilities or groups of statements related to the same task. Groupings like that could
be Misassigned Responsibility. Multiple dereferences of the same variable might indicate Feature Envy.

One smell in particular that easily qualifies as a Visual cue
is [Arrow Code](https://blog.codinghorror.com/flattening-arrow-code/). Surprisingly, many developers don't know about
this one. I always warn people that once they see it, they'll never be able to unsee it. Then it's goodbye blissful
ignorance, hello harsh realization that this has been right in front of them all the time.

All these refactoring candidates can usually be addressed with some kind of Extraction. [Compose Method](https://www.informit.com/articles/article.aspx?p=1398607) refactoring can
also come into play when dealing with visual cues, especially with Long Methods.

## Temporal Cues

Temporal cues have to do with the passing of time. The more time it takes to do something, the more likely it is you are
dealing with code that needs to be refactored. After all, refactoring is about making the code easier to work with and
understand. If it takes you a long time to understand code, it more than likely means you have accumulated debt and are
already paying interest.

How long does it take you to understand a small section of code? If it takes you more than a few seconds to understand
what a variable represents or what its purpose is, maybe it needs to be Renamed. How long does it take to connect the
dots? Find dependencies? Find where something is set up, initialized, or injected? If it takes too long to understand
the context under which some code will be executed, then maybe the code is lacking in Coherence or Cohesion.

Other temporal cues can help you with your development rhythm and flow, especially if you're doing TDD. How long has it
been since you were last Green? Since you last refactored? Since you last ran the tests? Since you last switched drivers
and/or navigators? Since you last took a break? These aren't necessarily problems in the code per se but they can still
have an impact on the quality of the code you're writing.

Here are a few threshold guidelines I use as temporal rules-of-thumb:

Three seconds to understand a method - any longer than that and maybe the method is too long or maybe you're spending
too much time trying to understand unclear code. If that's the case, (spoiler alert!) CLEAR it up. More on this later.

Thirty seconds to understand the context - any longer and maybe your design is not cohesive or coherent enough. You
should be able to gain situational awareness about the code fairly quickly. If not, then maybe things are too far apart
from each other or too spread out and that is getting in the way of you understanding the context in which the code
executes.

Three minutes between running the tests and refactoring. This keeps you in a tight feedback loop and keeps you moving
forward with clean code and a workable design.

Thirty minutes on a single problem - any more than that and you might be biting off more than you can chew. Maybe you
need to break the problem down into smaller chunks. Maybe your solution is just too complex and you're having a hard
time keeping track of many different concepts in your head. Maybe you haven't refactored in a while. Maybe it's time to
simplify and do some Extracting.

## Audible Cues

I have saved the best, and possibly the hardest to learn and leverage, for last: the Audible cues.

I often encourage developers to read the code out loud when pairing or ensembling—is that even a word? Because
apparently, m*bbing is anathema now. I understand, I get it. Anyway, reading the code out loud gives you another channel
by which to reconcile what the code is saying and what you intend for it to do. You have to develop a keen ear for this
to work though and sharpen your listening and cognition skills.

Listen for implementation details rather than intent. Listen for ideas that don't match the intent of the code. Listen
for incoherent articulation of intent. Listen for the wrong words being used. Listen for code that lies or otherwise
misleads.

All these are usually the consequence of poorly chosen names and are relatively easy to address: just Rename things so
they make sense. Of course, we all know that choosing good names is one of the two hardest things in programming,
besides cache invalidation and off-by-one errors (haha).

Sometimes reading the code out loud makes it clear that you're just saying too much and that maybe you could simplify
things. For example, read this code:

    Student student = StudentService.findStudent(studentId)

Now read it out loud.

Did you hear yourself stutter? Why does the code say "student" so many times? How about we refactor that to make it
clearer?

Now read this out loud:

    Student enrolled = StudentService.findBy(id)

Did you still stutter? Did you tell a more coherent story just now?

Making the code read just like it would when spoken out loud gives you a great opportunity to verify that the code
clearly expresses its intent. If you read the code out loud and someone listening to what you're saying can reconcile it
with what they're reading from the code, then most likely the code is clearly expressing its intent.

If, on the other hand, what you're saying doesn't match what other people are getting from reading the code, they should
call it out and say "You weren't making any sense just now. That's not what the code is saying." That is the sound of
technical debt which, by Ward Cunningham's definition, is a lack of understanding reflected in the code. The code is
creating confusion by not reading like it sounds.

### Listen for what you're not hearing

Possibly the most difficult part about audible cues is listening for what's not being said. Any relevant information
that is not explicitly written in the code will be either assumed or inferred by those listening to the code being read
out loud. But this only happens if they already know the information, because their brains will automatically fill in
the missing context.

Unfortunately, those who don't already have that part of the context in their heads will miss out on the tacit
information. This is a gap in the code that can cause confusion and misunderstanding. This is where bugs can insidiously
get into your code.

Information that is not readily available from reading the code is also technical debt because people who don't have it
will take longer to understand the code. It needs to get out of people's heads and be embodied in the code so that
anyone else reading the code later on can readily gain the understanding that goes with it.

## Writing CLEAR code

This brings me to the newly-coined (by me, as far as I know) companion acronym for Rule #2, Reveals Intent. And that is:
write CLEAR (Code Listens Exactly As it Reads) code.

That is to say, the story you get from listening to the code should match the story you get from reading it. In other
words, how it "listens" should match how it reads.

Yeah, I realize the grammar is a little peculiar and that "CLEAR code" ironically has duplication but I'm hoping you can
get over that minor grammatical idiosyncrasy because it makes for really cool refactoring-speak: "How about we CLEAR
this up?" aptly means "How about we make this code clearly express its intent?" Cool, right?

And if you use it, please try to remember my name: Junilu Lacar. (grin)

## Postblog

You may not have noticed this, but there are three refactoring techniques mentioned here that are usually involved with
the three categories of cues: Rename, Extract, and [Compose](https://www.informit.com/articles/article.aspx?p=1398607).

I have found that I use these three refactorings about eighty percent of the time (a [SWAG](https://en.wikipedia.org/wiki/Scientific_wild-ass_guess), of course). Most, if not all, IDEs have Rename and
Extract in their automatic refactoring toolbox. And Compose is usually achieved by a series of Extract and Renames, so
really, you only need to employ Rename and Extract most of the time to keep your code clean and CLEAR.

If you just use the three visual cues and these three refactorings, you'll usually get your code to a much better place
than it would be otherwise.

But remember, you first heard about CLEAR code right here, from me.

## Credits

Photo
by [Nubelson Fernandes](https://unsplash.com/@nublson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
on [Unsplash](https://unsplash.com/s/photos/books-reading-headphone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText):
Man reading and listening

Photo
by [Oskar Yildiz](https://unsplash.com/@oskaryil?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
on [Unsplash](https://unsplash.com/s/photos/computer-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText):
Laptop with code displayed

## References

Fowler, M. (2015) *BeckDesignRules*, Bliki article:
[https://martinfowler.com/bliki/BeckDesignRules.html](https://martinfowler.com/bliki/BeckDesignRules.html)

Fowler, M. (1999). _Refactoring: Improving the Design of Existing Code_, Addison-Wesley. 1st Edition.

Atwood, J. (Jan 2006) _Flattening Arrow Code_. Blog article: [https://blog.codinghorror.com/flattening-arrow-code/](https://blog.codinghorror.com/flattening-arrow-code/)

Also published on [LinkedIn](https://www.linkedin.com/pulse/how-write-clear-code-get-better-refactoring-junilu-lacar)
