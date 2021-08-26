# Optimization Adventures - Pilot

_(August 26, 2021)_

We often get posts on [the Ranch](https://coderanch.com) from people asking for help on interview questions. A good 
number of those posts have to do with code optimization. Here's an example I saw earlier today: 
```java
String str = "";
for (int i = 0; i > 100000; ++i) {
   str += i;
}
```
(Source: [https://coderanch.com/t/745096/java/Interview#3460935](https://coderanch.com/t/745096/java/Interview#3460935))

The original poster asked for help in figuring out a way to increase the performance of that code. What would _you_ 
say if asked to optimize it?

## The only reasonable answer

> _Premature optimization is the root of all evil._—Donald Knuth

The first response to the post seemed reasonable enough, suggesting the use of a `StringBuilder` instead. While that 
might have proved to be an acceptable answer to the interviewer—which would have made things even worse in this 
case—there's really only one reasonable answer to a request to optimize code: "Don't, just don't. Not yet." 

The next statement I would be looking for as an interviewer would be along the lines of using a profiler to gather 
concrete data before deciding what to optimize. Programmers are notoriously bad at optimizing by gut feeling. Nine 
times out of ten you're going to be wrong. This specific case is no exception.

An eagle-eyed programmer would easily see the reason this code doesn't need to be optimized as it stands. Maybe it 
was trick question, or maybe it was a typing error on OP's part. Regardless of the reason, an echo of Donald Knuth 
regarding the evils of premature optimization should be the first thing you hear when the topic comes up in an 
interview, or in any conversation for that matter.

## Modern compilers are smart, and only getting smarter

I'll only go into this briefly because this isn't the main point I want to make here. Modern Java compilers are pretty 
smart at finding ways to optimize code. And they're only getting smarter with each new release. So don't try to second 
guess the compiler; you're probably not that smart.

## So, what's the answer?

If you still haven't figured it out, that code, as it was originally posted, isn't a performance bottleneck. That's 
because the loop condition is `i > 100000`, not `i < 100000`. Get it? The loop initialization and condition are 
based on hard-coded numbers and given the loop initializes `i` to zero, the loop body will never be executed.

## But what's the real answer?

When it comes to the question of optimization, a good answer would be along these lines:

1. Don't, just don't. Not yet. Clarify the code first.
2. Don't, just don't. Not yet. Simplify the code first.
3. Don't, just don't. Not yet. Use a profiler first.
4. Don't, just don't. Not yet. See how often this code is actually executed first.
5. Don't, just don't. Not yet. ... (_do you get the picture yet?_)

Rather than looking for technical reasons to optimize code, refactor for clarity and simplicity first. Optimizing 
compilers generally have a better chance of finding ways to optimize code when it's simple and straightforward. 
Extract to one-liner methods. This tends to make code easier to read and optimizing compilers can easily inline them 
back in.

After refactoring, use a profiler to gather concrete data. As I said before, nine times out of ten, the performance 
issue will be somewhere other than where you thought it was.

And finally, look at its actual usage first. Is it really worth optimizing if it isn't even getting executed in 
production? You might find that the best way to optimize the code is simply to delete it.

## Choose your battles (and bottlenecks) wisely

The bottom line here is that optimization shouldn't be done on the basis of intuition. Developers are your biggest 
bottleneck in software development. Save _their_ time and you'll save more. Seek first to reduce development time by 
simplifying and clarifying your code. That's where you'll get the most return on your time and effort.
