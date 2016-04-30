---
title: "Shed a Tear for Graphics Programming"
excerpt: |
    I was recently introduce to real-time 3D rendering with OpenGL. The experinece was appalling. This post for a language-inclined, graphics-ignorant audience enumerates what's going wrong.
---
Programming for the dominant real-time graphics APIs, OpenGL and Direct3D, is unfathomably terrible.
CPU--GPU coordination is stringly typed, unsafe, and hard to debug.
The problem is that GPU-side code, in the form of *shader programs*, lives in a separate universe from CPU-side code.
The GPU universe has its own set of impoverished programming languages, and it talks to the CPU via a huge volume of boilerplate.

This post tours a few horrifying realities in a tiny OpenGL application.
You might also be interested in [a literate listing of the full, executable source code][tinygl].

[tinygl]: http://sampsyo.github.io/tinygl/


## Shaders are Strings

A *[shader][]* is a small program that runs on the GPU as part of the graphics rendering pipeline.
The graphics driver JITs shader programs from source code, which are passed in as a string from the application running on the CPU.
n OpenGL, shader programs are written in [GLSL][].

[glsl]: https://www.opengl.org/documentation/glsl/
[shader]: https://en.wikipedia.org/wiki/Shader

*Shaders in strings* are the root of all the evil in this post.
It's like [`eval` in JavaScript][eval], but worse: every OpenGL program is *required* to cram some of their code into strings.

[eval]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval


## Editorial

OpenGL and its equivalents are probably the most popular form of programming for heterogeneous hardware.
But their tools for CPU--GPU coordination are unconscionable.
If the world really is moving toward heterogeneous hardware, we need programming languages that can span the whole system.

I think the central problem is the misconception that there are two separate programs: one on the CPU and one on the GPU, with a loose interface between them.
In reality, programmers are writing one program that needs to divide its execution between the two units.