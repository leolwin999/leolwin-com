---
title: Assembly Project Ideas
author: Leo Lwin
pubDatetime: 2025-04-22T08:54:04.346Z
modDatetime: 2025-10-12T05:42:55.593Z
slug: assembly-project-ideas
featured: true
draft: false
tags:
  - assembly
  - tech
  - English
description: Easy assembly project ideas for those who are starting out.
---

## Table of Contents

## Intro

Imagine you are visiting Shanghai.

You're hungry, you decide to try dumplings. You can't speak Chinese, so you bring a translator guy along.
  
You tell him, "I want dumplings," and he translates the order  into  Chinese for the store owner. The store owner wraps some dumplings up, and hands them over. Piece of cake, right?  
  
In this scenario:  
- You are the programmer.
- The translator is the some high-level programming language.
- The shop owner is the kernel.
- And dumplings are the tasks you want the computer to do.
  
Now here's the kicker. What if you want something other than just dumplings?  
  
What if you'd  prefer to haggle the price, compliment the taste, or ask the owner how her day's been? Now, the translator can make mistakes. He might misinterpret your tone, skip details, or not translate the nuance correctly.
  
So what's the solution?
  
Learn Chinese and speak for yourself **directly**!
  
That's exactly what assembly language lets you do. It gives you low-level access to the CPU, letting you fine-tune performance and understand how your instructions run under the hood. 
  
If you’re curious about how computers really work or if you simply enjoy getting some hands-on with bare-metal programming, then these assembly projects are a great place to start.  
  
I'll be posting projects here, sorted from beginner-friendly to more advanced ones. Feel free to explore whatever you find interesting.
  
You can also check out the whole project in my [repo](https://github.com/leolwin999/Assembly-Projects).
  
Happy coding!

> These projects are work in progress. I regularly update contents and add new material, so don’t forget to check back!

## Overview


Easy Categories

|Project Title                                 | Short Description               |
|----------------------------------------------|---------------------------------|
|[Simple Hello](#simple-hello)                 | Simple Hello World              |
|[Below Or Above](#below-or-above)             | Checking a specific number      |
|[Addition](#addition)                         | Adding two numbers              |
|[Input Output](#input-output)                 | Read the text and write out     |
|[Password Checker](#password-checker)         | Checking user's input password  |
|[Prime Checker](#prime-checker)               | Checking if prime number or not |
|[Bit Shifter](#bit-shifter)                   | Shifting each bit to left       |
|[Read File](#read-file)                       | Read the text file              |
|[Write File](#write-file)                     | Write to the text file          |
|[Delete File](#delete-file)                   | Delete the text file            |
|[Clone File](#clone-file)                     | Clone the text file             |
|[Reverse String](#reverse-string)             | gnirts a esreveR                |
|[Random](#random)                             | Generate random number (0 - 100)|
|[Timer](#timer)                               | Simple countdown timer          |

Advance Categories

|Project Title                                 | Short Description               |
|----------------------------------------------|---------------------------------|
|[Server](#server)                             | Basic TCP server                |
|[Client](#client)                             | Basic TCP client                |
|[Web Scraper](#web-scraper)                   | Simple Web Scraper              |
|[HTTP Server](#http-server)                   | Simple HTTP (Web) Server        |


## Simple Hello
A very simple hello program, just like starting out in any programming language. This is your first step into the world of low-level programming. No compilers or fancy libraries but raw instructions, system calls, and a direct conversation with the machine.  
Skills you'll gain:
-   Basic x86_64 intel syntax structure (section .data, section .text, etc.)
-   Writing text to the terminal using syscall
-   Understanding registers like `rax`, `rdi`, `rsi`, and `rdx`
-   Intro to Linux system calls (like write)
-   How memory and code segments are organized in an assembly program

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/simple_hello.asm)

## Below Or Above
A simple program that checks if a number entered by the user is below, above, or within a specific range. It's like a mini decision-maker that shows how logic flows in low-level programming.  
Skills you'll gain:
-   Conditional logic in assembly (`cmp`, `jl`, `jg`, `je`, `jmp`, etc.)
-   User input handling
-   Number comparison using registers
-   Using system calls for I/O
-   Control flow using jumps

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/below_or_above.asm)

## Addition
A beginner-friendly project that takes two numbers, adds them together, and shows the result. This helps you understand how math works at the register level.  
Skills you'll gain:
-   Register manipulation
-   Arithmetic operations (`add`, `mov`)
-   Handling user input for numbers
-   Converting ascii strings to integers and vice versa
-   Printing results to stdout

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/addition.asm)

## Input Output
Reads a user’s input from the terminal and echoes it back. It’s the foundation of interacting with users in CLI-based assembly programs.  
Skills you'll gain:
-   Reading input from stdin
-   Writing output to stdout
-   Using syscall for I/O
-   Working with memory buffers
-   String handling basics

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/input_output.asm)

## Password Checker
A lightweight password-checking program that compares user input with a stored password byte by byte using the powerful `repe cmpsb` instruction. If the input matches, access is granted; if not, it’s denied.  
Skills you'll gain:
- Using `repe cmpsb` for memory/string comparison
- Reading and handling user input
- Defining and storing constant strings
- Working with pointers and memory addresses
- Implementing conditional branches based on comparison results

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/password_checker.asm)

## Prime Checker
A program that asks the user for a number and determines whether it’s prime or not. Instead of relying on shortcuts, it manually calculates the number to check for factors. Straightforward but powerful for learning loops and conditional jumps in assembly.  
Skills you'll gain:  
- Handling user input and converting it into integers
- Implementing division in assembly (`div`)
- Looping through potential factors
- Using conditional jumps for decision-making
- Applying mathematical logic at the register level

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/prime_checker.asm)

## Bit Shifter
This program takes a message and shifts its bits left by one position using the `rol` (rotate left) instruction. It’s a neat way to see how data can be transformed at the bit level, something that’s super common in cryptography, data encoding, and low-level optimization.  
Skills you'll gain:
- Using the `rol` instruction to rotate bits
- Understanding bitwise operations at the CPU level
- Working with ASCII characters as raw binary
- Iterating over strings and applying transformations
- Seeing how bit shifts can be used in encryption/obfuscation

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/bit_shifter.asm)

## Read File
This program reads content from a .txt file and prints it to the terminal. It shows how to interact with files on your system through system calls.  
Skills you'll gain:
-   File handling using system calls (open, read, close)
-   Using file descriptors
-   Buffer management
-   Error checking in system-level code
-   Working with real-world file paths

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/read_file.asm)

## Write File
This program writes contents to a .txt file. It overwrite the file or create one if not existed. It also shows how to interact with files on your system through system calls.  
Skills you'll gain:
-   File handling using system calls (write, open, close)
-   Using different syscall flags
-   Create and overwriting the file
-   Error checking in system-level code
-   Bitwise calculating 

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/write_file.asm)

## Delete File
This program deletes a .txt file. It deletes a file using the unlink() system call and removing the file’s link to its inode.  
Skills you'll gain:  
- Using the sys_unlink system call to remove files
- Understanding inodes and how file paths map to them
- Handling user input for file paths
- Working with strings for filenames
- Error handling when deleting non-existent or protected files

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/delete_file.asm)

## Clone File
This program clones a .txt file, reading its contents and writing them into a new file. A low-level version of “copy & paste,” where you have to handle every byte yourself.  
Skills you'll gain:
- Reading files using `sys_open` and `sys_read`
- Creating and writing to new files with sys_write
- Managing file descriptors properly
- Handling buffers and transferring data between files
- Understanding how Linux performs file I/O at the syscall level

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/file_cloner.asm)

## Reverse String
Enter a string and watch it reversed like magic. This teaches how to navigate and manipulate memory, one byte at a time.  
Skills you'll gain:
-   Working with string pointers
-   Looping through memory
-   Manual character swapping
-   Memory addressing and indexing
-   Buffer usage and length calculation

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/reverse_string.asm)

## Random
Generates a random number between 0 and 100. A fun way to explore randomness without high-level libraries. Only with raw CPU and system calls.  
Skills you'll gain:
-   Using `rdrand` instruction (if supported)
-   Bitmasking and modulo for range limiting
-   File reading for randomness
-   Converting random bytes to integers
-   Simple math operations in registers

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/random.asm)

## Timer
The user enters a number (in seconds), and the program counts down to zero, waiting one second between each tick using nanosleep. Great for learning how to handle time delays and user interaction in low-level code.  
Skills you'll gain:
- Using the `sys_nanosleep` syscall to delay execution
- Countdown logic and looping in assembly
- Converting user input (strings) to integers
- Printing the loop output to the terminal
- Structuring and managing time with timespec in memory

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/timer.asm)

## Server
A basic TCP server that listens on a port and handles simple incoming connections. Writing a server in assembly is not as hard as you think!  
Skills you'll gain:
-   Socket creation with `sys_socket`
-   Binding and listening on a port
-   Accepting client connections
-   Networking syscalls (bind, listen, accept)
-   Low-level networking logic and buffer handling

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/server.asm)

## Client
A TCP client that connects to a server and sends a message. Complements the server project and completes the full communication loop.   
Skills you'll gain:
-   Creating and connecting a socket
-   Sending and receiving messages
-   IP/Port formatting (structs in memory)
-   Network protocol basics (TCP)
-   Error handling in system-level code

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/client.asm)

## Web Scraper
A low-level web scraper built without high-level languages or libraries, just pure assembly and system calls. It connects to a website, sends an HTTP request, and reads the response directly from the socket.  
Skills you'll gain:
-   Working with TCP sockets in assembly (socket, connect, send, recv)
-   Using stack and struct
-   Crafting raw HTTP requests (GET method, headers, etc.)
-   Managing buffers and dynamic content
-   Understanding network protocols (IP, TCP, HTTP) at a low level

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/web_scraper.asm)

## HTTP Server
A HTTP server with raw syscalls and structs. It listens for incoming connections, parses basic HTTP requests, and sends back responses directly from memory. This shows how servers really work under the hood.  
Skills you'll gain:
- Creating and managing sockets using socket, bind, listen, and accept syscalls
- Building and using C-style structs in assembly (like `sockaddr_in`)
- Parsing raw HTTP GET requests manually
- Crafting and sending valid HTTP responses (status line, headers, body)
- Managing memory buffers and handling binary/text data

> Fetch the code [here!](https://github.com/leolwin999/Assembly-Projects/blob/main/Assembly_Files/http_server.asm)

## References

- [19+ Assembly Project Ideas for Beginners to Advanced Programmers](https://yourexcelbuddy.com/assembly-project-ideas/)
