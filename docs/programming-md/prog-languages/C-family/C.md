# C Notes

## C jargon

statement: instruction, call a function

- `Identifiers` are “names” that we (or the C standard) give to certain entities in the program. Here we have `A`, `i`, `main`, `printf`, `size_t`, and `EXIT_SUCCESS`. Identifiers can be:
  * variables
  * Type: `size_t`. The trailing `_t` means that the identifier refers to a type.
  * function: `main`, `printf`
  * Constants, such as `EXIT_SUCCESS`.

Attributes Attributes such as `[[ maybe_unused ]]` are placed into double square brackets as shown and provide some supplemental information to the principle structure of the program. **NOTE**: This feature is new in C23, so your compiler might not yet implement it.

---

A `directive` in C is a special instruction, beginning with a hash symbol (`#`), that is processed by the C preprocessor before the source code is actually compiled. These are not part of the C language syntax itself but serve as commands for text manipulation and conditional processing of the source file.

Example: `#include`, `#define`, 

---

The most dangerous constructs in C are the so-called `casts`.

### Declarations

Before we may use a particular `identifier` in a program, we have to give the compiler a `declaration` that specifies what that identifier is supposed to represent. In this way, identifiers differ from `keywords`: keywords are predefined by the language and must not be declared or redefined.

All identifiers in a program have to be declared, either by the programmer or from other header `.h` files or `include files`.

```c
// 5 declarations in isolation
int main(int, char*[]);
int argc;
[[maybe_unused]] char* argv[];
double A[5];
size_t I;
```

the scope is a part of the program where an identifier is visible. Declarations are bound to the scope in which they appear.

Block scope: `main() {}`, `for () {}`

Function parameters are scoped inside the function

`file scope` = `globals`

### Definition

Generally, declarations only specify the kind of object an identifier refers to, not what the concrete value of an identifier is, nor where the object it refers to can be found. This important role is filled by a `definition`.

An `initialization` is a grammatical construct that augments a declaration and provides an initial value for the object.

`size_t i = 0;`

```c
double A[5] = {                                      
   [0] = 9.0,                                         
   [1] = 2.9,
   [4] = 3.E+25,                                      
   [3] = .00007,                                      
 };
```

This form of the initializing we see here is called `designated`. The last item of the array A is set to the value `3.E+25`. Any position that is not listed in the initializer is set to `0`. In our example, the missing `[2]` is filled with `0.0`

For an array with n elements, the first element has index 0, and the last has index `n-1`.

### Statements

`Statements` are instructions that tell the compiler what to do with identifiers that have been declared so far.

 There are three categories of statements: iterations (do something several times), function calls (delegate execution somewhere else), and function returns (resume execution from where a function was called).

 `for (size_t i = 0; i < 5; i++) {...}` is called `domain iteration` in C. The domain is $0, ..., 4$.

 The `loop variable` (`i`) should not be define outside of the for loop

 ---

C does NOT implement `pass by reference`; instead, it has another mechanism to pass the control of a variable to another function: by taking addresses and transmitting pointers.
 
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

- `-Wall` tells it to warn us about anything that it finds unusual.
- `-o getting-started` tells it to store the compiler output in a file named `getting-started`.
- `getting-started.c` is the source file as input to the compiler

`-Werror` (Warnings as Errors) the compiler treats every single warning as if it were a fatal error. The build will fail if even one warning is detected.  
A C program should compile cleanly without warnings.

A single `.c` can be ported and compile on different machine architecture. An assembly only work on a specific machine.
