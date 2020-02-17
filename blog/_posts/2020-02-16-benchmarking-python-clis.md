---
title: Benchmarking Python CLIs
subtitle: Measuring CPU/memory performance during refactors
description: Measuring CPU/memory performance during refactors
toc: true
image: /assets/img/quicksort.gif
hero_image: /assets/img/death_star_laser_tunnel.gif
tags: python kmers beginner
---

## What is benchmarking and why do it?

Benchmarking your codebase can be a daunting challenge. If you're like me, you tend to build large monolithic scripts, occassionally a poorly planned module, and testing suite towards the end of the development process.

To add to the challenge, optimization is the root of all evil in most codebases and yet the difference between working on different versions of the same script... and writing *software*. Benchmarking your code during the optimization process is a necessary evil for many. But for the uninitiated, what do I mean when I say 'optimization'?

* Efficient data structures (.kdb uses `short` [max 65K] and `int` [max 4B])
* Multihreading (concurrent access to 'network' resources)
* Multiprocessing (parallelism)

While an oversimplification of the performance tuning process, the best results are obtained by an attention to the complexity of iterations, data structure sizes, use of parallelism and concurrency, and attention to memory ceilings in the target hardware tiers.

## What challenges are you facing with KDB?

### Memory is a challenge

A challenge I'm facing in the [.kdb](/showcase) project is the question of integer size and array implementation (`numpy.array` vs `array.array` instead of `list`). For example, the number of integers stored in the array is exponential in the choice of k. For non-trivial choices of k, the size is proportional to 4<sup>k</sup>. A choice of k=15 is close to 1.07B integers. A standard long signed integer can take as much 8bytes to store. This would equate to 8 billion bytes, close to 8Gb. While an impressive amount of memory to behold, it would be preferable to cut the amount an order of magnitude. The closest we can get while holding the information in memory is to switch to an unsigned integer.

Alternatively, we could find a way to offload the tallying of information onto the disk. While that would save considerably in terms of the memory allocation, it would result in a much less efficient IO-bound algorithm to calculate anything meaningful from the results. My mentors would likely be proud.

This is a larger problem then it seems. It's not just to save more memory and squeeze the algorithm's profile to less than most desktop workstations have available. An additional milestone would be the ability to generate profiles for k that could be in the terabyte range, which is not possible on conventional hardware. Nor could such profiles be loaded into memory.

### Concurrency vs parallelism

The choice of processing speed largely depends on what part of the code is responsible for the largest delays. Intuiting or isolating that section of the codebase is largely a worthwhile effort.

A good rule of thumb for most beginners is that concurrency should be prioritized when there are a large number (N) of random reads or writes to databases or disk that must be done in addition to calculations that produce the data. Concurrency can be handled by multithreading applications such that threads can be 'waiting' for network latency to resolve while other threads can be responsible for calculation.

In contrast, sometimes with a large amount of data, some simple operations must be done on a large number of inputs. Operating on each input in series to transform each datum can be time consuming and add significant runtime to your application. Preferably, the data should be divided into chunks and handled in parallel.


## How do you test a computer program?

There are several ways to create a performance profile for a calculation. The first type of profile is called a memory profile, and I won't cover this here. I often rely on manual guestimation of memory usage from my [menu-meters](https://www.macinstruct.com/node/423) or using the operating system's Activity Monitor (OSX), Task Manager (Windows), top (Linux). For Python the `memory_profiler` package is recommended and I'd direct readers to [this blog post](https://medium.com/zendesk-engineering/hunting-for-memory-leaks-in-python-applications-6824d0518774) on Medium for more information. 

Every once in a while, you might need to tune a program using basic freeware like GNU time, Bash time, or iPython `%timeit` to really estimate how long a program took to run in userspace. The next idea worth investigating to this regard is how a time command can be run to append individual results to a file, especially useful while investigating how multiple parameters or inputs can influence or vary the runtime of the program.


```bash
parallel -j $CORES '/usr/bin/time --append -o profile.txt --format"%P\t%e\t%U\t%S\t%x" -- my_script.py -x {2} {1}' ::: $(find test/data -name '*.txt') ::: $(seq 1 10)

```
