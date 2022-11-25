---
title: "ECF Signal"
date: 2022-11-13T15:46:38+08:00
draft: false
---

# shell

# signals


- sent from the kernel to process
- info: sig id and the fact that it arrived
  - 2 `SIGINT` user typed ctrl-c
  - 9 `SIGKILL`
  - 11 `SIGSEGV`: seg fault
  - 17 `SIGCHLD`, child stopped

## Sending a Signal

- Kernel *sends* a signal to *destination process* by setting some state in the context of the dest process (nothing happens except some bits changed in the context, no immediate impact)
- Kernel sends by two reasons:
  - it detects some event happens, seg-fault, divide-by-zero, etc.
  - another process has invoked the `kill` system call

## Receiving a Signal

- forced by the kernel to react in some way to the delivery of the signal
- Some possible ways:
  - *ignore*
  - *Terminate*
  - *Catch* by `signal handler`

```
sig received -> control passed to sig handler -> sig handler runs -> returns back to the next instruction
```

like a interrupt

## Pending and Blocked Signals

- signal is *pending* if sent but not *received* yet
  - at most one pending sig for any type
  - not queued, subsequent sig of the same type are discarded

- A process can *block* the receipt of certain signal
- use `sigprocmask`

```c
// Temporarily Blocking Signals
// block SIGINT and save previous blocked set
Sigprocmask(SIG_BLOCK, &mask, &prev_mask);

/* Code region that won't be interrupted by SIGINT */

// restore previous blocked set, unblocking SIGINT
Sigprocmask(SIG_SETMASK, &prev_mask, NULL);
```

### Signal Handling 

- use `signal` to install signal handler, which changes the default behavior

- signal handler runs in the same address space as the process, they have shared global state

- handlers can be interrupted by other handlers

### Guidlines for writing safe handlers

- keep it simple
- call only async-signal-safe functions, `printf`, `malloc` not allowed
- save and restore `errno` on entry and exit
- ... refer to the slides


... as I don't write signal handlers, I skipped the following content.

## Summary
To wrap up, signals for a process is just an id, receiving a signal is the event that the kernel force to change the execution path and pass control to the signal handler.

A process can install signal handler by calling the `signal` function. Blocking signals is done by calling `sigprocmask`, which will ignore specified signals until set mask again.
