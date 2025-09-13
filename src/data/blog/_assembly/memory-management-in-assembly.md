---
title: Memory Management in Assembly (You're the Boss Now!)
author: Leo Lwin
pubDatetime: 2025-06-04T08:20:57.230Z
slug: memory-management-in-assembly
featured: false
draft: false
tags:
  - assembly
  - tech
  - English
description: Memory management plays an important role in assembly. Let's discuss about it!
---

## Table of Contents


## Intro

When you code in high-level languages like Python or JavaScript, memory is handled for you. It’s like living in a hotel. You don’t need to take out the trash, someone else will clean up.  
How about in Assembly?  
You own the house.  
You take out the trash.  
You fix the plumbing.  
And if you mess up, your house burns down (Seriously!).  


## How Memory Works in Assembly (x86_64)


Assembly program’s memory is split into sections:  
- `.text`
Where your code lives (read-only)
- `.data`
Initialized variables
- `.bss`
Uninitialized variables (get 0 by default)  
  
**Heap**  
Grows upward for dynamic memory (malloc)  
**Stack**  
Grows downward for function calls, local vars


## Managing Memory Manually

If you want to allocate memory, you might use:
- `malloc` (via syscall or libc)
```
mov rdi, 64       ; Allocate 64 bytes
call malloc       ; rax now points to your new memory
```
  
- `mov` to store data:
```
mov [rax], 0xDEADBEEF    ; Store value at the memory address
```
 
 - `free` to release:
 ```
mov rdi, rax     ; Pass the address back
call free
 ```

If you forget to `free`, the memory will sit there **forever**


## No Auto Cleanup Here

In Python:
```
a = [1, 2, 3]
del a
```
Lemon squeezy!  
  
But in assembly, you have to allocate memory, write to memory and MUST free it all manually.  
  
If you fail? Well, prepare to face:  
- **Memory leaks** (your program slowly eats RAM)
- **Garbage values** (leftover junk in memory causing weird bugs)
- **Segfaults** (accessing stuff you shouldn’t)

## Why You Should Be Cautious

You think you freed memory but then try to access it again like:  
```
mov rdi, rax
call free
mov rbx, [rax]   ; use-after-free (undefined behavior!)
```
Or, you forgot to allocate _enough_ memory:  
```
mov rdi, 2
call malloc                         ; only 2 bytes!
mov qword [rax], 0x1122334455667788 ; Ugh...buffer overflow!
```
In C or Python, you’d get an error.  
In Assembly, it crashes or even worse, **keeps running with corrupted memory** prone to security risks.

## The Great Power

Assembly gives you the raw power. It's not just coding. You’re given a fully access to control your whole computer.  
But that ultimate power comes with responsibility. Memory management is like a double-edged sword. You can build fast, efficient programs as well as can also crash the system, leak memory, or even open up vulnerabilities.  
>That explains why most hackers use assembly language. Its ability to interact directly with computer hardware and allows fully control over system resources is crucial for tasks where higher-level languages are insufficient and inefficient.  
  
So be careful (and proud) because you’re now literally the garbage collector :) 


## Quick Tips

- Always **initialize memory** before use
- Don’t forget to **free** dynamically allocated memory
- **Never** access memory you didn’t allocate (Segfault)
- **Comment everything** when managing stack and heap manually or writing a long program
- Use debuggers


## Outro

Memory management in Assembly is indeed energy draining but man, it's crazy cool. You understand how the system really works!  
You’ll learn how stacks grow, how pointers behave, how programs crash and also how to stop them.  
And whenever you do assembly keep in mind that:  

![Uncle Ben Quote](@/assets/images/uncle_ben_quote.gif)

Happy Coding! 