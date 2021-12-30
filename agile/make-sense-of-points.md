# Making sense of Story Points using Dimensional Analysis

[Dimensional analysis](https://en.wikipedia.org/wiki/Dimensional_analysis) is something I learned in school while studying mechanical engineering. We used the technique to convert quantities from one system of measurement to another, say from imperial to metric. It's also a useful way to sanity check calculations based on basic rules of algebra and the units of the quantities involved.

In this article, I'll attempt to make sense of calculations that involve Story Points and Velocity using dimensional analysis.

## An example from physics

To demonstrate the use of dimensional analysis for calculating, sanity checking, and converting from one system of measure to another, let's start with a simple and familiar problem.

Suppose we need to calculate the average velocity of a car that travels 100 meters in 8 seconds.

Suppose further that we have forgotten the formula for calculating velocity given the distance and time. 

No problem, we can still figure out the answer with dimensional analysis, assuming we haven't forgotten that velocity is measured in units of distance per unit of time. That is, given the distance in meters and time in seconds, the average velocity will be given in meters per second, or m/s for short.

Dimensional analysis allows us to set up an equation using just units of measure, like this:

```text
    m/s = m/s
```
Which is really just a shorthand for this:
```text
    1 m/s = 1 m/s
```
This is in accordance with a basic rule of dimensional analysis: the two sides of an equation must be commensurable or have the same dimensions.

Plugging in the known values in the places where they logically fit based on their units, we get:

```text
    (velocity) m/s = (distance) 100 m / (time) 8 s
```

Therefore,

```text
    (velocity) m/s = 100/8 m/s
    (velocity) m/s = 12.5 m/s
```

From the above calculations, we can see that velocity = distance / time and that a car that travels 100 meters in 8 seconds moves with an average velocity of 12.5 m/s.

### Conversion

What if we wanted to know the velocity in miles per hour? We can use dimensional analysis to do the conversion, too.

Here are some equivalences I can remember off the top of my head:

  * 1 min = 60 s
  * 1 hr = 60 min
  * 1 mi = 1609 m

With that, we can set up our equation and do a quick sanity check by arranging the units to make sure they can be reduced to mi/hr:

```text
    m/s = m/s
        = (m/s ⋅ s/min) ⋅ min/hr ⋅ mi/m
        = (m/min) ⋅ min/hr ⋅ mi/m
        = (m/min ⋅ min/hr) ⋅ mi/m
        = (m/hr) ⋅ mi/m
        = (mi/m ⋅ m/hr)
        = mi/hr
```
Collecting and simplifying terms, we can see that the units can be reduced to mi/hr which is the system of measurement we want. This confirms we have everything we need to convert from m/s to mi/hr.

Plugging in our conversion factors, we get:

```text
    (velocity) 12.5 m/s = 12.5 m/s ⋅ 60 s / 1 min ⋅ 60 min / 1 hr ⋅ 1 mi / 1609 m
```
Combining terms and cancelling units, we have:
```text
    (velocity) 12.5 m/s
        = 12.5 m/s ⋅ 60 ⋅ 60  s/min ⋅ min/hr ⋅ 1 mi / 1609 m
        = 12.5 m/s ⋅ 3600 s/hr ⋅ 1 mi / 1609 m
        = 12.5 ⋅ 3600 m/s ⋅ s/hr ⋅ 1/1609 mi/m
        = 45000 m/hr ⋅ 1/1609 mi/m
        = 45000/1609 mi/m ⋅ m/hr
        = 27.9677 mi/hr
```
Therefore, 12.5 m/s is equivalent to roughly 28 mi/hr.

## Story Points and Velocity

Now let's try to do dimensional analysis with Story Points and Velocity (with a capital V, to distinguish it from the physics variety of velocity).

First, we need to know what kind of units we're dealing with. Let's start with Velocity.

The [glossary at scrum.org](https://www.scrum.org/resources/scrum-glossary) defines Velocity as
> an **indication of the amount of Product Backlog** turned into an Increment of product during a Sprint

Okay... an indication of the amount of Product Backlog. A Product Backlog Item is usually a story, so the amount of stories? What's the unit for a story? That would story points, right?

The [glossary at agilealliance.org](https://www.agilealliance.org/glossary/velocity/) has this in its entry for Velocity:
> the user stories remaining represent a total of 40 points.

Great, so stories are measured in points. But what's a point then?

Maybe this article about [improving estimation with story points](https://www.agilealliance.org/resources/experience-reports/improving-estimation-story-points/) will give us some answers:
> Story points are **relative measurement of the size and complexity of the user stories** wherein a base story is assigned some story point/s to start with and rest of the stories are estimated in story points based on its size and complexity compared to the base story

So points are a relative measurement. Okay, so like a ratio or a percentage. A percentage of what? Size and complexity?

What are the units for size? For complexity? It can't be points, because that would be a circular definition. You can't define a unit by itself. 

This is getting confusing as heck.

Wait, let's check Mike Cohn's _Agile Estimation and Planning_ book. That's kind of the handbook for Agile estimation and planning, right? There's gotta be some definitive answer in there.

Here we go, in chapter 4, Mike writes:

> There is **no set formula for defining the size of a story**. Rather, **a story-point estimate is an amalgamation** of the amount of effort involved in developing the feature, the complexity of developing it, the risk inherent in it, and so on.

Ok, so there's no set formula for defining size, but a story-point estimate is an [amalgamation](https://www.dictionary.com/browse/amalgamate), i.e., a combination.

A combination of what? Let's see...

Effort. Ok, so what are the units for effort? Again, it can't be points, otherwise, circular definition.

Complexity. There it is again, but what are the units for it?

Risk. What's the unit of measure for risk?

And... oh nuts. What the heck is "and so on"? How do you assign units to that?!

Looking further down the page, Mike starts talking about dog points. What?! He's got a list of eight dogs that he assigns "dog points" to.

According to this, a dog point is the height of the dog at the shoulder. So why not just say inches or feet? Why do we have to say "dog points"? What's the point of that?

Ooh, there's a table on the next page. Maybe this'll clear things up.

No, wait, it's not even sorted! Who does that? 5, 3, 10, 3, 1, 5, 9, 3. What is that, height in inches? 

Wait, he's got three 3s and two 5s there! What is going on here? I'm confused as heck now.

Let's see how he explains this:

> I determine these values by starting with Labrador retriever. This breed seems medium-size to me, so I give it a five. Great Danes seem about twice as tall, so I give them a ten. Saint Bernards seem a little less that twice as tall, so I give them a nine. A dachshund seems about as short as a dog gets, so I give them a one. Bulldogs are short, so I gave them a three. However, if I had been estimating dog points based on weight, I would have given bulldogs a higher number.

Ok, so it's a relative size again. But stories, they don't have a height. How do you eyeball a story's height? I still have no clue what size is about. 

Man, this guy's all over the place! Not only does he _not_ sort the table by size, he randomly jumps around the list when he explains his "dog point" choices. He doesn't explain all of them either: he said nothing about the Terrier, Poodle, or German Shepherd! 

No, wait, further down he does mention the terrier and the poodle and they're supposed to have the same number of dog points because they're about the same size. He says something about a loosely-defined story (or dog). 

Stories and dogs, hmmm. The analogy seems flawed. Aren't these two as comparable as video games and shoes? I mean how do you size a video game? 

These are called _incommensurable quantities_ in dimensional analysis, like how you can't compare miles to pounds or hours to inches. It doesn't make sense. None of this does! Ahhhgh!

Ok, so with dogs, he estimates by height (i.e. inches), but then he says he could have also estimated by weight (pounds) and that would have given different numbers. Well, duh! Incommensurable, buddy. And it seems kind of random to me to pick one or the other.

I don't know about this, it's not clearing up anything for me. So what now?

Let's go back to [agilealliance.org](https://www.agilealliance.org/) and that [points estimation article](https://www.agilealliance.org/glossary/points-estimates-in/). It says here that

> "Velocity,” in the sense Agile teams use the term, has no preferred unit of measurement, **it is a dimensionless quantity**.

What? So Velocity is a dimensionless quantity?!

(checks notes) scrum.org says that it's an "indication of the amount of Product Backlog turned into an Increment of product."

Are we supposed to amalgamate those into "Velocity is a dimensionless indication of the amount of Product Backlog turned into an increment of product"?

Now we have Velocity as a "dimensionless indication of the amount"? Looking up the meaning of amount, it's defined as

> the quantity of something, especially the total of a thing or things in number, size, value, or extent.

That last word, extent, might be something there. Extent, as in the amount of pain, confusion, and frustration this exercise is causing me? Yeah, I'm feeling quite a bit of that right now.

So is that how we're supposed to quantify Velocity? With dimensionless indications of amount like "a lot", "a little", "a bit", or "a bunch"?

"We delivered _a lot_ of stories this increment."

"We completed _a bunch_ of stories that the customer needed."

"We had _a few_ bugs that escaped into production."

Well, at least it makes sense, but I don't think it's going to fly, and certainly not in a math equation.

Let's take yet another tack. The [agilealliance.org entry on Velocity](https://www.agilealliance.org/glossary/velocity/) says

> Knowing velocity, the team can compute (or revise) an estimate of how long the project will take to complete

Great, so now we're back to time. "How long it will take to complete" is time, right? I thought these things aren't supposed to be about time. Gah.

So Velocity is also an indication of time. But if it's a dimensionless indication of amount, how can it also be the amount of time? Time isn't dimensionless; we have seconds, hours, days, weeks, months, _and so on_. Right, Mike?

One last tack. I heard somebody say Velocity is value. Value to the customer, that is.

Ok, but then what about [this article](https://www.scrum.org/resources/blog/scrum-myths-velocity-value) that says

>  Velocity and value are very different things.

And then it goes on to say

> In the context of a Scrum Team, value is only created when the product (increment) reaches the customers... the real value can only be measured once the product has been released and is being used.

So, value isn't even real until the product is actually being used by the customers. So, what, are we measuring _fake_ value when we're still planning and estimating?

One last try. [This article](https://scrum.courses/2019/03/26/what-is-scrum-team-velocity-measurement/) says:

> velocity is a measure of the team’s ability to deliver value to the custom

Yeah, well buddy, it was just established that value isn't a thing until the software is in the hands of the customer and is being used. So how do we measure value if the software is still in our hands and we haven't completed anything yet?

That's it, I give up. This is ridiculous.

## Conclusion

As you can see, I am unable to make sense of Story Points and Velocity, much less do any calculations with them using dimensional analysis. That whole exercise was a non-starter because without clear units of measure, dimensional analysis or any calculation involving Story Points and Velocity is literally impossible.

I therefore conclude that from a mathematical perspective, Velocity and Story Point calculations make no sense. Since logic is mathematical, it follows that Velocity and Story Points are also illogical.

Please prove me wrong.

{% include post-footer.md %}