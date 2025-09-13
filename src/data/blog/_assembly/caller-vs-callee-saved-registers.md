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
description: Caller vs Callee Saved Registers, Who’s responsible for what?
---

## Table Of Contents


## Intro

"Hey! I put something in a register… but after a function call or syscall, it’s GONE!!!"

Welcome to the world of Caller vs Callee Saved Registers where not all registers are equal when it comes to keeping your data safe.

## What are Caller and Callee Registers

In x86_64, we split general-purpose registers into **two types** when it comes to **function calls**:

|    Type       |   Registers                                      |
|---------------|--------------------------------------------------|
| Caller-saved  | `rax`, `rcx`, `rdx`, `rsi`, `rdi`, `r8–r11`      |
| Callee-saved  | `rbx`, `rbp`, `rsp`, `r12–r15`                   |


## How Does It Work?

Let’s say you’re writing a function, and before calling another one, you stored some important value in `rax`.  
That register is **caller-saved**, the function you're calling is _allowed_ to overwrite it. So it’s **your job** (the caller) to back it up **before the call**, and restore it **after**.  
For **callee-saved** registers, the function being called must **save them at the start** and **restore them at the end**, if it decides to use them. You don't specifically need to back it up, the function will do it :)

## Confused? It's Analogy Time!

_A Simple Analogy: A Phone Call_  
Imagine you need to ask your friend (a math whiz) to solve a problem.
- You pick up the phone and dial his number. **You are the CALLER.**
- Your friend answers the phone. **He is the CALLEE.**  
  
Let's apply the register-saving rules to this analogy:
-  **Caller-Saved Registers** are like your own short-term thoughts. Before you call your friend, if you suddenly got a brilliant idea, you'd better **write it down yourself** (save it). You can't expect your friend to remember it for you. He's busy and will use his own brain (the registers) for his own calculations. Our code knows that after `syscall` is executed, the value in `rax` will be overwritten with the **result**, so we don't expect it to be preserved.
-  **Callee-Saved Registers** are like a shared whiteboard in your room. If your friend (the callee) comes over and needs to use the whiteboard, the rule is that he **must erase his work and restore what was there before he leaves.** He is responsible for cleaning up. That's why we can put a number in the register like `r12` and trust that the syscall won't mess it up. It's a "safe" storage spot.

## Caution!

Suppose you’re doing this in assembly:

```
mov rsi, 0xy0u        ; storing an important value in rsi

call some_function    ; this might overwrite rsi!

; Ugh! rsi now holds something else...
```

Because `rsi` is a **caller-saved** register, `some_function` can overwrite it.  
If you don’t back it up, your program might **crash**, give **wrong output**, or behave weirdly, especially when doing **syscalls** or working with **external functions**.


## Quick Example

Here’s how you’d save & restore a caller-saved register manually:
```
mov rsi, 0xy0u
push rsi           ; Save rsi
call some_function
pop rsi            ; Restore rsi

```


## Outro

_Caller-saved registers (volatile):_ A function (or syscall) is allowed to change these registers freely. Our syscalls use these for saving arguments and return values. They are **constantly changing**.
 
_Callee-saved registers (non-volatile):_ A function is required to make sure these registers have the **same value** when it returns. (Must erase its work and restore what was there before)
  
  
That's all for now, Happy Coding! 