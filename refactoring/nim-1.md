# Refactoring Randori - Nim (Part 1)

These are some notes on refactorings I did while practicing on an implementation of the [Game of Nim](https://en.wikipedia.org/wiki/Nim)

## Making the code tell a story

I'm a bit obsessed with the idea of making code as expressive as possible. I like to tell people I work with to make 
the code tell a story. If I recall correctly, I first heard Kent Beck talk about code this way, in a podcast about 
JUnit and testing [SE-Radio, 2010] during which he said a program has a story ar

Anyway, I was messing around with an implementation of the Nim game and had gotten the main game loop to this point:

```java=
    public void play() {
        Supplier<Integer> whoIsMoving = player;
        while (pile > 1) {
            whoIsMoving = (whoIsMoving == player) ? computer : player;
            int move = whoIsMoving.get();
            System.out.printf("Pile: %d player takes %d...", pile, move);
            take(move);
        }
        winner = (whoIsMoving == computer) ? "computer" : "human";
    }
```
We'll walk through the story this code is telling, but first a little background.

Nim is a game involving two players. This particular implementation starts with thirteen marbles in a pile. On each 
turn, players must take at least one and at most three marbles from the pile. This implementation was going to be a 
misère game, so it goes on until a player forces their opponent to take the last marble. Whoever has to take the 
last marble loses. That's about it. Seems pretty simple, right?

Now for the code's story. Read the code again and see if you can lay out its story in your head. Now compare it to 
this telling:

We start by setting whose turn it is to move. While there's more than one marble in the pile, we let the other 
player be "it", that is, the one who's going to move. Then we get their move. Then we display some information about 
what's going on. Then we update the pile with the move. We do all that until we're left with only one marble. When 
we exit the loop, the winner will be whoever it was that moved last, leaving the last marble for the loser.

> **Reading the story out loud** 
>   
> What's hard about solo development is that you don't have much opportunity to get independent feedback. So you have 
> to learn to be very critical of your own work and put yourself in someone else's shoes. You want to try to 
> eliminate the inherent bias you have as the author of the code. This takes a lot of practice because for most 
> people, it's hard to criticize oneself.  
>   
> Luckily, I've gotten pretty good at taking criticism from myself after years of solo TDD practice (and being married 
> to a no-nonsense, don't-give-that-sh*t woman). One thing that helped was getting into a habit of reading the code 
> out loud, or if you will, telling the story of the code out loud. That way, I could literally hear myself not 
> making any sense. Try it, you might find it shocking and revealing.

After reading the story out loud to myself, I had a distinct sense of not making much sense. As I listened to myself,
I quietly asked myself "Is that what the code says?" and wherever there was a pause in affirming that, I noted down 
a smell of opacity or vagueness. Again, it takes some time to develop a keen sense of this opacity/vagueness but 
with some practice, it's hard to shake once it becomes a habit.

Reading the while-loop condition out loud as "more than one marble in the pile" seems like a pretty straightforward 
translation of the expression `pile > 1` but the fact that it's a _translation_ raises a yellow flag, which in 
racing mean caution, there's a hazard on the track. 

```java=
    public void play() {
        Supplier<Integer> whoseMove = null;
        while (notLastMarbleLeft()) {
            whoseMove = whoMovesAfter(whoseMove);
            int move = whoseMove.get();
            System.out.printf("%d in pile, %s takes %d, leaving %d%n", 
                    pile, nameOf(whoseMove), move, pile-move);
            take(move);
        }
        winner = nameOf(whoseMove);
    }
```

## References

SE-Radio, Sep 26, 2010. _Episode 167: The History of JUnit and the Future of Testing with Kent Beck_. [Podcast].  
https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/

{% include post-footer.md %}
