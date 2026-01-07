# C Notes

## C jargon

statement: instruction, call a function

## `printf`

`printf` uses `format specifiers` that start with the percent (`%`) character (`%g`). Escape characters start with backslash (`\n`).

## Development Environment

`MinGW` (Minimalist GNU for Windows) is a free and open source software development environment to create Microsoft Windows applications. It includes: gcc

The `GNU Debugger (GDB)` is a portable debugger that runs on many Unix-like systems and works for many programming languages, including Ada, Assembly, C, C++, D, Fortran, Haskell, Go, Objective-C, OpenCL C, Modula-2, Pascal, Rust, and partially others.

`ucrt` stands for `Universal C Runtime`. It provides the fundamental functions that C and C++ programs need to run on Windows.

### MSYS2

`MSYS2` is a collection of tools and libraries providing you with an easy-to-use environment for building, installing and running native Windows software.

MSYS2 allows you to build native Windows programs.

`Cygwin` (/ˈsɪɡwɪn/ SIG-win) is a free and open-source Unix-like environment and command-line interface (CLI) for Microsoft Windows. The project also provides a software repository containing open-source packages. Cygwin allows source code for Unix-like operating systems to be compiled and run on Windows

## Compiling

`> c17 -Wall -o getting-started getting-started.c -lm`

- `-o getting-started` tells it to store the compiler output in a file named `getting-started`.
- `getting-started.c` is the source file as input to the compiler
