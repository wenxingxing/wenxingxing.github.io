---
title: "System-Level I/O"
date: 2022-11-14T17:53:01+08:00
draft: true
---

## Unix I/O

everything is file, series of bytes

`open`, `close`, `read`, `write`, `lseek` 

nasty to handle short count 

## Standard I/O

buffered I/O provided by `libc`, the I/O library maintains a buffer, every time application requires I/O, it first queries the buffer, if the buffer is empty, then use *Unix I/O* to fill the buffer.

## FD

each process has a file descriptor table, with 0 ~ 2 default for `stdin`, `stdout`, `stderr`

### redirect I/O

change the pointer in the descriptor table

```c
/*
(process) file descriptor -> (kernel) opened files, with metadata (check with `man 2 stat`) ->  v-node
*/
```

