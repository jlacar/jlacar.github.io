# All I Needed to Start Refactoring I Learned in Kindergarten

![Girl painting picture](/docs/assets/nicole-leeper-GfF9fYvmVTU-unsplash.jpeg)

In the discussion of a poll on refactoring that I recently posted on LinkedIn, Gregor Riegler offered this: "I think it also demonstrates that lacking know-how is a key bottleneck."

In [a previous post](/refactoring/clean-code-is-the-enemy.md), I wrote that if training and know-how is the main thing needed to get us out of the messes we find ourselves mired in, then we're all doomed.

In this post, I borrow heavily from Robert Fulghum's _AlI I Really Need to Know I Learned In Kindergarten_ to get to the heart of what I think developers need to start on the journey back to healthier codebases. 

If this sounds like more of what I've said before, I think it's an important message that bears repeating.

> _Everything you need to know is in there somewhere._ —Robert  Fulghum

## Basic Sanitation

Of the sixteen rules in Fulghum's Kindergarten Credo, a quarter of them are about basic sanitation:
  - Put things back where you found them
  - Clean up your own mess
  - Wash your hands before you eat
  - Flush.

The second one is probably would you'd immediately associate with refactoring but all of them are related in some way. As a companion to all four of them, I would add this one:
  - And then remember the Dick-and-Jane books and the first word you learned—the biggest word of all—LOOK.

### Clean up your own mess

Let's start with the obvious one. When it comes to writing first-time code, we developers are like kindergarteners with their first coloring book: we use the wrong colors, we color outside the lines, we don't fill everything up, and color in things that shouldn't be colored in. And then, when we're marginally satisfied with our work, we quickly turn the page and start on the next picture.

I'm sure if you had a chance to flip through one of your old coloring books from kindergarten, you'd chuckle at how crude and unrefined your motor skills and sense of art were back then.

But If you had a chance to travel back in time and help your young self, would you teach yourself the nuances of shadow and light, of the significance of space, negative space, perspective, and depth? I doubt it.

If anything, you'd probably start with what a typical kindergartner could handle: stick with the basic tools and try not to color outside the lines.

### The basic refactoring tools

The basic tools for refactoring are Rename and Extract. Arlo Belshee has [a few other ones you can use](https://arlobelshee.com/the-core-6-refactorings/) but these two are the essential ones, in my opinion. Think of them as your basic pencil and eraser.

![Pencil and eraser](/docs/assets/kim-gorga-2Zl80uqruUU-unsplash.jpeg)

Rename and Extract are arguably the most commonly-used items in your IDE's refactoring menu. And they're usually safe to perform, even without tests that will fail if you somehow mess up.

Do you remember getting buddied up with someone when you go on a field trip or even just crossing the street? Well, your IDE is your best buddy for crossing Refactoring Street on the way to Better Code Town.

![Kid holding hand](/docs/assets/guillaume-de-germain-fgmLRBlUIpc-unsplash.jpeg)

Modern IDEs can warn you of any potential problems in renaming or extracting code. Trust your IDE to do its job and watch out for any danger. A good IDE will tell you when it's safe to go ahead and it will even do all the heavy lifting for you!

### Stay inside the lines

To stay inside the lines, use Compose. If you're not clear on how this helps you do that, read up on SLAP ([Single Level of Abstraction Principle](https://www.google.com/search?q=single+level+of+abstraction+principle)). Jumping from one level of abstraction to another inside the same method is like coloring outside the lines. It's messy and it's distracting.

![squiggly lines](/docs/assets/andrew-donovan-valdivia-DSnAH3SgGbI-unsplash.jpeg)

You don't need to pay for an expensive coach or go to a training class to grok this. Just [look it up](https://www.google.com/search?q=compose+method+refactoring).

Compose mainly uses Rename and Extract. Learning how to Compose will take all of half an hour to read up on and maybe a couple of hours to practice and get a hang of. 

Start using Compose and use it to paint a clearer pictures in your code that other people can easily understand.

![Inside the lines](/docs/assets/sandeep-singh-Qito7fJMOEo-unsplash.jpeg)

### Wash your hands before you eat

When it comes to code, this one is more about not putting a clean dish on a dirty table. If the table is dirty, clean it off first before you put clean dishes on it.

This is how Robert Fulghum might put it:
> A prudent man avoids a mess like this.

This means that you shouldn't put good code on top of bad code. Refactor the area where the new code will be set, so it doesn't get contaminated by existing poor or ill-suited designs.

This is how Kent Beck says it:

![Kent Beck tweet](/docs/assets/kent-beck-easy.png)

A corollary to this is related to the good old 3-second rule. Remember that one? The one that says if a thing hasn't been on the ground for more than three seconds then it's still good to pick up and eat? Seems like a lot of us developers have never outgrown the 3-second rule. But seriously, don't follow that rule, because it's gross. Grow up already.

All this means is that you shouldn't just take something that you picked up from God only knows where and just plop it into your codebase. 

Make sure you understand any piece of code you didn't write before you copy-paste it in. Don't just blindly copy things off of stackoverflow or any other place without first properly vetting for fit for purpose and functionality. And make sure to write some tests around it.

### Put things back where you found them

![Nice and tidy toys](/docs/assets/pro-church-media-2DTE3ePfnD8-unsplash.jpeg)

For me, this is really about keeping things nice and tidy. If you have to play around and make a bit of a mess here and there, that's fine. Play around, make a mess. Experiment. Learn. Have fun. But when you're done playing and experimenting and learning, make sure to put all the puzzle pieces back in the box, the toys back on the shelf, and all the blocks back in the box. Give the other kids a chance to play, experiment, and learn, too.

This is basically the Boy Scout Rule: never leave the campsite any worse than you found it. And it's best if you can leave it a little better, so the next person can benefit from it.

### Flush

Basically, just get rid of crap when you see it.

Don't be afraid to double or triple flush either. Heck, flush every two or three minutes if you need to. That's what the TDD cycle should feel like. Think of the mantra as Red-Green-Flush instead. And don't worry, frequent flushing actually helps the environment when it comes to code.

This applies mostly to old code and comments. But it also applies to duplication, complex logic, temporary variables (in certain contexts), and other stuff that generally costs more than their worth the effort to keep around. Don't be afraid to delete any and all of that stuf. You use source control, right? You can always get something back if you find out you really need it. Most of the time you never do. So keep your cognitive load light and get rid of old baggage.

There's a good reason the delete button is bigger than most other keys on your keyboard. Use it.

## With all of these, LOOK

As I said before, LOOK is a necessary companion to the four basic sanitation rules.

Looking, _really_ looking, implies being mindful and paying close attention.

Pay attention to things that could trip people up. Pay attention to things that could be confusing or cause misunderstand of what's really going on or how things actually work. Pay attention to what you're doing. Pay attention to what you're NOT doing.

Poorly chosen names that lie or otherwise mislead. Duplicated logic. Copy-pasted code. Too much reliance on another objects' business. Too many parameters. Too many lines of code in a single method. These are things we tend to overlook all the time but they're things that need to be cleaned up before we leave for the day or flip the page to start on a new task.

Be mindful of all of these, _especially_ the little things. A single little Lego brick can hurt the most when you accidentally step on it.

### Look for what can't be seen

Look for gaps as well, otherwise known as pitfalls or gotchas. You'll need a little bit of sophistication here and a bit of effort to learn how to spot things that aren't there. In art, they sometimes call this "negative space."

Missing context, tacit information, implicit assumptions, things that only Alice or Bob knows, these are all things that are NOT reflected in the code. They can cause confusion and misunderstanding. These are traps that unsuspecting newcomers to the code can easily fall into. Even experienced hands can fall into them, too, if they're not careful or forget about them.

Never assume other people will see these traps or figure out that they exist. You know what happens when we [assume](https://www.urbandictionary.com/define.php?term=Assume) things, right?

![Donkey behind a wall](/docs/assets/anna-kaminova-eNNLkFPt8zA-unsplash.jpeg)

Look before you flush, too. We said that most comments should be flushed away but before you do that, also make sure there's nothing in them that are meant to warn unwary programmers of potential dangers.

Sometimes you'll find well-intentioned reminders and warnings from people who cared enough about others who might get tripped up by the code. But just because there's something useful in the comment, doesn't mean you shouldn't flush it. Find out what the gap is and either write a test that clearly expresses the danger as an exceptional case or refactor the code to eliminate the danger altogether.

## No need to wait for someone to teach you how

Given all of the above, I hope you've gathered by now that you already have everything you need to get started with refactoring. With what you already know, you can start cleaning up the majority of the mess that you have to deal with on a daily, hourly, and minute-by-minute basis.

You have what it takes, now get on with it!

## Conclusion

Let me leave you with the same thought that Robert Fulghum ended with in his book.

When you start on your refactoring journey, you'll be like a kindergartner, able to do some simple things with just a pencil and eraser (Rename and Extract). Then you learn how to stay inside the lines with Compose, and draw pictures that make sense, that aren't just a bunch of scratches and squiggles. This is how you start.

But there's also a very important rule that you should remember. It's the one about sticking with your buddies. That's about team. Always work with your team when you refactor. 

And make the code part of it. In a way, the code is its own person, too, you know. In fact, if you all pour yourselves into it, the code is _all_ of you. Those are your collective thoughts and your collective words. 

The code is _your_ story. Own it, and stick with it.

> So it's still true, no matter how old you are—when you go out into the world, it is best to hold hands and stick together. —Robert  Fulghum

![Toys and friends](/docs/assets/hannah-rodrigo-mf_3yZnC6ug-unsplash.jpeg)

## Photo Credits

Girl painting picture. Photo by <a href="https://unsplash.com/@worldorphans?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Nicole Leeper</a> on <a href="https://unsplash.com/s/photos/kindergarten?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Pencil and eraser. Photo by <a href="https://unsplash.com/@kimgorga?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Kim Gorga</a> on <a href="https://unsplash.com/s/photos/pencil-eraser?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Kid holding hand. Photo by <a href="https://unsplash.com/@guillaumedegermain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Guillaume de Germain</a> on <a href="https://unsplash.com/s/photos/kids-crossing-street?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Nice and tidy toys. Photo by <a href="https://unsplash.com/@prochurchmedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Pro Church Media</a> on <a href="https://unsplash.com/s/photos/coloring-book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Donkey behind a wall. Photo by <a href="https://unsplash.com/@annakaminova?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Anna Kaminova</a> on <a href="https://unsplash.com/s/photos/donkey-clown?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Toys and friends. Photo by <a href="https://unsplash.com/@hannahrodrigo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Hannah Rodrigo</a> on <a href="https://unsplash.com/s/photos/coloring-book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Squiggly lines. Photo by <a href="https://unsplash.com/@donovan_valdivia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Andrew "Donovan" Valdivia</a> on <a href="https://unsplash.com/s/photos/coloring-book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Inside the lines. Photo by <a href="https://unsplash.com/@funjabi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sandeep Singh</a> on <a href="https://unsplash.com/s/photos/coloring-book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

{% include post-footer.md %}