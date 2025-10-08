---
title: How Cheat Codes Work in Pokémon Games (And How to Create One)
author: Leo Lwin
pubDatetime: 2025-10-08T08:57:44.763Z
slug: how-cheat-codes-work-in-pokemon-games
featured: true
draft: false
tags:
  - pokemon
  - reversing
  - game
  - English
description: Join with me on the short journey of tweaking some cheat codes in Pokemon Fire Red
---

## Table of Contents

## Intro
Back in 2015, when I was in middle school, my friend suggested me to play Pokémon FireRed. But my mom only let me play games for two hours on weekends, so it took me about three to four months to become a Pokémon master.  
  
One day my friend handed me a single line of code that gave me unlimited Rare Candy. Unlike legends in the classic ‘90s, I played with an emulator on my tablet, so it was fairly easy to use cheat codes. Just type the codes and enable it, and the magic happens!  
  
I was instantly obsessed. That tiny cheat didn’t just make the game easier but it opened a door to the whole new world.  
  
I was so intrigued that I started learning about other games' modding, using Lucky Patcher, GameGuardian and other tools. The simple joy of changing a number on the screen and seeing the game obey you. That’s what dragged me into the fascinating world of hacking.  
  
This blog's purpose is to help you find a way of sharing your first spark!  
- what cheat codes are
- how they work at a basic level, and 
- how you can create your own!
  
## What you’ll need
To follow this post you’ll want a GBA emulator that has memory-editing tools. I use [VisualBoyAdvance](https://github.com/visualboyadvance-m/visualboyadvance-m) but you can use any emulator you like with memory manipulating and disassembling support.  
  
A gba file of [Pokémon FireRed](https://www.romsgames.net/gameboy-rom-pokemon-red-version/). The emulator will let you run the game and peek inside its memory in real time. That’s where cheat codes do their magic.
  
## A quick glance at CodeBreaker and GameShark
Code Breaker, Game Shark and Action Replay were originally physical hardware cartridges that you plugged into your GBA. They had their own mini-processors and software. Their code formats are instructions for that hardware's software.  
  
They can have different commands for writing different data sizes (8-bit, 16-bit, 32-bit) and can even include conditional logic (like "if player's health is below 50%, then write this value").


## An illegal discount
Let's get started our adventure, shall we?  
  
Skip all the preceding scenes; choosing starter Pokemon, first battle, deliver the parcel back to Professor Oak, etc.  
  
In Pokemon Mart, talk with the owner and you have to choose an item to buy. (we'll buy Pokeball for example). Now let's see if what we can do something about it.  
  
In VBA, Select Tools > Cheats > Find cheat... > Compare Type (Equal), Signed, Data size (32 Bits), Set Specific Value to 200 and Click Search.  
  
![200](@/assets/images/Pkmn_cheat_200.png)
  
You'll see the value 200 at the address 0x0203993C.  
Double click it and set the Description to whatever you want. Change the value 200 to 10 and click 'OK'. Then click 'OK' again and continue the game.  
  
![10](@/assets/images/Pkmn_cheat_10.png)
  
Wait a minute. The value actually changed?  
  
![200 to 10](@/assets/images/Pkmn_cheat_200_to_10.png)
  
Yes!!! You can actually buy 1 Pokeball for only 10 P!  
  
Not just that, you can buy any item you want with only 10 P.  
  
![Other items](@/assets/images/Pkmn_cheat_other.png)
  
Of course, our cheat has some limitations. You can buy only one item at a time with that discount price but it's enough for us to analyze it.  
  
## Diving into A Rabbit Hole:
Go to Tools > Cheats > List cheats... and our Buy 1 for 10 P cheat was like this:  
  
![List cheat](@/assets/images/Pkmn_cheat_list_cheat.png)
  
```
0203993C:0000000A
Type: Generic Code
```
  
Hmmm....it looks really familiar, don't you think?  
  
In Tools > Memory Viewer, search for the address 0203993C and it was like this:   
  
![Memory Viewer](@/assets/images/Pkmn_cheat_mem_view.png)
  
In the address 0203993C, the value is 0000000A, which is 10 in decimal! We did it!  
  
In the left side of the window, you'll see there is a drop down menu for address areas. Since our address lies around 0x02000000, it's in WRAM. But what is WRAM?  
  

WRAM stands for Work RAM. Think of it as the GBA's main workbench or scratchpad.  
- ROM (the game file on your computer) is like the instruction manual. It's big, but it's slow to read from.
- WRAM is the fast, temporary workspace. When the GBA needs to do something, it copies the necessary instructions (code) and data (player stats, item prices, current map info) from the slow ROM into the fast WRAM. The CPU then works directly with the stuff in WRAM.
  
Both the game's code that is currently running and the data it's actively using (like the money and that item price) reside in WRAM.  
  
Back to our cheat, it's the Generic Code - 0203993C:0000000A. Now we've grasped the concept behind it. It's actually a simple instruction.  
  
"Change the value in the address 0x0203993C to 0000000A"  
  
That's it and we got our illegal discount.  
  
The format is like:  
```
<address>:<value>
```
This makes a really good foundation for us to understand how Code Breaker works.  
  
## Code Breaker - Easier than you think
The legendary code for master ball is:  
```
82025840 0001
Type: Code Breaker Advance
```	
Similar to our Generic Code 0203993C:0000000A, but the preceding 82025840 doesn't seem like an address right?  
  
If you have a time, check other [Code Breaker](https://www.pokemoncoders.com/pokemon-fire-red-gameshark-codes/) cheats. Do you notice something?  
  
Yes! All the codes have a preceding 8!  
  
That 8 at the beginning is not part of the address. It's a command prefix.  
  
The code 82025840 0001 can be broken down like this for a Code Breaker device:  
- 8: This is the command. It tells the cheat engine, "I want you to perform a 16-bit write." (A 16-bit write can handle values up to 65535, and it occupies 2 bytes).
- 2025840: This is the memory address, 0x02025840.
- 0001: This is the value you want to write. In this case, 0001 is the item ID for a Master Ball. It's actually the value 00000001.  
  
So, the code is an instruction: "Write the 16-bit value 00000001 to address 0x02025840".  
  
	
## Creating our own:
Since we know the format of Code Breaker, why don't we try turning our "Buy 1 for 10 P cheat" from Generic Code to Code Breaker?  

Generic Code is "0203993C:0000000A"  
  
Add 8 to the preceding bit and slash some bits. So our code become like this "8203993C 000A".  
  
Let's test this out! Go to Tools > Cheats > List cheats... and click (+) to add cheat.  
  
![New Code Breaker](@/assets/images/Pkmn_cheat_new_cb.png)
  
Don't forget to uncheck our previous Generic Code. Now let's go to the Pokemon Mart and see if it's really works or not.  
  
![Code Breaker works](@/assets/images/Pkmn_cheat_cb_10.png)
  
It works!!! To be sure, go to Tools > Memory Viewer to confirm this.  
  
![Observing memory of Code Breaker](@/assets/images/Pkmn_cheat_cb_mem.png)
  
Yes! Our injected value is in the address 0x0203993C. We've just created our own Code Breaker cheat. Isn't that amazing?!  
  
## How do Game Shark codes work?
Game Shark codes look like total gibberish, and there's no obvious "address:value" format. That's because Game Shark codes are encrypted.   
  
Code Breaker speaks a very plain language: "Write this value to this address." (8 2025840 0044), while Game Shark speaks in a secret code to prevent people from easily stealing their cheats or understanding how they work.  
  
Here’s the process:  
- *Creation*: A person finds a simple cheat, like the Rare Candy code. ( For e.g. 82025840 0044).
- *Encryption*: They then use a special program that takes this simple code and uses a secret key (known only to the Game Shark device) to scramble it. This turns the simple, readable code into the long, complex string you see, like EFCE867D 5403D40D.
- *Execution*: When you type that long code into a Game Shark (or an emulator that supports it), the device's internal software uses its secret key to decrypt the code.
- *Translation*: After decryption, the complex code turns back into a simple instruction that the device understands, like "Write the 16-bit value 0044 to address 0x02025840."
- *Action*: The cheat device then executes that simple instruction.
  
So, a Game Shark code is just an encrypted package. The complex string is the locked box, and the Game Shark device has the key to open it and read the simple instruction inside. It was a form of commercial protection to make their codes unique.  
  
## Final thoughts and Where to go next
You now know:  
- Cheats are just memory writes (or slightly more advanced instructions).
- CodeBreaker is a readable, convenient wrapper for direct writes (e.g., 820000A0 000A).
- GameShark may be obfuscated but you can often reproduce its effect by memory-searching.  
  
If you want to level up more:  
- Keep experimenting with an emulator’s Memory Viewer and Find-Cheat/search tools. That’s the fastest hands-on teacher.
- Learn a bit of hex/endianness so you know how values are stored (16-bit vs 32-bit, little-endian on GBA).
- Read emulator dev forums and GBA hacking guides.
- Learn a little ARM assembly (GBA uses a variant of ARM) if you want to patch game code rather than just memory. That opens up scripted cheats and stable patches.  
  
The goal is learning how software works. Use the skills responsibly.  
  
And of course, it's just named as "Creating our own cheat" but in fact, many cheat codes including our illegal discount ones have been discoverd by many hobbyists before.  
  
This is just a demonstration to prove that if we keep exploring and testing, we can discover more and more codes and ways to fiddle with the game.  
  
So stay curious :)  
Happy Reversing!  
  