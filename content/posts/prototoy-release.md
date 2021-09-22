---
title: "Prototoy: offline shadertoy clone in rust"
date: 2021-06-04T12:00:00+03:00
tags: [rust, graphics]
---

Time for reinventing the wheel again!

Shadertoy is great and all, but I didn't find it super comfortable to use.
* Can't use your own editor
* Browsers eat my battery!

Instead of staying mad at things, I took this as a learning opportunity and made an offline version.

## Introducing: Prototoy!

Prototoy is a rather simple program, it allows you to load a shader from your disk,
show it in a window and watch for file changes to hot reload it.
Make that in Rust too of course - because why not - and voilà!

```
ShaderToy Viewer

USAGE:
    prototoy [FLAGS] <shader_path>

FLAGS:
    -h, --help       Prints help information
    -s               only render new frame when the shader changes
    -V, --version    Prints version information

ARGS:
    <shader_path>    path of the GLSL shader
```

Here's the code on [GitHub](https://github.com/Ryp/prototoy) as usual!

## Caveats!

Of course I didn't try to rewrite the full set of features of the original website,
but the core idea is there :)

Feel free to open PRs if you feel it's missing something important!
