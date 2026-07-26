# Number Systems & Units

## Number System Notations

- $11_2=3_{10}$: `11` binary equals `3` in decimal
- $7_8$ (using subscript) means a `7` digit in base-8 number NOT the number seven in decimal ($7_{10}$)

- `0d10` = ten decimal
- `0x10` = ten hexadecimal = 16
- `0b0110` = binary

- In `C` programming language (and Java, Python, C++):
  - The prefix `0x` signify that the number is hexadecimal literal. Example: `0xFF = 255`.
  - `0` is the prefix for Octal. Example: `077 = 63`.

## Decimal

Use this online [Base Converter](https://www.rapidtables.com/convert/number/base-converter.html)

Decimal is called "base-10" because it uses ten digits (0–9).

Example: The number 253 means (2 × 100) + (5 × 10) + (3 × 1). Each position is a power of 10

- `1` means "on / yes / true"; a truthy value in programming languages
- `0` means "off / no / false"
- Linux exit code `0` is successful. Any non-zero exit code (from 1 to 255) signifies error => Hiểu `0` la2 "no error"

### DEC -> BIN

1. The subtraction method
2. Successive division

75 = 64 + 8 + 2 + 1 => `100 1011`

142 = 128 + 8 + 4 + 2 => `1000 1110`

339 = 256 + 64 + 16 + 2 + 1 => `1 0101 0011`

---

- 75 / 2 = 37 R 1 <- Least Significant Bit
- 37 / 2 = 18 R 1
- 18 / 2 = 9 R 0
- 9 / 2 = 4 R 1
- 4 / 2 = 2 R 0
- 2 / 2 = 1 R 0
- 1 / 2 = 0 R 1 <- Most Significant Bit
- => `75 = 100 1011` (write it from bottom -> top)

- 142/2 = 71 R 0
- 71/2 = 35 R 1
- 35/2 = 17 R1
- 17/2 = 8 R1
- 8/2 = 4
- 4/2 = 2
- 2/2 = 1
- 1/2 = 0 R1
- => `142 = 1001110`

### DEC -> HEX

k

## Binary

Binary is "base-2" because it only uses two digits (0 and 1).

Example: The number $1101_2$ means (1 × 8) + (1 × 4) + (0 × 2) + (1 × 1) = 13 in decimal. Each position is a power of 2.

`0000, 0001, 0010, 0011, 0100, 0101, 0110, 0111, 1000, ...`

- $2^0=1$
- $2^1=2$
- $2^2=4$
- $2^3=8$
- 2^4 = 16
- 2^5 = 32
- 2^6 = 64
- 2^7 = 128
- 2^8 = 256

4 bits max `1111 = 15` => Nhớ $2^4=16$ rồi trừ đi một là ra `15`.

1 byte max `1111 111 = 255`, `2^8 = 256`

`1 0000 + 1111 = 1 1111` or `16 + 15 = 31`

The rightmost digit of a binary number is called the `least-significant bit`, because it has the least value. The leftmost digit is called the `most-significant bit`, because it has the greatest value.

You should be able to convert **between** binary and decimal in your head, without writing down the value of each bit.

### Add & Subtract Binaries

k

### BIN -> HEX

- `1011 0111`
  - The first thing is you separate the binary in group of four, here we have two groups of four.
  - Convert to decimal: `1011 0111 = 11 & 7`
  - `11 & 7 = 0xB7`

- $1101.1011.0010.1111_2$
  - Split the number into four-bit groups: 1101, 1011, 0010, 1111
  - Convert each four-bit group into decimal: 13, 11, 2, 15.
  - Convert each decimal number into hexadecimal: D, B, 2, F.

Memorizing the decimal values of hexadecimal A–F is very helpful: 0d10 = 0xA, 0d11 = 0xB, 0d12 = 0xC, 0d13 = 0xD, 0d14 = 0xE, 0d15 = 0xF.

## Octal

Octal is base-8. Digits used: `0, 1, 2, 3, 4, 5, 6, 7`

One octal digit represents exactly three binary digits ($7 = 111$, $7 = 4+2+1$). This is because `2^3 = 8`.

- For example, in file permission:
- 3 octal digits represent $3 \times 3 = 9$ bits
- 4 octal digits represent $4 \times 3 = 12$ bits

Example: The number 37 means $(3 × 8) + (7 × 1) = 31$ in decimal.

`0 1 2 3 4 5 6 7 10 11 12 13 14 15 16 17 20, ...`

### Octet

In networking, an `octet` is a unit of size means "a group of 8 bits" (one byte). It is `0-255` in decimal, `00000000-11111111` in binary.

We use the word "octet" instead of "byte" because, historically, a "byte" could mean different sizes (like 6 or 7 bits) depending on the computer architecture. "Octet" is unambiguous.

The decimal range `0-255` represents all the possible values that can be stored in one 8-bit byte (256 different values).

### OCT -> BIN

- `056`
  - Separate the 5 & 6
  - `05 = 101`
  - `06 = 110`
  - `056 = 101 110`

### Linux File Permissions

The most common place you'll see the octal system today is in Linux/macOS file permissions. When you use the `chmod` command, you're often using octal numbers:

Three bits represent three boolean flags: `read-write-execute`.

Cách viết `rw-r--r--` thực chất là thể hiện 6 bits boolean flags chứ không phải 6 characters.

- `chmod 755`
  - `07 = 111` => Read + Write + Execute (`rwx`)
  - `05 = 101` => Read + Execute (`r-x`)
  - `05 = 101` => Read + Execute

- `06 = 110` => read, write (`rw-`)
- `04 = 100` => read (`r--`)

The octal number system is **NOT** used for IPv4 (it use decimal from `0-255` to represent 1 byte). One octal digit can only represents exactly three binary digits.

## Hexadecimal

Hexadecimal is base-16. Digits used: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A (10), B (11), C (12), D (13), E (14), F (15)`.  
Because more digits are available, hexadecimal can express large values in fewer characters.

Hexadecimal is a a very compact way to represent binary data. It's commonly used for colors (e.g., `#FF0000` for red), memory addresses, and **MAC addresses**.

- One "hex" digit = four binary digits because `2^4 = 16`. Example: `F = 1111 = 15`, 15 = 8 + 4 + 2 + 1)
- 2 hex digit = 8 bits (1 byte, an octet): `FF = 11111111 = 255`

Example: The number `F3` means (15 × 16) + (3 × 1) = 243 in decimal

`0 1 2 3 4 5 6 7 8 9 A B C D E F 10 11 12 13 14 15 16 17 18 19 1A 1B 1C, ...`

- $16^2=256=2^8$
- $16^3=4096$

| Decimal | Binary | Hexadecimal |
|---------|--------|-------------|
| 0       | 0000   | 0           |
| 1       | 0001   | 1           |
| 2       | 0010   | 2           |
| 3       | 0011   | 3           |
| 4       | 0100   | 4           |
| 5       | 0101   | 5           |
| 6       | 0110   | 6           |
| 7       | 0111   | 7           |
| 8       | 1000   | 8           |
| 9       | 1001   | 9           |
| 10      | 1010   | A           |
| 11      | 1011   | B           |
| 12      | 1100   | C           |
| 13      | 1101   | D           |
| 14      | 1110   | E           |
| 15      | 1111   | F           |

### HEX -> BIN

Base 16 & 2 are related while base 10 is not. The relationship is: `16 = 2^4`.

To convert `0x75` to decimal, just convert 7 to binary and 5 to binary and stick the results next to each other. This obviously doesn't work with converting to decimal, as 0x75 is 117, not 75. You have to do some multiplication by powers of 16 to get the correct decimal result.

Why is the process of converting hex to decimal completely different than hex to binary? Why does the 1st digit in 0x75 literally mean 0111 (7) in binary, but it doesn't literally mean 7 in decimal?

Base 16 is a power of 2, so you can group 4 bits to a hex, or vice versa. Base ten is not a power of two so conversion is not easy.

For example, if you had base 27 and base 3, the conversion is also straightforward because 27 is a power of 3.

This is exactly why Hexadecimal is a useful base in computer science.

Binary (base 2) and Hexadecimal (base 16) are both even powers of 2: `2^1` and `2^4` respectively. Decimal (base 10) is not an even power of 2 (`2^3.32193`).

---

- `A9`
  - `A = 10 = 8 + 2 = 1010`
  - `9 = 8 + 1 = 1001`
  - `A9 = 1010 1001 = 169`

- `3B7`
  - `3 = 0011`
  - `B = 1011`
  - `7 = 0111`
  - `3B7 = 0011 1011 0111`

### HEX -> DEC

- `23E = 2 x 16^2 + 3x16 + 14x1 = 574`
- `F7D = 15x16^2 + 7x16 + 13x1 = 3965`

## Bitwise Operations

These are logical operations performed on individual bits. The most important one in networking is bitwise `AND`.

Bitwise operators work on the individual bits of numbers, while logical operators work on true/false (boolean) values.

Logical operators are AND `&&`, OR `||`, and NOT `!`.

- Bitwise operator:
  - ``&`` AND
  - `|` OR
  - `~` NOT
  - `^` XOR
  - `<<` / `>>` Bitwise Shift

One byte can store 8 boolean value.

- The bitwise `AND`:
  - If both bits are 1, the resulting bit is 1.
  - If either bit is 0 (or both are 0), the resulting bit is 0.

The Truth Table for `AND`

| x | y | x & y |
|---|---|-------|
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

## Binary Arithmetic

k

## Binary, Octal, Decimal, Hexadecimal number system

Binary numbers only use the digits 0 and 1.

Octal Number System has a base of eight and uses the numbers from 0 to 7.

The sequence `1 2 4 10 20 40 100 200 400` corresponds to the powers of 2 in decimal (base-10).

- `1` (octal) = 1 (decimal) = $2^0$
- `2` (octal) = 2 (decimal) = $2^1$
- `4` (octal) = 4 (decimal) = $2^2$
- `10` (octal) = $1 \times 8^1 + 0 \times 8^0 = 8$ (decimal) = $2^3$
- `20` (octal) = $2 \times 8^1 + 0 \times 8^0 = 16$ (decimal) = $2^4$
- `40` (octal) = $4 \times 8^1 + 0 \times 8^0 = 32$ (decimal) = $2^5$
- `100` (octal) = $1 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 64$ (decimal) = $2^6$
- `200` (octal) = $2 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 128$ (decimal) = $2^7$
- `400` (octal) = $4 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 256$ (decimal) = $2^8$
