# C Notes

## C jargon

statement: instruction, call a function

- `Identifiers` are “names” that we (or the C standard) give to certain entities in the program. Here we have `A`, `i`, `main`, `printf`, `size_t`, and `EXIT_SUCCESS`. Identifiers can be:
  - variables
  - Type: `size_t`. The trailing `_t` means that the identifier refers to a type.
  - function: `main`, `printf`
  - Constants, such as `EXIT_SUCCESS`.

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

## Data Types

`size_t` corresponds to “sizes,” so they are numbers that cannot be negative. Their range of possible values starts at 0 (natural numbers).

### Signed & Unsigned

C and C++ are unusual amongst languages nowadays in making a distinction between signed and unsigned integers.

An int is `signed` by default, meaning it can represent **both** positive and negative values. An `unsigned` is an integer that can never be negative.

If you take an `unsigned 0` and subtract `1` from it, the result wraps around (`arithmetic underflow`), leaving a very large number (2^32-1 with the typical 32-bit integer size).

You should use unsigned values whenever you are dealing with bit values, i.e. direct representations of the contents of memory; or when doing manipulations such as bit masking or shifting on data, for example when writing low-level code to read binary file formats such as audio files; or if you happen to be doing work such as embedded programming where type sizes and alignments really matter.

But stick to `signed` integers otherwise. You'll avoid a whole class of common problems.

## Expressing computations

In `a + b`, `+` is the operator; `a`, `b` are the operands.

- There are three types of operators:
  - The operators that operate on values:
    - Arithmetic operator: `+ - * /`
    - Remainder operation `%`
  - Those that operate on objects:
    - The assignment operator `=`
    - The increment & decrement operators `++i` & `--i`
  - Those that operate on types.

### Arithmetic Operators

The operators `+` and `-` have `unary variants`. `-b` gives the negative of `b`: a value a such that $b + a = 0$. `+a` simply provides the value of `a`.

- In C, a `well-defined operator` refers to an operator whose behavior is strictly governed by the C Standard, ensuring that the result is predictable across different compilers and hardware.
- When an operator is "well-defined," the compiler is not allowed to "guess" or optimize in a way that changes the outcome. This is the opposite of Undefined Behavior (like dividing by zero) or Unspecified Behavior (like the order of function arguments).

Unsigned arithmetic is always well defined.

The operations `+`, -, and * on size_t provide the mathematically correct result if it is representable as a `size_t` (is within the range `[0, SIZE_MAX]`).

When the result is not in that range and thus is not `representable` as a `size_t` value, we speak of `arithmetic overflow`. Overflow can happen, for example, if we multiply two values that are so large that their mathematical product is greater than `SIZE_MAX`. We’ll look at how C deals with overflow in the next section.

---

The operators `/` and `%` are a bit more complicated because they correspond to integer division and the remainder operation.

`a/b` evaluates to the number of times b fits into a, and `a%b` is the remaining value once the maximum number of `bs` are removed from `a`.

The operators `/` and `%` come in pairs: if we have `z = a / b`, the remainder `a%b` can be computed as $a - (z\cdot b)$.

For unsigned values, `a == (a/b)*b + (a%b)`.

There is only one value that is not allowed for these two operations: `0`. Division by zero is forbidden.

Unsigned `/` and `%` are only well defined if the second operand is not 0.

The `%` operator can also be used to explain additive and multiplicative arithmetic on unsigned types a bit better. As already mentioned, when an unsigned type is given a value outside its range, it is said to `overflow`. In that case, the result is reduced as if the `%` operator had been used. The resulting value “wraps around” the range of the type. In the case of `size_t`, the range is `0 to SIZE_MAX`.

This means for `size_t` values, `SIZE_MAX + 1` is equal to `0`, and `0 - 1` is equal to `SIZE_MAX`.

### Operators that modify objects

- In the expression `a = 42`
  - `a` is the objcet
  - `42` is the value
  - `=` is the assignment operator

- For the increment and decrement operators, there are two other forms: `postfix increment` and `postfix decrement`. They differ from the one we have seen in the result they provide to the surrounding expression.
  - The prefix versions of these operators (`++a` and `--a`) do the operation first and then return the result, much like the corresponding assignment operators (`a+=1` and `a-=1`);
  - the postfix operations return the value before the operation and perform the modification of the object thereafter.
- For any of them, the effect on the variable is the same: the incremented or decremented value.

### Boolean context

- zero `0` = `false`
- non-zero = `true`

Remember that `false` and `true` are nothing more than fancy names for 0 and 1, respectively. So, they can be used in arithmetic or for array indexing. In the following code, c will always be 1, and d will be 1 if a and b are equal and 0 otherwise:

```c
size_t c = (a < b) + (a == b) + (a > b);
size_t d = (a <= b) + (a >= b) - 1;
```

## Control flow

Functions are a way to transfer control unconditionally. The call transfers control unconditionally _to_ the function, and a `return` statement unconditionally transfers it _back_ to the caller.

### Conditional Execution `if`

```c
if (i > 25) {
  j = i - 25;
}
```

`i > 25` is called the `controlling expression`, and the part in `{ ... }` is called the `secondary block`.

The `if (...) ... else ...` is a `selection statement`. It selects one of the two possible code paths:

```c
if (condition) secondary-block0
else secondary-block1
```

- zero `0` = `false`
- non-zero = `true`

In `bool`, a `true` is `1`; while `false` is `0`. But it’s important to use `false` and `true` (and not the numbers) to emphasize that a value is to be interpreted as a condition.

In C, all scalars have a truth value. Here, `scalar` types include all the numerical types such as `size_t`, `bool`, `int`, `pointer`, etc…

### Iteration, loops

```c
// counts `i` down from `10` to `1`, inclusive
// when i becomes 0, it will evaluate to false, and the loop will stop
for (size_t i = 10; i; --i) {
  something(i);
}

for (size_t i = 0, stop = upper_bound(); i < stop; ++i) {
  something_else(i);
}

// counts down from `9` to `0`
// do not loop forever
for (size_t i = 9; i <= 9; --i) {
  something_else(i);
}
```

`i` is called the `loop variable`.

---

```c
while (condition) secondary-block

// do while
do secondary-block while(condition);
```

if the condition immediately evaluates to `false`, a `while` loop will not run its secondary block at all, but the `do` loop will unconditionally run its block at least once before ever looking at the condition.

`do` always needs a semicolon after its `while (condition)` to terminate the statement.

```c
for (;;) {
  double prod = a*x;
  if (fabs(1.0 - prod) < ε) {     // Stops if close enough
    break;
  }
  x *= (2.0 - prod);              // Heron approximation
}

// similar
while (true) {
  double prod = a*x;
  if (fabs(1.0 - prod) < ε) {       // Stops if close enough
    break;
  }
  x *= (2.0 - prod);                // Heron approximation
}
```

`for (;;)` here is equivalent to `while (true)`. The fact that the controlling expression of `for` (the middle part between the `;;`) can be omitted and is interpreted as “always **`true`**” is just a historical artifact in the rules of C and has no other special purpose.
