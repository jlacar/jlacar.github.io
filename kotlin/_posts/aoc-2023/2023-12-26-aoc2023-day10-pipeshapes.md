---
layout: post
title: Advent of Code 2023, Day 10 - A Pipe Maze DSL
author: Junilu Lacar
banner-img: /assets/images/aoc2023/10-pipe-maze/pipe-maze-1.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 400
banner-height: 400
---
It's after Christmas and I'm still on Day 10 of the Advent of Code 2023 puzzles. That's fine, I'm still learning a lot about Kotlin.

In this installment, I'll look into how Kotlin makes it easy to create a Domain-Specific Language (DSL).

I mentioned DSLs in the last article, where I 

## The Day 10 Puzzle

The Day 10 puzzle has us parsing a huge file that represents a maze of pipes. As with all AoC puzzles, the first task is to turn this input into something that fits our mental model of the problem.

The pipe shapes in the maze are each represented by a character: 
- `'|'` is a vertical pipe that connects at the top and bottom, 
- `'-'` is a horizontal pipe that connects at the left and right sides, 
- `'J'` is a 90-degree bend that connects at the top and left sides, 
- `'L'` is a 90-degree bend that connects at the top and right sides,
- `'7'` is a 90-degree bend that connects at the bottom and left sides,
- `'F'` is a 90-degree bend that connects at the bottom and right sides,
- `'.'` represents the ground which doesn't connect to anything
- `'S'` represents the starting point of the maze.

We don't know what shape of pipe is at the starting point but we can deduce what it is from the fact that it has to connect to two other pipes that are part of a big loop in the maze. We're tasked with finding that big loop and determining the number of steps it takes to get to furthest away from the starting point.