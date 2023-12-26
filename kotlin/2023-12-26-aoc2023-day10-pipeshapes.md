---
layout: post
title: Advent of Code 2023, Day 10 - A Pipe Maze DSL
author: Junilu Lacar
banner-img: /assets/images/aoc2023/10-pipe-maze/pipe-maze-1.jpg
banner-alt: Image created by DALL-E3 on Bing.com
banner-width: 400
banner-height: 400
---
Kotlin has many features that make it easy to create a Domain-Specific Language (DSL). In this installment, I'll show how I created a DSL in solving the [Day 10 - Pipe Maze](https://adventofcode.com/2023/day/10) puzzle 

It's after Christmas and I'm still on Day 10 of the Advent of Code 2023 puzzles. That's fine, I'm still learning a lot about Kotlin.

I mentioned DSLs in the last article, where I 

## The Pipe Maze Puzzle

The Day 10 puzzle has us parsing a text file that represents a maze of pipes. As with all AoC puzzles, the first task is to turn this text into something that fits our mental model of the problem.

The pipe shapes in the maze are each represented by a character: 
- `'|'` is a **vertical pipe** that connects at the top and bottom, 
- `'-'` is a **horizontal pipe** that connects at the left and right sides, 
- `'J'` is a **90-degree bend** that connects at the top and left sides, 
- `'L'` is a **90-degree bend** that connects at the top and right sides,
- `'7'` is a **90-degree bend** that connects at the bottom and left sides,
- `'F'` is a **90-degree bend** that connects at the bottom and right sides,
- `'.'` is **ground**, which doesn't connect to anything
- `'S'` is the **starting point**. There's a pipe here but we don't know its shape

The problem actually uses the four cardinal directions, north, south, east, and west but I'm using top, bottom, right, and left because for some reason it's easier on my brain.

We don't know what the shape of the pipe at the starting point but we can deduce what it is from the fact that it has to connect to two other pipes that are part of a big loop in the maze. We're tasked with finding that big loop and determining the number of steps it takes to get the furthest away from the starting point.
