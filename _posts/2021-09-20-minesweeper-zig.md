---
layout: post
title: "Minesweeper with Zig"
categories: game zig minesweeper
date: 2021-09-20 12:22 +0300
---

After porting my CHIP-8 emulator to Zig (repo on [GitHub](https://github.com/Ryp/chip8-emu-zig)) and [writing about it](https://ryp.github.io/emu/zig/chip8/2021/07/04/chip8-emulator-zig),
I decided to come back for some fun and wanted to try my hand on building a small game from the ground up.

Sometimes you win             | Sometimes you lose
:-------------------------:|:-------------------------:
<img width="100%" src="{{ site.baseurl }}{{ site.images }}/minesweeper/screenshot-win.png" /> | <img width="100%" src="{{ site.baseurl }}{{ site.images }}/minesweeper/screenshot-lose.png" />

# Sweeping the mines

Here's a few pain points I noticed coming back to Zig for the second time:
* **Pointers:** Zig has several pointer types, and it can be confusing sometimes when choosing. You'll get back to the docs a few times before getting this right.
* **Initialization rules:** Coming from C++, Zig has quite a different philosophy around variable initialization and the syntax for it. Don't expect to write `var = {}` to get a zero-initialized variable!
* If you want something mutable, get a pointer on it! I bumped my head against this a few times, because I expected things like `var my_var = array[x]` to hold a reference by default. Nope. Get a pointer instead! What's weird here is that the syntax after initialization can stay the same for both variants, so be careful!

# On the bright side

Writing Zig is still a nice experience overall, I especially like the smooth integration of array slices and the ability to get an loop counter super easily.

I used SDL2 for this project again, and there's no surprise - the C-interop is smooth as butter, no fiddling around with wrappers and minimal syntax overhead.

# Play the thing

As usual, here's the code on [GitHub](https://github.com/Ryp/minesweeper-zig).
