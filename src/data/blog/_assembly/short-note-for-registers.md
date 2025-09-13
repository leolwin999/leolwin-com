---
title: Short Note for Registers
author: Leo Lwin
pubDatetime: 2025-03-22T15:10:55.618Z
slug: short-note-for-registers
featured: false
draft: false
tags:
  - assembly
  - tech
  - English
description: TL;DR about registers for you
---

## Table Of Contents


## Intro

> There are a lot of resources about registers so I assume you might already know them. This note is not a deep dive but a recap of how registers actually work.

Think of **registers** as super-fast little drawers inside the CPU. You use them to hold data temporarily while your program is running. They're faster than RAM or disk. If you're doing math or moving stuff around, the CPU wants it in a register.

Ready to take a peek into the registers on the x86_64 (Intel) architecture? :) 


## General Purpose Registers (64-bit)


|    Name   | What It’s Commonly Used For                    | 
|-----------|------------------------------------------------|
|   rax     | Return value from functions, math operations   | 
|   rbx     | Base pointer (can be general-purpose too)      |
|   rcx     | Loop counter, string ops                       |
|   rdx     | Function arguments, I/O ops                    |
|   rsi     | Source index for string/memory ops             |
|   rdi     | Destination index for string/memory ops        |
|   rsp     | Stack pointer (points to top of the stack)     |
|   rbp     | Base pointer (points to base of the stack)     |
| r8 - r15  | Extra general-purpose registers (yay, 64-bit!) |

You might not want to use `rsp` and `rbp` registers because of their functions. (They control how stack works). But you can perform arithmetic operations on them directly, which make them general purpose.

## A Mini Example

```
section .text
    global _start

_start:
    mov rax, 5        ; Put 5 into rax
    mov rbx, 10       ; Put 10 into rbx
    add rax, rbx      ; rax = rax + rbx → 5 + 10 = 15
```
After running this, `rax` holds 15, just like a calculator using registers! Isn't it awesome? 

## Other versions

`rax`, `rbx`, etc. are 64-bit. Want smaller versions? You can use:
- `eax` → for 32-bit version of `rax`
- `ax` → for 16-bit version of `rax`
- `al` →for 8-bit version of `rax`
    
Same goes for `rbx` → `ebx`, `bx`, `bl`, etc.

```
mov al, 0xFF        ; 8-bit
mov ax, 0xFFFF      ; 16-bit
mov eax, 0xDEADBEEF ; 32-bit
```
My dumb way of noting is, a single 'F' in '0xFF' is 4 bit long.  
So 'F + F = 4 + 4 = 8 (0xFF is 8 bit)'.

## Benefits!

What You’ll Learn from playing with registers:
-   How the CPU processes and transfers data
-   The power of using the right register at the right time (Very important!) 
-   Why "low-level" = high control

## Outro

Registers are your best friends when speaking assembly. They can give you the ability to control the CPU like a wizard with a wand. They're fast, flexible, and essential.   
Next time if you're debugging or writing a shellcode, you’ll see these register names _everywhere_.   
Happy Coding!
