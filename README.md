> **Warning**
> *This project is still in active development and is NOT in a stable state.*
>
> Contributions—whether through pull requests, issues, or discussions—are greatly appreciated.

# U IR Representation

This repository is the host of the documentation and specifications for the UIR (U IR Representation) format. This is an intermediate step which can be transpiled into other IR formats like [LLVM](https://llvm.org/docs/LangRef.html), or even compiled straight into assembly or machine code through arbitrary compiler backends.

## What is UIR?

UIR is a bytecode format, meaning it is NOT designed with the intent to be human-readable. Instead, it is recommended to be compiled into by something like the U language compiler. As an example, this is what the U language compilation pipeline looks like:

- A human (preferably) writes source code in U
- The U compiler compiles the source into UIR bytecode
- An arbitrary backend compiles the bytecode into machine code

The purpose of this design is to help delegate the work of the compiler better by allowing it to perform optimizations and generate code without having to commit to a single backend.

## Why not use LLVM like everyone else?

While it would be nice to commit to a single backend and call it a day, unfortunately there are pros and cons to everything—compilers included. Using a backend-independent compiler and custom IR means:

- Better flexibility: if your environment requires a certain backend, or you simply prefer one over the other, use it!
- Better hardware support: some backends support certain architectures that others don't.
- Better support for custom/fantasy architectures: no need to parse inconsistent source code.
- Better language: more of our focus goes into making a good language instead of designing it around a particular backend.
