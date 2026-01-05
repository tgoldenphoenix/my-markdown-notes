# Assembly Notes

## Registers

`registers` is a special kind of memory built right into the CPU that is very small, but extremely fast to access

Instructions & data are stored in RAM and are loaded into the registers.

There are 16 general-purpose registers. In `x86-64`, each register is 64 bits wide (6 bytes), and for each of them the lower byte, word and double-word can be addressed individually (incidentally, 1 "word" = 2 bytes, 1 "double-word" = 4 bytes, in case you haven't heard this terminology before).

Like most x86 registers, `RAX` (Accumulator Register) is backward-compatible. You can access smaller portions of the register using different names. This allows a 64-bit processor to run code designed for older 32-bit or 16-bit systems.

- Register Name,Size,Description
- `RAX`, 64 bits, The full register (Quad-word).
- `EAX`, 32 bits, The lower 32 bits (Double-word, the 32 bits on the right side).
- `AX`, 16 bits, The lower 16 bits (Word, the 16 bits on the right side).
- `AL`, 8 bits, The lowest 8 bits (Byte) from 7-0.
- `AH`, 8 bits, Bits 8 through 15 (High Byte of `AX`).

`rsp` holds the stack pointer (which is used by instructions like `push`, `pop`, `call` and `ret`

`rsi` and `rdi` serve as source and destination index for "string manipulation" instructions

Another example where certain registers get "special treatment" are the multiplication instructions, which require one of the multiplier values to be in the register `rax`, and write the result into the pair of registers `rax` and `rdx`.

`rip` holds the address of the next instruction to execute. It is modified by control flow instructions like `call` or `jmp`.

`rflags` holds a bunch of binary flags indicating various aspects of the program's state, such as whether the result of the last arithmetic operation was less, equal or greater than zero. The behavior of many instructions depends on those flags, and many instructions update certain flags as part of their execution. The flags register can also be read and written "wholesale" using special instructions.

## Memory and Addresses


k