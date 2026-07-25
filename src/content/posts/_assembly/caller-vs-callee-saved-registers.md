---
title: Caller vs Callee Saved Registers
author: Leo Lwin
pubDatetime: 2025-04-01T15:18:42.544Z
slug: caller-vs-callee-saved-registers
featured: false
draft: false
tags:
  - assembly
  - tech
  - English
  - short
description: Caller vs Callee Saved Registers, Who’s responsible for what?
---

## Table Of Contents


## Intro

"Hey! I put something in a register, but after a function call or syscall, it’s GONE!!!"

Yeah that happens. In the world of Caller vs Callee Saved Registers, not all registers are equal when it comes to keeping your data safe.

## What are Caller and Callee Registers

In x86_64, we split general-purpose registers into **two types** in accord to **function calls**:

|    Type       |   Registers                                      |
|---------------|--------------------------------------------------|
| Caller-saved  | `rax`, `rcx`, `rdx`, `rsi`, `rdi`, `r8–r11`      |
| Callee-saved  | `rbx`, `rbp`, `rsp`, `r12–r15`                   |


## How Does It Work?

Let’s say you’re writing a function, and before calling another one, you stored some important value in `rax`.  
  
That register is **caller-saved**, the function you're calling is _allowed_ to overwrite it. So it’s **your job** (the caller) to back it up **before the call**, and restore it **after**.  
  
For **callee-saved** registers, the function being called must **save them at the start** and **restore them at the end**, if it decides to use them. You don't specifically need to back it up, the function will do it :)  
  
## Caution!

Suppose you’re doing this in assembly:

```
mov rsi, 0xdad        ; storing an important value in rsi

call some_function    ; this might overwrite rsi!

; Now rsi holds something else...
```

Because `rsi` is a **caller-saved** register, `some_function` can overwrite it.  
If you don’t back it up, your program might **crash**, give **wrong output**, or behave weirdly, especially when doing **syscalls** or working with **external functions**.


## Quick Example

Here’s how you’d save & restore a caller-saved register manually:
```
mov rsi, 0xdad
push rsi           ; Save rsi
call some_function
pop rsi            ; Restore rsi

```


## Outro

_Caller-saved registers (volatile):_ A function (or syscall) is allowed to change these registers freely. Our syscalls use these for saving arguments and return values. They are **constantly changing**.
 
_Callee-saved registers (non-volatile):_ A function is required to make sure these registers have the **same value** when it returns. (Must erase its work and restore what was there before)
  
  
That's all for now, Happy Coding! 