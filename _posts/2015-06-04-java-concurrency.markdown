---
layout: post
title:  "Java Concurrency/Multi-threading"
date:   2015-06-04 01:01:00
categories: java programming concurrency 
tags: java programming concurrency thread lock synchronization
---

In this I will be covering concepts of concurrency, synchronization, internals of concurrency.

## Introduction:
First came the era of Multi-Tasking then came Multi-Threading. This resulted in way too much improvement in performances but also resulted in many complication related to concurrent execution, memory synchronization and stuff. Also we can't ignore the context switching overhead (When a CPU switches from executing one thread to executing another, the CPU needs to save the local data, program pointer etc. of the current thread, and load the local data, program pointer etc. of the next thread to execute).

## Concurrency Models:
A concurrency model specifies how threads in the the system collaborate to complete the jobs they are are given. The concurrency models described in this text are similar to different architectures used in distributed systems.

### 1. Parallel Workers
A delegator distributes the incoming jobs to different workers. Each worker completes the full job. The workers work in parallel, running in different threads, and possibly on different CPUs.

The shared workers often need access to some kind of shared data, either in memory or in a shared database. This results in complications like race-conditions, deadlocks etc. Making shared memory access blocking we can solve these problems to some extent but part of the parallelization is lost when threads are waiting for each other when accessing the shared data structures. Shared state can be modified by other threads in the system. A worker that does not keep state internally (but re-reads it every time it is needed) is called stateless.

Another disadvantage of the parallel worker model is that the job execution order is non-deterministic.

![Parallel Workers Architecture]({{ site.url }}assets/javaConcurrency/parallel_workers.png)

### 2. Assembly Line (reactive systems/event driven systems)
The workers are organized like workers at an assembly line in a factory. Each worker only performs a part of the full job. When that part is finished the worker forwards the job to the next worker. Each worker is running in its own thread, and shares no state with other workers. This is also sometimes referred to as a shared nothing concurrency model.


## References:

1. [Jenkov - Java Concurrency / Multi-Threading Tutorial](http://tutorials.jenkov.com/java-concurrency/index.html)