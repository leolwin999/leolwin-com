---
title: What the heap? (Part 2)
author: Leo Lwin
pubDatetime: 2025-08-19T16:35:04.625Z
slug: what-the-heap-part-2
featured: false
draft: false
tags:
  - assembly
  - disassembling
  - tech
  - English
description: Let's explore about malloc, calloc and realloc. This is second part.
---

## Table of Contents

## Allocations 
In [Part 1](https://leolwin.com/posts/what-the-heap-part-1), we saw how the heap is managed chaos. You ask for memory, and the allocator carves it up, handing you a pointer.  
Now let's build on what we saw with `malloc` and free and explore two more crucial heap functions: `calloc` and `realloc`.  
  
We'll focus on answering three new questions with our GDB experiment:  
- What's the real difference between `malloc` and `calloc`?
- What happens when you try to resize a memory block with `realloc`? (This is super interesting!)
- How does the system handle very large allocations differently?
  
## The C Code for Our Next Dive
This program will demonstrate `calloc`, and then show the two different behaviors of `realloc`. We'll use getchar() to pause the program at key moments.  
Create a file named *heap2.c*:  
```
// heap_demo2.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void print_bytes(void *p, int num_bytes) {
    unsigned char* ptr = p;
    for (int i=0; i < num_bytes; ++i) {
        printf("%02x ", ptr[i]);
    }
    printf("\n");
}

int main() {
    printf("--- Part 1: calloc ---\n");

    // Allocate space for 5 integers with calloc

    int *c_ptr = (int *)calloc(5, sizeof(int));
    printf("calloc allocated memory at: %p\n", c_ptr);
    printf("Bytes at c_ptr: ");
    print_bytes(c_ptr, 5 * sizeof(int));
    printf("Press Enter to continue...\n");
    getchar(); // PAUSE 1

    free(c_ptr);

    printf("\n--- Part 2: realloc (forcing a move) ---\n");

    // Allocate a 16-byte block

    char *p1 = (char *)malloc(16);
    strcpy(p1, "first block");
    printf("p1 allocated at %p with content: %s\n", p1, p1);

    // Allocate a "blocker" chunk right after it to prevent expansion

    char *p2 = (char *)malloc(16);
    printf("p2 (the blocker) allocated at %p\n", p2);

    // Now, try to reallocate p1 to a bigger size.
    // Since p2 is in the way, it will have to move.

    char *p1_new = (char *)realloc(p1, 32);
    printf("p1 was reallocated to address: %p\n", p1_new);
    printf("Content is still there: %s\n", p1_new);
    printf("Press Enter to continue...\n");
    getchar(); // PAUSE 2

    printf("\n--- Part 3: realloc (expanding in place) ---\n");

    // Now we free the blocker chunk. The space after p1_new is available.

    free(p2);
    printf("Freed the blocker chunk p2.\n");

    // Let's reallocate p1_new again. This time it should expand in place.

    char *p1_final = (char *)realloc(p1_new, 48);
    printf("p1_new was reallocated again to address: %p\n", p1_final);
    printf("Press Enter to continue...\n");
    getchar(); // PAUSE 3


    printf("\n--- Part 4: Large allocation (mmap) ---\n");

    // A large allocation is often handled by mmap, not the heap (sbrk)

    char *large_alloc = malloc(500 * 1024); // Allocate ~500KB
    printf("Large allocation is at: %p\n", large_alloc);
    printf("Check the process memory maps now!\n");
    printf("Press Enter to finish...\n");
    getchar(); // PAUSE 4

    free(p1_final);
    free(large_alloc);
    return 0;
}
```
  
As before, compile with the -g flag
```
gcc -g -o heap2 heap2.c
```
Now, just run the program. It will automatically stop at our first getchar().
```
./heap2
```
  
## Observation 1: calloc Initializes Memory to Zero
  
The program has paused at PAUSE 1. The output shows that calloc allocated memory and our print_bytes function printed a series of zeros.
  
![Heap2 First Pause](@/assets/images/heap2_first_pause.png)
  
This is the key difference: *malloc()* leaves **the allocated memory with whatever garbage values were there before**, while *calloc()* guarantees **the memory is filled with zeros**. This makes calloc slightly slower but safer if you need zero-initialized memory.
  
Press Enter in the terminal where the program is running to continue to the next pause.
  
## Observation 2: realloc Can Move Your Data
The program is now at PAUSE 2. We tried to make p1 bigger, but we had intentionally allocated p2 right after it, blocking its expansion.
  
Look at the output:
  
![Heap2 Second Pause](@/assets/images/heap2_second_pause.png)
  
Aha! The address of our block changed from ...6b0 to ...b00 .
  
This is what happened:
- realloc saw that the chunk at ...6b0 needed to grow from 16 to 32 bytes.
- It checked the memory immediately following it, but saw that the chunk for p2 (...ae0) was in use. There was no room to expand.
- It had to find a new, completely different spot on the heap that was large enough (...b00).
- It copied the data ("first block") from the old location to the new one.
- It freed the old chunk (...6b0).
  
This is a critical lesson: after a realloc, you must always use the new pointer it returns, as the old one may now point to freed memory.
  
Press Enter again in the program's terminal.
  
## Observation 3: realloc Can Expand In Place
The program is now at PAUSE 3. This time, we free'd the blocker p2 before calling realloc. This means the space after our chunk was available.
  
Look at the output:
  
![Heap 2 Third Pause](@/assets/images/heap2_third_pause.png)
  
Look! The address is the same! 0x5607bd270b00.
  
realloc was much more efficient this time:
- It saw the chunk at ...b00 needed to grow.
- It checked the memory immediately following it and found that the chunk that used to be p2 was now free.
- It simply merged the free chunk with the existing one, expanding the chunk in-place without needing to copy any data.
  
Let's verify this in GDB (set the breakpoint after PAUSE 4). Check the chunk header at 0x555555559b00 - 8.
- Before this realloc, it was a 32-byte chunk, so the header was $0x21$.
- Now, we asked for 48 bytes. The allocator will give us a chunk of size 48 + 8(header) = 56, rounded up to the next multiple of 16, which is 64 bytes ($0x40$). The header should be $0x41$.
  
![Heap 2 41](@/assets/images/heap2_41.png)
  
It worked exactly as expected! The chunk grew without moving.
  
## Bonus Observation: Large Allocations use mmap
Now, back in GDB (after PAUSE 4), we'll look at the process's overall memory layout using the `info proc mappings` command. This command shows how different memory regions are mapped.
  
Run it now in GDB:
  
![Heap 2 small and large alloc](@/assets/images/heap2_small_and_large_alloc.png)
  
You will see two key things:
- A region labeled [heap]. This is the main heap where all our small malloc/realloc chunks lived. The system grows this area using the sbrk system call.
- A separate, large, anonymous region of memory (it has no objfile name). Its size will be just over the 500KB we requested. This memory was allocated using the mmap system call.
  
For large requests (typically > 128KB), glibc malloc decides it's more efficient to ask the kernel for a dedicated memory map instead of trying to manage it within the main heap. This avoids fragmenting the main heap with a single giant block.
  
## Final Thought
You have been peeling back the layers of memory management. Keep experimenting and it's the best way to make these concepts stick.
  
Part 3 might be more interesting because it's a full exploit walkthrough. So, I'll meet you there.