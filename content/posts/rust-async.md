---
title: "Rust Async"
date: 2023-02-26T15:29:06+08:00
draft: false
---


1. `Future`
1. `async/await`
1. `Pin`
1. `Send`

## Future

`Future` is something you can `poll`, trying to make progress, and won't block if stucked (yield control). And it will call `wake_up` if progress can be made.


```Rust
pub trait Future {
    type Output;

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

## `async/await`

how it works?

1. the executor will repeatedly fetch a `Task` from the queue, try to make progress on a `Future` by calling `poll`
2. if it returned `Pending` then save it back to the `Task`
3. when the future can make any progress, the `wake` will be called, then the `Task` itself will be pushed to the task queue again, so that the executor will be able to `poll` it agagin in the near future.


## `Pin`

use `Pin` to avoid an object being moved, used here as the state-machine in the `Future` may reference itself.

## `Send`

`Send` means transferring control between threads, not `move`-ing objects from one place to another. So it is nothing related to `Pin`