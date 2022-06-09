---
title: "Zig as a Drop-in C++ Compiler"
date: 2022-06-09T20:37:12+03:00
tags: [zig, cpp, reaper]
---

Alright, mad scientist time: you guys know about [Zig](https://ziglang.org/)?
This language in interesting in itself and I recommend you check it out if you didn't before, but for today we're staying in magical C++ land.

Got the idea from [this blog post](https://andrewkelley.me/post/zig-cc-powerful-drop-in-replacement-gcc-clang.html) that I could use the Zig compiler as replacement for Clang, GCC and MSVC
in my engine and stop having to use Visual Studio on windows.

Sounds enticing, right? Setting up VS is always a pain, especially if your main dev machine isn't Windows, or you want to execute your CI there.

# The Dirty Details

Getting started was quite fast, I already had a [C++ project lying around](https://github.com/Ryp/reaper) and a zig compiler.

The first step is override the compiler, with CMake that meant replacing the `CXX` env var with a Zig invocation and starting a CMake build.

```sh
$ CXX='zig c++' cmake -H. -Bbuild -DCMAKE_BUILD_TYPE=Debug
```

Then you can start building, and hope for the best! This step required me fix a few details:
* Remove precompiled headers - you probably can find a way around this, but for me it was just easier to rip everything out.
* Remove unity build support - CMake has a pluging called `cotire` that lets you do that, to the trash it goes!

An couple of hours of hacking later I was up and running on Linux, and not long after Windows support followed.

On windows I expected it to be a pain, but it really wasn't:
* Download a 50MB tarball of the compiler.
* Setup Ninja and tell CMake about it.
* Remove dependencies on the recent Windows SDK. One workaround to this is to dynamic load those symbols from the windows DLLs instead of relying on the headers.

Zig comes with the compiler binary and for everything else it compiles from source - that means it bundles everything you need for all platforms. You don't need to depend on local stuff, even for the standard library!
Because of that, the Windows headers don't provide us the latest API.

# Profit

The small upside is that I could selectively run on older versions of Windows if I wanted to in the future, and just fallback on the older API.

But finally, we got what we came for, no need for Visual Studio on Windows!

Now that's all nice and everything, but the cool stuff doesn't stop there!
Zig has excellent support for cross-compiling, by design.

# Next steps

If I bothered to set up my dependencies correctly, I could compile everything from windows or linux, and generate binaries for all targets there. Could even add 64-bit ARM or Mac if I wanted!

I tried this on another project with fewer dependencies and it worked flawlessly, maybe I'll spend the time to add it for my engine as well.
Just imagine how much simpler my CI scripts and maintenance would be, I could just setup a Linux CI and be done with it!

Zig also has support for its own build system that is written in Zig, so since I'm already using the compiler, I could go further and start to replace CMake with parts of Zig code.
From there you can go crazy and even slowly port C++ parts of the engine to Zig while having both parts coexist!

# Conclusion

What do we get from all this?
* No need for Visual Studio for compilation on windows
* I can run and compile from VSCode

What more can I get in the future?
* Cross compile to every platform
* Better CI
* Replace CMake with Zig's build system?
* Write parts of my engine in Zig?

That'll be it for this one, thanks for coming to my Ted talk!
