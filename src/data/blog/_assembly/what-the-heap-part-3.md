---
title: What the heap? (Part 3)
author: Leo Lwin
pubDatetime: 2025-09-24T15:38:36.249Z
slug: what-the-heap-part-3
featured: false
draft: false
tags:
  - assembly
  - reversing
  - tech
  - English
description: It's an exploitation time. This is final part.
---

## Table of Contents

## Flash back
So far, we've learnt the concept of heap in [Part 1](https://leolwin.com/posts/what-the-heap-part-1) and about malloc, calloc and realloc in [Part 2](https://leolwin.com/posts/what-the-heap-part-2). We've built and now it's time to break! :) 
  
## What are we going to do?
We'll explore one of the most classic and conceptually simple heap vulnerabilities: **a Use-After-Free (UAF)**  
The core idea is simple:  
- Memory is allocated for an object.
- A pointer is kept to that memory.
- The memory is free'd, but the pointer is not cleared (it becomes a "dangling pointer").
- A new object is allocated. The memory manager, trying to be efficient, gives us the exact same piece of memory that was just freed.
- The old, dangling pointer can now be used to modify the new object, leading to unexpected and dangerous behavior.  
  
Let's see this in action.  
  
## A Word of Caution
We are about to write and exploit a deliberately vulnerable program. This is for educational purposes only, on your own machine, to understand how software can fail.
  
Never attempt to use these techniques on systems you do not own. The goal here is to become a better, more secure programmer.  
  
## The Vulnerable C Code
We'll create a program that lets a user create a "note". A note has a function pointer inside it that's used to print its contents. The vulnerability will allow us to overwrite this function pointer and make the program call a different, "secret" function instead.
  
Create a file named *uaf_demo.c*:  
```
// uaf_demo.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

// A structure to hold a note and a function to print it
struct Note {
    void (*print_note_func)();
    char data[20];
};

// A simple message structure, conveniently the same size as Note
struct Message {
    char message_data[28];
};

void print_note_content(struct Note* n) {
    printf("Note Data: %s\n", n->data);
}

void secret_function() {
    printf("***********************************\n");
    printf("**** ACCESS GRANTED / PWNED    ****\n");
    printf("***********************************\n");
}

int main() {
    struct Note* note_ptr;
    struct Message* msg_ptr;

    // --- The Setup ---
    // 1. Allocate a Note object on the heap.
    note_ptr = (struct Note*)malloc(sizeof(struct Note));
    note_ptr->print_note_func = print_note_content;
    strcpy(note_ptr->data, "This is a note.");

    printf("Created a note at address: %p\n", note_ptr);
    printf("Its print function is at: %p\n", note_ptr->print_note_func);


    // --- The Vulnerability ---
    // 2. We free the note, but forget to NULL out note_ptr.
    // note_ptr is now a "dangling pointer".
    free(note_ptr);
    printf("\nNote has been freed. The pointer still dangles!\n");


    // --- The Exploitation ---
    // 3. We allocate a new object of a similar size.
    // The heap allocator (tcache) is likely to reuse the chunk we just freed.
    msg_ptr = (struct Message*)malloc(sizeof(struct Message));
    printf("Allocated a new message at: %p\n", msg_ptr);
    printf("--> Notice that the addresses are the same!\n\n");

    // 4. We write into the new object, but this overwrites the old object's data
    // because they are at the same memory location.
    printf("Enter new message content (this will be our exploit payload):\n");
    // read() allows us to input raw bytes, including nulls if needed.
    read(0, msg_ptr->message_data, 28);


    // --- The Payoff ---
    // 5. We use the original dangling pointer, thinking it's still a valid Note.
    // The program will read the function pointer from that memory location
    // (which we just overwrote!) and call it.
    printf("\nCalling the function from the original (dangling) note pointer...\n");
    note_ptr->print_note_func(note_ptr);

    return 0;
}
```

## Compile for Exploitation
Modern compilers have many protections. To make our demonstration clear and reliable, we will disable one called PIE (Position Independent Executable). This ensures that the address of our *secret_function* is the same every time we run the program.
  
```
gcc -g -o uaf_demo uaf_demo.c -no-pie
```
  
## The Walkthrough with GDB
Let's start debugging!
  
The program will run and then pause, asking for input.
  
![Asking for input](@/assets/images/heap3_asking_for_input.png)
  
If we put something, the program simply ended with Segmentation fault.
  
![Segfault](@/assets/images/heap3_segfault.png)
    
First, we need to know the memory address of *secret_function*. We'll use this address as our payload.
  
![Address](@/assets/images/heap3_address.png)
  
Great! The address is $0x401194$. Your address will likely be different, so use the one GDB gives you. Keep this address handy.
  
Let's stop right before the vulnerable function call to see the state of memory.  
Break before the line:  
`note_ptr->print_note_func(note_ptr);`
  
![Break](@/assets/images/heap3_break.png)
  
This is the crucial moment. We need to provide input that overwrites the print_note_func pointer.
- The Note struct starts with the function pointer, which is 8 bytes on a 64-bit system.
- We need to provide the address of secret_function (0x401194) in little-endian format.
Don't worry about converting it manually. We can use a little Python script to generate the exact bytes. Without closing GDB, open a new terminal (Ctrl+T) and run this command. Remember to replace '0x401194' with the address you found in your GDB session!
  
```
python3 -c "import sys; sys.stdout.buffer.write(b'\x94\x11\x40\x00\x00\x00\x00\x00')" > uaf_demo.txt
```
  
This command prints the raw bytes of our address and put the output of this command into text file. The output will look like garbled text, which is perfectly fine.
  
Now, go back to the GDB terminal and re-run the program with our payload text file.
  
The program will continue and hit our breakpoint.
  
![Re run](@/assets/images/heap3_re_run.png)
  
We're now stopped right before the call. Let's look at what's in our *note_ptr*'s memory. The x command examines memory. x/4gx means examine 4 "giant words" (8-byte values) in hex format.
  
![Examine](@/assets/images/heap3_examine.png)
  
Yo! look at that first value! It's $0x401194$. This is the address of *secret_function*. We successfully used our input for the Message object to overwrite the function pointer in the dangling Note object because they share the same memory.
  
Now, let the program execute that single, corrupted line. Instead of `next`, use `continue` to see the result.
  
The program continues, and you will see the output:
  
![Yay!](@/assets/images/heap3_yay.png)
  
Success! Instead of calling *print_note_content*, the program was tricked into calling *secret_function*. We successfully hijacked the program's control flow.
  

## How it happens on the heap?
Heap Allocation:  
When a program needs to store data dynamically, it requests memory from the heap.  
  
Memory Release (Freeing):  
When the program is finished with the data, it frees the memory, returning it to the heap for reuse.  
  
Dangling Pointer:  
A UAF vulnerability arises when a pointer still holds the address of this freed memory.  
  
Memory Reuse:  
If the freed memory is later reallocated and reused by the program, the original dangling pointer might now refer to new, potentially attacker-controlled data.  
  
Exploitation:  
An attacker can exploit this by placing malicious data (in this case, an address of "secret_function") into the reallocated memory. When the program then uses the dangling pointer to access this memory, it inadvertently executes or reveals the attacker's data, which could lead to code execution or sensitive information leaks
  
## Fix and Build again
The fix is incredibly simple, which is why it's so important to be disciplined about it.
  
After you free a pointer, always set it to NULL.
  
```
// In uaf_demo.c, change this:
free(note_ptr);

// To this:
free(note_ptr);
note_ptr = NULL;
```
  
```
// Before using it, add a check:
if (note_ptr) {
    note_ptr->print_note_func(note_ptr);
}
```
  
With these changes, the exploit is completely prevented. The program would find the pointer is NULL and skip the function call entirely, avoiding the crash and the security breach.
  
## Final thought
You’ve gone from metaphors to low-level reality for the heap. That’s rare, most people stop at the textbooks. You didn’t. You learned allocator internals, flags, chunk headers, bins, and how attackers and defenders think.
  
That's the end of our heap series :) 
  
And don't forget, reverse engineering is a challenging and time-consuming process. Don't get discouraged by initial confusion. Consistent effort is key to developing your skills.
  
Keep going. Always! 

