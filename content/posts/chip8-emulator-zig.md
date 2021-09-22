---
title: "CHIP-8 Emulator: Zig port"
date: 2021-07-04T12:00:00+03:00
tags: [zig, chip8]
---

After porting a CHIP-8 emulator to Rust (repo on [GitHub](https://github.com/Ryp/chip8-emu-rs)),
I decided to come back to trying new languages, feeling like Rust isn't at a state where I would consider writing new code in it.
So if not Rust, what kind shiny new thing is around on the block?

# Zig

[Zig](https://ziglang.org) has been around some time now, the community is active but didn't grow into large scale yet.
It seems to aim at low-level programmers, as a safer alternative to C with a lean feature set and better performance.
I kept hearing good things about it, so I decided it was time to give it a try and see if it would be a 'better C'.

Starting simple, I took an existing small C++ codebase and began converting bit by bit.
If you want to dive right in, here's the code on [GitHub](https://github.com/Ryp/chip8-emu-zig).

This write-up won't be a full feature tour nor a "*Look at this awesome new language, let's rewrite the planet with it*" piece -
but rather a few notes on what I found noteworthy while doing my port.

# Noteworthy Zig things

## Interoperability with C code

So, one thing I always dislike is how much every new language likes to lock in users into its ecosystem.
Yes Rust, I'm looking at you.

It usually goes like this:
- Use *new shiny language*
- *Good-old library* that you always used before is not available
    * Scenario 1: Someone was here before, went through the pain of writing bindings.
      Great - except they are out-of-date 9 times out of 10.
    * Scenario 2: No luck, your turn to suffer - enjoy writing bindings between two languages that were never meant to work together,
      for an API that is ten times as large as your use case, then maintain it forever please.
- Scratch chin and look back at your life decisions

Instead of this, Zig supports including C code directly in the zig source file.
Just prefix your usual C code with the namespace of your choosing,
prepare to add a few harmless casts, and you're good to go.

Yes. That simple.

## Draw me a pointer

All pointers are non-nullable by default. This is great! No need to add those verbose checks at every function start - pointers you get always point to something.

When you need a nullable pointer, they are supported too with the `?` syntax added to the type.
It's a big win for safety in my opinion.

One slightly more uncomfortable side of pointer are the small zoo of variants that exist. Which one do I use: `*u8`? `[]u8`? `[*]u8`? `*[N]u8`?
Reading the docs makes that clearer but it's not something I expected at first. Expect some fighting with the compiler for array types and operations.

## Defer

Zig does NOT support constructors and destructors like regular OOP languages. My opinion is still torn on this one, but at least we get something new out of it.

The new `defer` (and its friend `errdefer`) allows you to *defer* execution to the end of the scope.
One way to imitate the usual OOP-style contruct is the following:
```zig
foo_ctor();
defer foo_dtor();
...
```

`defer` and `errdefer` integrate very nicely with the rest of the language regarding error handling. I won't go into much more details here, but trust me - it's really nice!

I really don't like not having a one-liner way of writing stuff like profiling macros anymore (and other scope-style objects), but I guess it's not a total deal breaker either.

## No macros

Yup. Goodbye.

## No C-style for loops

You read that right, your beloved `for (int i = 0; i < size; i++)` hardcoded in your fingertips won't work. It simply and purely not supported.

The `foreach`-style `for` is still supported mind you, but the old integer-based counter is not.
You will have to accept to write a while loop, that's not the end of the world though!

Coming to think of it a bit more, the C-style `for` syntax we are so used to is quite weird after all -
three expressions inside of a single parenthesis set? Zig simply forces us to be slightly more verbose, nothing crazy in my opinion.

# Conclusion

There's plenty of interesting subjects I didn't talk about of course, but hopefully this gives you some insights into this shiny language.
On my end, I'm interested into what Zig will evolve into, I see a good potential there.
