---
title: "Rust Lifetime"
date: 2022-11-29T13:36:16+08:00
draft: false
---

## The problem

Dangling reference is bad, we don't want it.

## How rust compiler helps

Rust compiler use borrow checker to validate references, every reference has a lifetime.

```rust
fn main {
  let x: i32 = 0;         //------------------+--- 'b
                          //                  |
  let y: &i32 = &x;       //----+--'a         |
                          //    |             |
  println!("x: {}", y);   //----+-------------+
}
```


## Explicitly annotating lifetime

Most of the time, lifetime are implicit and inferred. But there are cases when lifetime can be ambigous, such as:

```rust
fn foo(x: &str, y: &str) -> &str {
    // ...
}
```

the return value's lifetime could be equal to `x` or `y`, which is unknown for the compiler. So we need annotate it explicity:

```rust
// in english: return value will be validate, as long as both x and y are valid
fn foo<'a>(x: &'a str, y: &'a str) -> &'a str {
}
// 'a could be 'foo 'bar 'shit...,
// but we use 'a 'b 'c ... in convention
```

then the return values's lifetime is `min(x's lifetime, y's lifetime)`

## Lifetime Elision

Rust compiler use three rules
1. every reference is assigned a lifetime
2. if there are only 1 input reference, then it's lifetime is assigned to every output reference
3. if there are `&self` or `&mut self`, then it's lifetime is ...

After using these 3 rules, if there're still output reference's lifetime is unclear, then compiler will report an error

## Static lifetime

```rust
let s: &'static str = "fuck";
```

s is embedded in the program's binary, (rodata?)
