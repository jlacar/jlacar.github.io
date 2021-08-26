# Revisiting the Debt Metaphor

> *You keep using that word. I don't think it means what you think it means.*—Iñigo Montoya in *"The Princess Bride"*

The term "Technical Debt", coined some thirty years ago by Ward Cunningham, has undergone what Martin Fowler calls 
[semantic diffusion](https://martinfowler.com/bliki/SemanticDiffusion.html) (Fowler, 2006), where a term's 
definition is weakened as it spreads through the wider community.

It's important to understand that the effects of semantic diffusion extend beyond just a term's meaning; it can also 
diminish a term's usefulness in influencing actions and outcomes. In the case of Technical Debt, or more precisely 
perhaps, the Debt Metaphor, semantic diffusion has real economic consequences. 

In this article, I will revisit the Debt Metaphor and elaborate on what Ward Cunningham actually meant when he 
coined it. I'll also look into how semantic diffusion has weakened its meaning and limited its usefulness. Hopefully,
understanding how Ward meant to use the Debt Metaphor will help us regain some of its utility and benefit from it 
the way Ward and others have benefited from it.   

## A short history of the Debt Metaphor

The earliest mention of the Debt Metaphor appears to be in [an experience report](https://c2.com/doc/oopsla92.html) 
(Cunningham, 1992) written by Ward for OOPSLA '92. While there are no explicit references in that paper to 
"Technical Debt" or "the Debt Metaphor" per se, all the relevant ideas are there. What we now know as refactoring 
is referred to as "consolidation", "revise", and "rewrite" and what we consider as code with technical debt is 
referred to as "first time code", "immature code", "unconsolidated implementation", and "not-quite-right code".    

In 2009, Ward posted [a video on YouTube](https://youtu.be/pqeJFYwnkjE) in which he reflects on the Debt Metaphor, a 
[transcript](http://wiki.c2.com/?WardExplainsDebtMetaphor) of which is posted on his wiki. Notably absent from it is 
any mention of "Technical Debt". He does, however, refer to "the Debt Metaphor" consistently throughout the video.  

Please take a few minutes to read the [OOPSLA '92 experience report](https://c2.com/doc/oopsla92.html) and watch 
[Ward's 2009 video](http://www.youtube.com/watch?v=pqeJFYwnkjE) explaining the Debt Metaphor. This will give you the 
context you'll need to better understand the rest of this article.  

## Semantic Diffusion

Context is everything, so it's not surprising that semantic diffusion set in on the Debt Metaphor shortly after it 
started bumping up against other people's experiences. The term "Technical Debt" also soon emerged and became common 
in our lexicon, more common it seems than "the Debt Metaphor", along with interpretations that were perhaps subtly 
but significantly different from what Ward had originally intended.   

Ward called this out in his video, saying "A lot of bloggers at least have explained the debt metaphor and confused 
it, I think, with the idea that you could write code poorly with the intention of doing a good job later and 
thinking that that was the primary source of debt."  

It's unfortunate there aren't more people who know first-hand what it's like to reap the benefits of diligently 
repaying technical debt through regular and timely refactoring. With the state of software development practice 
being what it is, I suppose there will always be more people who see technical debt through lenses clouded by 
painful experiences with programs that are hard to understand and difficult to work with.     

This doesn't mean we should just ignore what Ward originally intended though. All it means is that as long as 
software development practices stay more aligned with the less favorable view of Technical Debt, the term will 
continue to be weakened and its usefulness diminished by semantic diffusion and only a minority will see 
"debt" in the positive light that Ward saw it and be able to benefit from it.   

So expect to see more articles like "[Technical Debt - Bad metaphor or worst metaphor?](https://ronjeffries.com/articles/015-11/tech-debt/)" 
(Jeffries, 2015) in the foreseeable future. Ron points out a number of shortcomings of the metaphor which, in my 
opinion, are the effects of semantic diffusion and applying a different context to the original idea. But at least 
Ron is familiar enough with Ward's original thinking to offer a balanced perspective: "As for Ward’s original notion,
it seems to me that he was referring to a gap between what the code 'understands' and what we understand as its 
authors."   

Other articles abound, however, that clearly misrepresent what Ward meant, like [this recent one]() (Myers, 2021), 
with a definition of Technical Debt ostensibly attributed to Ward but patently different from his actual definition. 

# What did Ward actually mean then?

When he coined the Debt Metaphor, Ward saw going into debt as a positive thing. He said, "I thought borrowing money 
was a good thing. With borrowed money, you can do something sooner than you might otherwise. I thought that rushing 
software out the door to get some experience with it was a good idea." (Cunningham, 2009)

Most of us live with some kind of debt without much of a problem, and we often use debt to our advantage. If you have 
a mortgage, a car loan, or a credit card account in good standing, you're a prime example of this. And that's a good 
thing, isn't it?  

So, at least by Ward's estimation, taking on technical debt is a tactical choice made in order to gain experience 
with the running software sooner rather than later. With that experience, you can learn things you couldn't 
otherwise, even when the software is incomplete, or as Ward put it, "not quite right." The idea was to quickly 
accumulate learnings and reflect them back into the software by refactoring as soon as possible.   

This explains why refactoring done as part of a tight feedback loop, as it's done in test-driven development, works 
very well and why Ward thinks going into debt this way is "a good methodology. It's at the heart of Extreme 
Programming" and that "the debt metaphor is one of many explanations why Extreme Programming works." (Cunningham, 2009) 

The problem comes when people interpret "not-quite-right code" (Cunningham, 1992) as code that's lacking in quality 
somehow or code where you've taken some shortcuts to get software out the door sooner. That's not really what Ward 
meant. More on this later but that's a clear sign of semantic diffusion.  

## No time to get it right, all the time to fix it

With regard to borrowed money, Ward cautions that "until you pay back that money you'll be paying interest." This is 
really where semantic diffusion creeps in, weakening the Debt Metaphor and diminishing its usefulness. 

If your only experience with technical debt has been painful and unproductive, it's more likely that you'll 
misunderstand Ward's original intent and make the mistake of thinking you can write code poorly or take shortcuts as 
long as you repay the debt by refactoring later on.  

However, experience shows there's little time, if any, to refactor later on. We're always under pressure to 
deliver and show progress, and quickly move on to the next feature on the backlog. Who has time to refactor when 
there are so many other things we need to get done? In the immortal words of Sweet Brown, "I ain't got time for that!"  

Not refactoring because "we don't have time to do that" makes no sense, financially or otherwise. Would you rather 
spend more time trying to understand code, tracking down and fixing bugs, and dealing with production issues?
Won't it cost less if we just refactor now and avoid those problems in the future?  

Unfortunately, many organizations carry a heavy technical debt load that would require substantial investment in 
time and effort to refactor before seeing any kind of justifiable returns. In other words, it costs too much to try 
to address the cause of their pain now, so they just choose to carry on living with that pain.  

To compound the problem, when code is deemed to be already working, managers, stakeholders, and even developers can 
lose sight of the long-term economic impact of a heavy technical debt load and question the need to refactor at all. 
"Why fix what isn't broken?" is often asked of anyone who wants to carve out time in their already busy development 
schedules to do the necessary refactoring.   

And therein lies the rub: there's never enough time to refactor now but there's always time to deal with the 
consequences later. 

## What is Technical Debt really about?

Going into debt with poorly-written code is like borrowing money from the mob; not refactoring is like stiffing them.
Nobody in their right mind would ever consider doing that. Well, borrowing money maybe, if you like living like that. 
But stiffing the mob? Definitely not, at least not if you like living at all. We say we'd never do anything crazy 
like that and yet this happens all the time with software.

When we ship code that's lacking in quality, we're taking out a loan we have little to no chance of ever repaying. 
That's Ron Jeffries' main point in his article, and it's a very valid point. But in Ward's metaphor, the primary 
source of debt is *not* a lack of quality. 

If poorly-written code and taking shortcuts are not the primary sources of debt in Ward's metaphor, what is? Ward 
makes it quite clear with these statements: 

"... if we fail to make the program align with what we then understood to be the proper way to think about our 
financial objects then we were going to continually stumble over that disagreement and that would slow us down which 
is like paying interest on a loan."  

"If you develop a program for a long period of time by only adding features and never reorganizing it to reflect 
your understanding of those features, then eventually that program simply does not contain any understanding and all 
efforts to work on it take longer and longer. In other words, the interest is total and you'll make zero progress."  

Ron Jeffries captures it succinctly: the primary source of debt in Ward's original notion is the 
gap between what the code "understands" and what we understand as its authors. 

> *I'm never in favor of writing code poorly*—Ward Cunningham

As if to help us bridge the gap created by semantic diffusion, Ward offers this: "I'm never in favor of 
writing code poorly but I am in favor of writing code to reflect your current understanding of a problem even if that 
understanding is partial." (Cunningham, 2009)  

## There are no interest-free loans in software

Ward likens being slowed down by technical debt to paying interest on a loan. In the experience report, Ward writes 
"Every minute spent on not-quite-right code counts as interest on that debt. Entire engineering organizations can be 
brought to a stand-still under the debt load of an unconsolidated implementation." (Cunningham, 1992)  

Many organizations seem to think they can avoid paying interest by not going into debt at all. But that's not the 
point nor is it realistic. There's always something you don't know, don't fully understand, or couldn't have known 
beforehand as you're developing software. In other words, you're always going to pay interest. That's the basic price 
of admission we have to pay regardless. The question isn't _if_ you'll pay, it's _how much_ you'll pay.   

How much you'll pay, of course, depends on how quickly and how often you pay off your debt. If you don't make 
regular payments on a loan, you'll have to pay extra fees and your interest rate will likely go up because you're 
now a greater risk and liability to the bank.  

Likewise, failing to refactor your program promptly and regularly will increase the risk and liability in the 
program, and you end up paying more interest as all efforts to work on the program take longer and longer and your 
progress becomes slower and slower. Worst-case scenario, the interest becomes total and you make zero progress.  

## How to benefit from the Debt Metaphor

Ward tells us how to use the Debt Metaphor to our benefit at the end of his 2009 video: "... the ability to pay back 
debt and make the debt metaphor work for your advantage depends upon your writing code that is clean enough to be 
able to refactor as you come to understand your problem."  

Just as making regular and timely payments on a loan keeps you out of financial trouble, regularly and timely 
refactoring keeps you out of technical trouble. 

The best way I know to do that is by practicing Test-Driven Development (TDD), making sure not to skimp on the 
refactoring step. Keep the code simple and follow principles like [YAGNI](https://martinfowler.com/bliki/Yagni.html) 
(You Ain't Gonna Need It), [DTSTTCPW](http://wiki.c2.com/?DoTheSimplestThingThatCouldPossiblyWork) (Do The 
Simplest Thing That Could Possibly Work), [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (Don't 
Repeat Yourself), and Kent Beck's [Four Rules of Simple Design](https://martinfowler.com/bliki/BeckDesignRules.html). 
These help instill technical discipline and excellence in the way we develop software.   

With fiscal discipline, you can live comfortably and still thrive with debt. Likewise, by maintaining technical 
discipline and being diligent in refactoring your program to keep it aligned with your evolving understanding of the 
problem, you can stay productive and still thrive with technical debt.  

## TL;DR

When Ward Cunningham coined the Debt Metaphor, he meant for debt to be a good thing, something we could use to our 
advantage. Semantic diffusion has led people to think that poorly-written code and taking shortcuts for the sake of 
expediency were the primary sources of debt. On the contrary, Ward was never in favor of poorly-written code but he 
was in favor of writing code cleanly enough to be able to easily refactor as our understanding of the problem evolves.   

In Ward's original notion, the primary source of debt was the misalignment between our understanding and the 
understanding, or lack thereof, embodied in the program. That debt is paid back by refactoring the code to reflect 
our understanding as best as possible; doing this as soon as possible reduces the amount of interest we pay on the debt.  

## Giving proper credit

Ward Cunningham deserves credit for coining the Debt Metaphor but let's not credit him for what semantic diffusion 
has misled many to think he meant. Hopefully, this article has helped you gain a better understanding of a more useful 
way to think about the Debt Metaphor and what Ward really meant when he coined it.  

# References

Cunningham, Ward (1992, March 26). The WyCash Portfolio Management System. OOPSLA '92 Experience Report. 
[https://c2.com/doc/oopsla92.html](https://c2.com/doc/oopsla92.html) 

Cunningham, Ward (2009, Feb 14). Debt Metaphor. YouTube. [https://youtu.be/pqeJFYwnkjE](https://youtu.be/pqeJFYwnkjE)

Fowler, Martin (2006, Dec 14). SemanticDiffusion. [blog post] [https://martinfowler.com/bliki/SemanticDiffusion.html](https://martinfowler.com/bliki/SemanticDiffusion.html)

Jeffries, Ron (2015, Nov 9). Technical Debt - Bad metaphor or worst metaphor? [blog post] [https://ronjeffries.com/articles/015-11/tech-debt/](https://ronjeffries.com/articles/015-11/tech-debt/) 

Myers, Rob (2021, Apr). Avoid Tech Debt with these "Four Core" Practices. [https://www.agileinstitute.com/articles/avoiding-tech-debt-w-the-core-four-practices](https://www.agileinstitute.com/articles/avoiding-tech-debt-w-the-core-four-practices)

{% include post-footer.md %}
