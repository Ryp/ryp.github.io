---
layout: post
title: "CHIP-8 Emulator"
categories: emu cpp chip8
images:
  - chip8/brick.png
  - chip8/invaders.png
  - chip8/tetris.png
  - chip8/tictactoe.png
date: 2019-05-06 11:06 +0200
---

Thought it was a fun idea to implement a very basic emulator so I did just that!
My [CHIP-8](https://en.wikipedia.org/wiki/CHIP-8) emu can run most vanilla ROMs without problems.
It's a simple 8bits computer with a rather reduced instruction set, so it wasn't too much pain to get
things going.

I used the publicly available ROMs for testing as well as a bunch of custom unit tests.
I have to say it was a fun little side projet, especially since I didn't have to write any of the games for it,
just load that ROM and here we go!

As usual, the source code is available on [GitHub](https://github.com/Ryp/chip8-emu).

If you want to join the fun and make your own, here's a rather complete [documentation](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#Dxyn) to get you going!

Here are a bunch of screenshots:
<img width="100%" src="{{ site.baseurl }}{{ site.images }}/chip8/tictactoe.png" />
<img width="100%" src="{{ site.baseurl }}{{ site.images }}/chip8/brick.png" />
<img width="100%" src="{{ site.baseurl }}{{ site.images }}/chip8/invaders.png" />
