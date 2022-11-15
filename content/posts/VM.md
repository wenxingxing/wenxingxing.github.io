---
title: "ICS-CMU Notes: Lec 17 Virtual Memory"
date: 2022-11-15T13:36:16+08:00
draft: false
---

## Why Virtual Memory

1. for security, isolates address spaces
2. use main memory efficiently, as a cache for the disk
3. simplifies memory management

## DRAM cache organization

Because Disk access is 10k slower than DRAM, so the cost of a page fault is far more expensive than choosing a better victim to evict when there is a conflict or a miss.

### Consequences

- Large page(block) size
- Fully associative
  - Any VP can be placed in any PP(physical page)
  - Requires a *large* mapping function
- Highly sophisticated than simple LRU, but because it's in software (kernel code) instead of hardware as we do in L1, L2 cache, it's relatively acceptable
- Never write through, too slow

## Page Table

*page table* is an array of page table entries (PETs) that maps virtual pages to physical pages, it'a a *per-process* kernel data structure in **DRAM**

```c
/*
Virtual Address -> MMU -> Page Table -> Physical Memory
*/

```

### Handling Page Fault 

- page miss cause *page fault*
- page fault handler selects a victim to be evicted
- Offending instruction is re-executed, then page-hit


## VM as tool for mem-management

- Simplifying mem allocation
- Sharing code and data easier, just map to the same physical address space (`libc.so`)


## Address Translation with page table

Pretty much like L1, L2 cache

```
Virtual Address: [Virtual page number (VPN)] [Virtual page offset (VPO)]

VPN => index into page table => maps to physical page number (PPN)

VPO => maps into Physical page offset


Physical address: [Physical page number (PPN)] [Physical page offset (PPO)]
```

In short, VPN => Index to Page table => PPN

VPO => PPO
		

## Multi-Level page tables

If only one level, then we need (total_mem / page_size) that amount of PTEs, no matter they are used or not.

For 4KB page size, 48-bit address space, 8-byte PET, we need 512 GB page table.
```
2^48 / 2^12 * 2^3 = 2^39 bytes
```

If we use multi-level pages tables, then for those address that are not used, we can omit them, by not allocating sub-level pages tables, and set there upper level entry to `null`

Which means, the top level pages table should always be fully allocated. But lower level pages table could be allocated *on demand*.

In this way we decrease the total amount of pages tables we need to use.


## Locality

Because of locality, the overhead of multi-level page table is negligible, because a single entry of level-1 page table covers a *huge amount of memory space*.

## TLB, **Translation Lookaside Buffer**

exclusive cache for `VPN => page entries`


## Summary

- Programmer's view
  - illusion of exclusive use of the main memory
  - don't need to worry about memory overwrite by other process
- System's View
  - use DRAM as a cache for the disk memory, use it efficiently by caching *virtual memory pages* sits on the disk, by *locality*
  - simplifies management and programming, such as gives linker a good assumption that every program starts at `0x4000000`
  - simplifies protection by providing interposition point to check permission, in the page table
