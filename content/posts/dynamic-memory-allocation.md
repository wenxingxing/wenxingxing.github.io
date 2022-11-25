---
title: "Dynamic Memory Allocation"
date: 2022-11-25T20:27:04+08:00
draft: false
---


## what is it?

An interface that connects the `user-code` and `virtual memory` provided by the system. Because a users jut can't use `sbrk` system call to require memory, that would be a nightmare. 不同的操作系统，系统调用还不一样，那么还要维护多套代码，显然是不可能的。

## metrics

- throughput
- fragmentation

## Fragmentation

- Internal: < block-size
- External: there is enough aggregated heap memory, but no single free block is large enough. Hard to assess, depends on *future requests*

## tradeoff

It's all about tradeoff, space-time, throughput/fragmentation.

It's easy to make a very fast allocator, but having bad space efficiency.

## policy

- placement
  - first-fit, next-fit, best-fit
- splitting
- coalescing
  - Immediate
  - Deferred: improve performance of `free` until needed.