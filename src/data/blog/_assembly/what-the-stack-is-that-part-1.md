---
title: What the stack is that? (Part 1)
author: Leo Lwin
pubDatetime: 2025-07-15T12:22:04.633Z
slug: what-the-stack-is-that-part-1
featured: false
draft: false
tags:
  - assembly
  - disassembling
  - tech
  - English
description: Let's deep dive into how stack works. This is first part.
---

## Table of Contents

## Throw Out the Plates  
  
If you’ve studied Computer Science, chances are you’ve been told the stack works just like a stack of plates. Push a plate on top, pop the top one off.  
LIFO = Last In, First Out.  
Simple, clean and digestible.  
But here’s the thing. Real-life stacks, the ones you’ll meet in low-level programming, C, and assembly, don’t care about your kitchen metaphors.  
Forget the plates. Forget the laundry pile analogy. We’re about to look at what the stack actually is, what it actually does, and how you can mess with it, tweak it, maybe even abuse it a little (in the name of understanding, of course).


## So ... What Is the Stack, Really?
  
In raw terms, the stack is just a region of memory. But it’s not just where your local variables live. It’s also where your function calls get tracked, where return addresses are stored, and where sometimes, mischief begins.  
In most x86_64 systems, the stack grows downward, from high memory addresses to low.  
Is tightly tied to two special-purpose registers:  
- RSP (Stack Pointer): Points to the top of the stack.
- RBP (Base Pointer): Often used to keep track of the function’s frame/base in memory.  
Let’s see what this actually means.  



## A Simple C Program
  
```
#include <stdio.h>

void greet() {
        int x = 999;
        printf("Hello Friend : %d\n",x);
}

int main() {
        greet();
        return 0;
}
```
  
We’re declaring a local variable `x` inside greet(), which gets pushed to the stack. But let’s not stop there. Let’s see this stack behavior in assembly.
  
## Getting Low
  
Compile it first:
```
gcc -fno-stack-protector -no-pie -o stack_test stack_test.c
```
  
`-fno-stack-protector` - used for not to insert the canary (random values) into the compiled executable, so that you get the clean result.
  
> Stack protection typically works by placing a "canary" value (a randomly generated integer) on the stack before the return address. Before a function returns, the canary's integrity is checked. If the value has been altered (indicating a potential buffer overflow), the program aborts, preventing the execution of potentially malicious code.  
  
`-no-pie` -  used to disable PIE.  
  
> PIE (Position Independent Executable) is a security feature that loads executables at random memory addresses to make it harder for attackers to exploit them.  
  
Then disassemble:  
```
objdump -M x86-64,intel -d stack_test
```
  
`-M x86-64,intel` - Disassemble in 64bit mode and Display instruction in Intel syntax  
  
`-d` - disassemble (obviously)
  
Look for something like this in the greet function:

![Objdump Stack](@/assets/images/objdump_stack.png)
  
Each instruction is touching the stack:  
- `push rbp` saves the previous base pointer.
- `mov rbp, rsp` sets up the current stack frame.
- `sub rsp, 0x10` carves out space for local variables (like x).
- `mov DWORD PTR [rbp-0x4], 0x3e7` stores the value "999" into that space. 
  


![Stack Example 1](@/assets/images/stack_example_1.png)
  
  
  
![Stack Example 2](@/assets/images/stack_example_2.png)

## Accepting the Stack for What It Is
  
The stack doesn’t really "pile up". It doesn't wait nicely in layers. It’s raw and structured chaos.
  
Forget what you thought the stack was and learn to see it as it really is. A controlled descent into memory is waiting to be explored.
  
So, Let's head to [Part 2](https://leolwin.com/posts/what-the-stack-is-that-part-2)