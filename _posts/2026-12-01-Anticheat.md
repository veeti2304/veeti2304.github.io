---
title: "Lessons in anti-cheat systems, a learning journey (EN)"
date: 2026-01-12 08:51:00 +0200
last_modified_at: 2026-01-13 16:47:00 +0200
categories: [Blogging, Cyber Security, Game Hacking]
tags: [cpp, lowlevel, anticheat]
description: "My first learning journey in how anti-cheat / cheat systems work"
comments: true
---

# Some lessons in Anti-Cheat and Game Cheats

### Before we get started

#### Notice!
*This is completely new to me and is a sort of learning project for me, so bear with me.*

I expect you to have atleast some knowledge in the game itself, and game hacking in general.

I only posses information about the game, but I do have software development skills (studying it currently by the way) which I help will help with my journey. **But by no means am I a professional**.

I do own the Unheard Edition (which I acccidentaly bought when drunk af.), and I have cheated on that account (STRICTLY ON PVE mind you, I'm not a total asshole.)

So hopefully, I won't get banned for doing my research. After all I hope this will do atleast some good in detecting cheaters in the future since it definitely is a blatant issue in this game, but mostly I'd blame BattlEye for being so fucking easy to bypass. It's ridiculous.

The second victim of my blame is BSG for not taking enough action against cheaters unlike games like PUBG which openly report banned players.

I was fairly drunk when I wrote this, so sorry for any mistakes caused by that :D

  

### Some constants we'll be refering to later.

* ````[data]```` refers to the data directory.

* ```[root]``` simply refers to everything INSIDE the root of the data directory of well everything for the cheat's configs and what not.

* ```[cfg]``` refers to the the directory with config files for the cheats. These can be 2 different directories based on the version of the cheat you are using.
  

### Curent findings

* The main data directory itself is at : ```%appdata%\wepd0nk9 (ref. to as [data])```, where I'm guessing the directory name is randomized. If not, that would be a disaster of it's own. My findings however point me to believe this just a static string, since both of the cheat versions I've tried share this path.

* Most configs have the mysterious ```.6czc``` file extension

* Config paths:

* ```[data]/kmn6idmmgisa/``` - "Pro version" cfg dir

* ```[data]/kuzjh8g3msj2/``` - "Lite version" cfg dir


#### Unknowns (for now):

* ```[data]/[root]/[cfg]/ga8jkomc226/```

* ```[data]/[root]/[cfg]/xe5lzy1vf76/``` - seems to contain an arbitrary looking file (~450kb)

* ```[data]/[root]/iwzde8l9csa.6zc``` <-- seems like a configuration solely based on the file extension. May be something else though.

* ```[data]/[root]/ga8jkomc226``` <-- seems like an empty directory

### About the .6cz file extension:

Most of the known config files seem to have atleast these bytes as their first bytes:

```7F 44 63 24 D0 58 E7 F7 64 7B 38 6C AE EF B8 34 BC 9B 92 A6 B0 F8 0F E8 97 91 15 80 02 4C DE 5A 9F 0E B5 C1 87 B7 CB 12 DC 06 C8 7B E6 09 A1 7C```

but there are several lines of binary that do not seem to change for a while before meaniningful changes to said configs have been made.

I'd guess this means they are encrypted with something like XOR (which is common for cheats) or another similar algorithm.

Example of a simple XOR function:
```c
// Source - https://stackoverflow.com/a
// Posted by Joe Z, modified by community. See post 'Timeline' for change history
// Retrieved 2026-01-12, License - CC BY-SA 4.0

void xor_crypt(const char *key, int key_len, char *data, int data_len)
{
	for (int i = 0; i < data_len; i++)
	data[i] ^= key[ i % key_len ];
}
```

XORing data is a very rudimentary way to encrypt data, which means we may for example be able to find the encryption key in our memory.

The (for now..., subtle foreshadowing) mystery ```iwzde8l9csa.6zc``` starts with the bytes:<br>

```F1 A1 D4 D5 18 1A CE A9 D8 5A 71 FF 2B A4 6C 04 A6 31 93 04 5E 93 C7 45 C6 B0 25 1C 8D 2A 4C 18 AB 45 86 53 D4 50 C2 65 10 1B 6D 2F 7A 71 03 32```

so it looks completely different from the rest of them.

Unlike the rest of the config files, this actually looks to be unencrypted JSON so let's have a look in another chapter.  

I may try to analyze some files for clues, but honestly my **RE skills with low-level programs are terrible**.<br>

Especially when the cheat actively prevents debugging, to the extremes of closing programs like Process Hacker if detected. I'd assume this is easy enough to bypass with the help of changing the executable name and using something like Cheat Engine to

### Other informtion about their "proprieatary" file type

Why did I quote "proprieatary" is due to the fact that they are just using JSON with encryption :D, not exactly high tech. I've seen other games do this as well.

The end of each file, are are just random bytes, and in the beginning I cannot find a clear magic number (once again indicating, it's just a bunch of encrypted JSON).
<sup>(Or maybe XML or a similar format, but why on earth would anyone use something like that :D)</sup>

Most likely this is done to throw off anti-cheats which may access the system FS or doing to find files or doing some other unorthodox methods like this (which in my understanding is definitely not something you'd do most of the time, unless it's a really:
* [invasive anti-cheat which reads your browser](https://www.reddit.com/r/leagueoflegends/comments/aayvu4/lol_reads_your_browser_tabs_is_this_a_gross).
* [anti-cheat reading FS](https://www.reddit.com/r/StallmanWasRight/comments/zmpf15/easy_anti_cheat_is_illegally_scanning_your/)

Mind you these are from reddit and may not be from the most trustful sources, but they are just some examples.

### What is this mysterious .6cz file format?

Well, I'd assume at the end of day it's just a masked name for JSON.

Most cheats I've examined fom GitHub use JSON for configuration, since it's just so easy.

Also this is collaborated by the the next chapter.

### The item data file

The mystery file, which we can now analyze since it's plaintext JSON.

Full contents can be found in a separate JSON file since it's massive. (link posted later)

So, from this we can deduce some of the key:<br>

* "g" - Group of the item such as "Barter item", "Weapon mod" etc.

* "n" - The name of the item shown in game.

* "b" - Most likely an internal identifier of the group, not sure though.

Mystery keys:<br>

* "c"<br>

* "p"<br>

* "t"<br>

I'll be posting the file as a separate one since it seems so massive.<br>

I'd guess by reading it that it is indeed a list of items in the game, most likely used for the Item ESP functionality which helps the player find with loot.

### Conluding
Anti-cheat systems are crucial for modern online games to ensure fair play. During my time playing tarkov I've run into quite a few cheaters who have ruined my complete experience (due to the hardcore nature of the game, dying means losing everything which kind of sucks).

So sadly this concludes this for now, due to my insufficient skills in debugging low-level programs, but I'm sure to keep you in the loop for new developments.

For now, even though unethical, maybe BSG should start scanning for known cheat files?
This would do atleast something...

### Sources
1. [Stackoverflow for the XOR code by "Joe Z"](https://stackoverflow.com/a). Thanks!
2. Mostly my own research.

### What to expect next from me?
I'll try to take a break from this and focus on my "Näyttöprojekti" (Display project) for my studies, but I'll be sure to return to this topic later on when I have more time and energy to focus on it.

A little teaser about it:
It's going to be an RSA encrypted chat messaging application with multiple components written in PHP (backend) and C# (WinForms of course since for now, I won't bother learning any more XAML than I have to) for the win client.

A little side project of mine is also a sort of "RAT" (Remote Admin Tool) or "Remote Access Trojan" depending on how you look at it due to my interest in cybersec. I'll be publishing that on Github later on when it's more polished. But the first project is my main focus for now.