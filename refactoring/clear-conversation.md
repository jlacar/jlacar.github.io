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

## References

[SE Radio](https://www.se-radio.net/author/dalestrok/) (September 26, 2018), [_Episode 167: The History of JUnit and the
Future of Testing with Kent
Beck_](https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/).

Cunningham, W., (February 14, 2009), [_The Debt Metaphor_](https://www.youtube.com/watch?v=pqeJFYwnkjE), YouTube video.