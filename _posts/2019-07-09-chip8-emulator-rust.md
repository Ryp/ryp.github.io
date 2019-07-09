---
layout: post
title: "CHIP-8 Emulator Rust Port"
categories: emu rust chip8
date: 2019-07-09 17:00 +0200
---

**tl;dr:** I ported my CHIP-8 emulator to Rust.

Amongst game dev circles, Rust is certainly getting a lot of attention recently,
especially since Ready At Dawn announced they would be [switching entirely to
Rust for newly-written code](https://twitter.com/AndreaPessino/status/1021532074153394176).
This is certainly a big thing, coming from such an experienced studio
(we gamedevs are notoriously picky when adopting new libraries/languages),
and proves that Rust is taken seriously for replacing C++.

Personally, I tend to dislike C++ and its ecosystem, and couldn't be happier
that new languages try address the need for a superior paradigm. C++ is full of
traps and requires a lot of discipline to produce maintainable code.

I thought it would be a nice start to just take existing code and try to see how
well it would translate from C++. I simply took my previous CHIP-8 emu and started
to chip at it with the help of the docs.

If you want to dive right in, here's the code on [GitHub](https://github.com/Ryp/chip8-emu-rs).

Anyway, it was my first go at Rust so go easy on me! =)

## First impressions

I must say I was pleasantly surprised with the experience overall. The docs are
available and uploaded to a common platform, which makes them easy to read since
the format is always the same.

Adding dependencies is a breeze, and the whole buildsystem (`man cargo`) is simple enough to
get things going fast. Being used to fiddle with CMake regularly, this is a **big**
improvement.

I wish that rust packages and my distro packages would use the same software
though... It really feels wrong to have each new language rewrite its
own package manager from scratch. Stop reinventing the wheel!

The best feature in my eyes for now is the way Rust handles ownership. The fact
that ownership can be asserted at compile time is a very powerful asset.

Rust provides you with the right tool to deal with memory safely, with array slices for
example. This allows Rust to warn you when manipulating two variables that point to the same
mutable memory in the same scope for example.

This is only a superficial overview of what I've been able to see so far, but
that gives you a rough idea of my initial impression with this tech.

## Debug builds

The big issue I'm seeing for now is the debug performance. My first working
executable was running dog slow! Get that: for one rendered frame of the emu in
~1080p, I would spend **~1000ms** filling the backbuffer on the CPU!
The release build would run at about **42ms** for the same test.

After profiling it seems like Rust spends a LOT of time validating,
almost all of that time was spent in iterator and range debug validation.

My initial [backbuffer filling routine](https://github.com/Ryp/chip8-emu-rs/blob/master/src/sdl2/backend.rs#L80) was of course naive and non-optimized, so
after spending a couple of hours improving it, I got this result:

| **Debug**   | 500ms |
| **Release** | ~7ms  |

The gain from 42ms to 7ms was expected in my release build, as I roughly cut the
work needed in my code by 8, but I was not expecting my debug perf to stay at
500ms.

## Conclusion

I don't know what to think about the debug times yet, I probably need to ask
around and see if that's an isolated case or if there's a way to mitigate this.

Despite that, Rust definitely feels like a well thought-out language and I'll be
programming with it more in the future!
