# disruptor

[TOC]

## introduction



> it seems like Java's BlockingQueue. Like a queue the purpose of the disruptor is to move data between threads within the same process.
>
> however there are some key features that the disruptor provides that distinguish it from a queue. they are:
>
> - multicast events to consumers, with consumer dependency graph.
> - pre-allocate memory for events.
> - optionally lock-free

## core Concepts

