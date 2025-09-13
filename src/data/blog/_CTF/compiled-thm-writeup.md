---
title: Compiled THM Write Up
author: Leo Lwin
pubDatetime: 2025-06-25T07:19:59.266Z
slug: compiled-thm-writeup
featured: true
draft: false
tags:
  - tryhackme
  - CTF
  - tech
  - English
description: Walkthrough of the TryHackMe's room - Compiled
---

## Table Of Contents


## Intro

![Compiled Room THM](@/assets/images/Compiled.png)

THM recently released a 5 minutes hack rooms, which are pretty easy even for beginners. This is one of those rooms.  
So instead of doom scrolling, you can test your skills out and spend your time more wisely.

![Download Task THM](@/assets/images/Download_task.png)

Let's download the task file.

![Password Ask THM](@/assets/images/Password_ask.png)

The file is an executable format. Make sure to run `chmod +x Compiled-1688545393558.Compiled` if it's not permitted.  
When we run the file, it asks for the password.

![Try Again THM](@/assets/images/try_again.png)

The password is not that simple, obviously. We need to retrieve this.

## Ghidra

![Ghidra THM](@/assets/images/ghidra_opening.png)

Let's use Ghidra. You can use any reversing tool you prefer.

![Ghidra Import THM](@/assets/images/ghidra_import.png)

Select Files > Import (OR) Press "I" to import our executable file.

![Open in Ghidra THM](@/assets/images/Opening_in_ghidra.png)

Double Click our file to open in Code Editor.

![Analyze Ghidra THM](@/assets/images/Analyze_ghidra_file.png)

Select "Yes" to analyze our file.

![Analyze Ghidra Again THM](@/assets/images/Analyze_ghidra_again.png)

Select "Analyze".

![Choose Main THM](@/assets/images/choose_main.png)

From the left, navigate to symbol tree, select Functions > main.  
To the right is the decompilation of the main function. We'll be focusing on this one.
</br>
</br>
If you're familiar with C, I'm sure you've already guessed the password.  
There's a "scanf" which accepts our input. However, there are some texts in both sides of a format specifier "%s".  
Don't let this confuse you. Let's trace the code in our head step by step.

![Code Logic THM](@/assets/images/Code_logic.png)

Cool! It's `DoYouEven_init`. Is that correct?

![Correct Password THM](@/assets/images/Correct_password.png)

It works...Awesome!

## Outro

Now we gonna put this in the field and fetch our score (and streak).

![Finish Compiled THM](@/assets/images/Finished_compiled.png)

And That's it. Hope you enjoyed this room. Happy Reversing! 