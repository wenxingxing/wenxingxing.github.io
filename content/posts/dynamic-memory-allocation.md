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


## utilization

H_k: heap size after k-th request
P_k: aggregated payloads after k+1 requests

peak memory utilization 
todo: math tex
```tex
u_k = (max_i<k R_k) / H_k
```


## Implicit free lists

every block use the header save the `size` and `in-use` info, then use `size` to determine the `next-block`,
in this way we call it a `implicit` list, because we're not using a pointer.

trick: because block is aligned by x-bytes (known ahead), the lower x-bits of the `size` is always 0, so we exploit this fact, and save `in-use` info in the lower bits.

## Explicit free lists

every free block has pointers pointing to next free blocks, could be double-linked list

Then we need to handle different `coalesce` case
- `use` -> to-be-freed -> `use`
- free -> to-be-freed -> `use`
- `use` -> to-be-freed -> free
- free -> to-be-freed -> free

## Segregated free lists

put different sized free blocks into different list classified by there size, by far the most fast and efficient way of implementing allocator

## Garbage collection

The idea: 

1. what is garbage: blocks that will never be accessed by user-code
2. how to go gc: mark & sweep
   1. start from root node, do dfs-traversal, mark each block visited, (as non-garbage)
   2. iterate all blocks, collect those not marked blocks (garbage)

problem: 
when pointers point to the middle of a block, we can't determine it is a pointer, or just a big number.

solve:
- conservative gc: just assume it is a pointer
- use a binary tree, 记住所有block的start和end, 对于一个pointer，做查找，如果存在一个block包含了这个pointer，那么这个block就不是garbage

## C-pointers 

refer to K&R page 53