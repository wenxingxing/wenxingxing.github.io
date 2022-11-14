---
title: "ICS-CMU notes: Lec 14: ECF Exceptions & Processes"
date: 2022-11-12T09:47:05+08:00
draft: false
---


# Exceptional Control Flow: Exceptions and Processes

## Control flow
- branch and jumps => *program state*
- can't react to changes in *system state*
  - event outside the system: data from disk, keyboard input, network adapter
  - instruction divides by zero
  - timer expires
  - ...

## Exceptional Control Flow
- ubiquitous at all level of computer system


### Asynchronous Exceptions (Interrupts)

- processor's *interrupt pin*
- example:
  - Timer interrupt used by *kernel*
  - I/O interrupt

### Synchronous Exception

- *Traps*
  - Intentional
  - Example: *system call*, looks like a call, but in fact transferring control to the kernel

- *Faults*
  - page faults, recoverable
  - protection faults, unrecoverable

- *Aborts*
  - unintentional and unrecoverable
  - Example: illegal instruction, **parity error**


## Processes

provides two abstractions:

- Logical control flow
  - seems to have exclusive use of the cpu
  - by *context switch*
- Private address space
  - seems to have exclusive use of the memory
  - by *virtual memory*

### `fork` example

fork runs in the *kernel*, it copy every things in the process, including code and data, opened file descriptors, and start a new process.

fork call once and return *twice*, one in the parent process, one in the child process.


```c
pid = Fork()
if (pid == 0) { // child code
    // ...
}

// parent code
// ...
```


```c
/* execution flow:

(uer space) parent call fork 
    -> (kernel space) do stuff, copy, scheduling 
        -> (user space) parent process (next instruction, returned) -> ...
        -> (user space) child process (next instruction, returned) -> ...
*/
```



### Zombie

child process exited but not reaped by the parent, becomes zombie (i.e., defunct)

- `wait`: Synchronizing with Children
- `waitpid`: ..

### `execve`: run a different program

`int execve(char* filename, char* argv[], char* envp[])`

- Load and runs in the current process
- Overwrites code, data and stack
  - retain PID, open files and signal context
- called **once** and **never** returns except error



**todo: change font**