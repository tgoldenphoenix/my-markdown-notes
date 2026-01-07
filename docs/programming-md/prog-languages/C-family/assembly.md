# Assembly Notes

## Terminologies

- `B`, byte
- `W`, word, 2 bytes
- `D`, double word, 4 bytes
- `Q`, quad word, 8 bytes ($2^3$)
- `T`, ten bytes
 
## Registers

`registers` is a special kind of memory built right into the CPU that is very small, but extremely fast to access

Instructions & data are stored in RAM and are loaded into the registers.

There are 16 general-purpose registers. In `x86-64`, each register is 64 bits wide (6 bytes), and for each of them the lower byte, word and double-word can be addressed individually (incidentally, 1 "word" = 2 bytes, 1 "double-word" = 4 bytes, in case you haven't heard this terminology before).

`rsp` holds the stack pointer (which is used by instructions like `push`, `pop`, `call` and `ret`

`rsi` and `rdi` serve as source and destination index for "string manipulation" instructions

Another example where certain registers get "special treatment" are the multiplication instructions, which require one of the multiplier values to be in the register `rax`, and write the result into the pair of registers `rax` and `rdx`.

`rip` holds the address of the next instruction to execute. It is modified by control flow instructions like `call` or `jmp`.

`rflags` holds a bunch of binary flags indicating various aspects of the program's state, such as whether the result of the last arithmetic operation was less, equal or greater than zero. The behavior of many instructions depends on those flags, and many instructions update certain flags as part of their execution. The flags register can also be read and written "wholesale" using special instructions.

### AX

Like most x86 registers, `RAX` (Accumulator Register) is backward-compatible. You can access smaller portions of the register using different names. This allows a 64-bit processor to run code designed for older 32-bit or 16-bit systems.

- Register Name,Size,Description:
- `RAX`, 64 bits, The full register (`Quad-word`, full 8 bytes).
  * the 'R' (register) prefix is used for all 64-bit general-purpose registers
- `EAX`, 32 bits, The lower 32 bits (`Double-word`, the 32 bits - 4 bytes - on the right side).
  * The "E" in EAX stands for "Extended," which was added when CPUs moved from 16-bit to 32-bit.
- `AX` (accumulator register), 16 bits, The lower 16 bits (`Word`, the 16 bits - 2 bytes - on the right side).
- `AL`, 8 bits, The lowest 8 bits (Byte) from 7-0.
- `AH`, 8 bits, Bits 8 through 15 (High Byte of `AX`).

### BX

`BX` = base register

- `rbx` full 8 bytes
- `ebx` right 4 bytes
- `bx`
- `bl`, `bh`

### CX

`CX` stands for the Count Register. Like the other registers we've discussed, it is 16 bits wide and has been extended into ECX (32-bit) and RCX (64-bit).

`rcx ecx cx ch cl`

### DI

`DI` = Destination Index register

`rdi`, `edi`, `di`, `dil`

Note: Unlike AX or BX, `DI` does not have a "high byte" version (there is no `DH` for DI).

## Instructions

The `INC` and `DEC` instructions increment or decrement values by one. Since the one is an implicit operand, the machine code for INC and DEC is smaller than for the equivalent `ADD` and `SUB` instructions.

```txt
inc ecx ; ecx++
dec dl ; dl--
```

## Directives

A directive is an artifact of the assembler not the CPU. They are gen-erally used to either instruct the assembler to do something or inform the assembler of something. They are not translated into machine code.

NASM code passes through a preprocessor just like C. It has many of the same preprocessor commands as C. However, NASM’s preprocessor directives start with a `%` instead of a # as in C.

The `equ` directive can be used to define a symbol. `Symbols` are named constants that can be used in the assembly program. The format is

`symbol equ value`

Symbol values can not be redefined later.

---

The `%define` directive is similar to C’s `#define` directive. It is most commonly used to define constant macros just as in C.

```txt
%define SIZE 100
mov eax, SIZE
```

The above code defines a macro named `SIZE` and shows its use in a `MOV` instruction. Macros are more flexible than symbols in two ways. Macros can be redefined and can be more than simple constant numbers.

---

`Data directives` are used in data segments to define room for memory. There are two ways memory can be reserved. The first way only defines room for data; the second way defines room and an initial value.

- The first method uses one of the `RESX` directives. The `X` is replaced with a letter that determines the size of the object (or objects) that will be stored.
- The second method (that defines an initial value, too) uses one of the `DX` directives. The `X` letters are the same as those in the RESX directives.

```txt
L1 db 0 ; byte labeled L1 with initial value 0
L2 dw 1000 ; word labeled L2 with initial value 1000
L7 resb 1 ; 1 uninitialized byte
L8 db "A" ; byte initialized to ASCII code for A (65)
```

Double quotes and single quotes are treated the same. Consecutive data definitions are stored sequentially in memory. That is, the word `L2` is stored immediately after `L1` in memory. Sequences of memory may also be defined.

```txt
L9 db 0, 1, 2, 3 ; defines 4 bytes
L10 db "w", "o", "r", ’d’, 0 ; defines a C string = "word"
L11 db ’word’, 0 ; same as L10
```

For large sequences, NASM’s `TIMES` directive is often useful. This direc-tive repeats its operand a specified number of times. For example,

```txt
L12 times 100 db 0 ; equivalent to 100 (db 0)’s
L13 resw 100 ; reserves room for 100 words
```

Remember that labels can be used to refer to data in code. There are two ways that a label can be used. If a plain label is used, it is interpreted as the address (or offset) of the data. If the label is placed inside square brackets (`[]`), it is interpreted as **the data at the address**. In other words, one should think of a label as a `pointer` to the data and the square brackets dereferences the pointer just as the asterisk does in C.

In 32-bit mode, addresses are 32-bit. Here are some examples:

```txt
mov al, [L1] ; copy byte at L1 into AL
mov eax, L1 ; EAX = address of byte at L1
mov al, [L6] ; copy first byte of double word at L6 into AL
```

Line 7 of the examples shows an important property of NASM. The assembler does not keep track of the type of data that a label refers to. It is up to the programmer to make sure that he (or she) uses a label correctly. Later it will be common to store addresses of data in registers and use the register like a pointer variable in C. Again, no checking is made that a pointer is used correctly. In this way, assembly is **much more error prone than even C**.

---

Consider the following instruction:

`mov [L6], 1 ; store a 1 at L6`

This statement produces an `operation size not specified error`. Why? Because the assembler does not know whether to store the 1 as a byte, word or double word. To fix this, add a `size specifier`:

`mov dword [L6], 1 ; store a 1 at L6`

## Memory and Addresses

k