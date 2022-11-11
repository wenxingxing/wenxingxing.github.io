---
title: "ICS-CMU notes: Lec13 Linking"
date: 2022-11-11T19:44:31+08:00
draft: false
---


# Linking

## todo

- [ ] add markmind automatically

## why

- modular
- ..

## ELF Object File Format

- ELF header
- Segment header table
- .text: should be `.code`, named for historical reason
- .rodata: read only data, seprating the HEAP into two parts.
- .data: initialized global data
- .bss: *block starting symbol*, un-initialized global(static) data, 
- .symtab: symbol table
- .rel.text, .rel.data: hints for the linker?
- .debug: debug symbols, line numbers, etc.
- Section header table


## Linker Symbols

- Global: non-`static` func & var
- External: refer symbols defined in other modules
- Local: `static` var & func, C's `private`, infomation hiding, abstration

## Linking

### Step 1: Symbol Resolution
find where is the symbol we're referencing

- Local Symbols
    - Local **non-static** C var vs locl **static** var
        - non-static: stored on the stack
        - ~ : stored in either `.bss` or `.data`

### Step 2: Relocation
- `Merge`
- Combine `.text` together into a single `.text` section, do the same on other sections too.


## Static lib

`.a` files archived into `.o` file, using `ar`

### Linker's algorithm: *cmd order matters*
    - scan .o and .a files in the command line order
    - keep a list of current unresolved reference
    - as each obj file is encountered, try to resolve each unresolved ref in the list
    - if any entries in the list at the end of scan, then **undefined reference**

## Shared lib

- if lib updated, no need to rebuild all the binary referencing it, saving sapce too
- link at load time
- runtime link `dlopen`
- at run time, they are loaded in the middle the **HEAP** space, seprating the HEAP into two parts.

## Interpositioning

- Compile time: by preprocessing, hijack the include header
- Link time: `gcc -Wl,--wrap,malloc -Wl,--wrap,free` ...
- Load/Run-time: `(LD_PRELOAD=...)`