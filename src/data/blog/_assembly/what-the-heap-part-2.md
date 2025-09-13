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
In Part 1, we saw how the heap is managed chaos. You ask for memory, and the allocator carves it up, handing you a pointer.  
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
  
*To be continued...*