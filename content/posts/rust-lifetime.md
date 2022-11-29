---
title: "Rust Lifetime"
date: 2022-11-29T13:36:16+08:00
draft: false
---

## the problem

Dangling reference is bad, we don't want it.

## how rust compiler helps

rust compiler use borrow checker, every reference has a lifetime

```rust
fn main {
  let x: i32 = 0;         //------------------+--- 'b
                          //                  |
  let y: &i32 = &x;       //----+--'a         |
                          //    |             |
  println!("x: {}", y);   //----+-------------+
}
```


