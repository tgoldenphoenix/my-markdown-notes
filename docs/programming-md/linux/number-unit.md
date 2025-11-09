# Number Systems & Units

## Number System Notations

`(7)~8` (using subscript) means digit `7` in base 8 number NOT the number seven in decimal `(7)~10`.

- Tương tự ta có:
  * `(11)~2 = (3)~10`: `11` binary equals `3` in decimal

## Decimal

Use this online [Base Converter](https://www.rapidtables.com/convert/number/base-converter.html)

Decimal is called "base-10" because it uses ten digits (0–9).

Example: The number 253 means (2 × 100) + (5 × 10) + (3 × 1). Each position is a power of 10

- `1` means "on / yes / true"; a truthy value in programming languages
- `0` means "off / no / false"
- Linux exit code `0` is successful. Any non-zero exit code (from 1 to 255) signifies error => Hiểu `0` la2 "no error"

## Binary

Binary is "base-2" because it only uses two digits (0 and 1).

Example: The number 1101 means (1 × 8) + (1 × 4) + (0 × 2) + (1 × 1) = 13 in decimal. Each position is a power of 2.

`0000, 0001, 0010, 0011, 0100, 0101, 0110, 0111, 1000, ...`

- 2^0 = 1
- 2^1 = 2
- 2^2 = 4
- 2^3 = 8
- 2^4 = 16
- 2^5 = 32
- 2^6 = 64
- 2^7 = 128
- 2^8 = 256

### Decimal => binary

1. The subtraction method
2. Successive division

75 = 64 + 8 + 2 + 1 => `1001011`

142 = 128 + 8 + 4 + 2 => `10001110`

339 = 256 + 64 + 16 + 2 + 1 => `101010011`

---

- 75 / 2 = 37 R 1 <- Least Significant Bit
- 37 / 2 = 18 R 1
- 18 / 2 = 9 R 0
- 9 / 2 = 4 R 1
- 4 / 2 = 2 R 0
- 2 / 2 = 1 R 0
- 1 / 2 = 0 R 1 <- Most Significant Bit
- => `75 = 1001011` (write it from bottom -> top)

- 142/2 = 71 R 0
- 71/2 = 35 R 1
- 35/2 = 17 R1
- 17/2 = 8 R1
- 8/2 = 4
- 4/2 = 2
- 2/2 = 1
- 1/2 = 0 R1
- => `142 = 1001110`

## Octal

Octal is base-8. Digits used: `0, 1, 2, 3, 4, 5, 6, 7`

One octal digit represents exactly three binary digits (`7 = 111`, 7 = 4+2+1).

Example: The number 37 means (3 × 8) + (7 × 1) = 31 in decimal.

`0 1 2 3 4 5 6 7 10 11 12 13 14 15 16 17 20, ...`

The most common place you'll see the octal system today is in Linux/macOS file permissions. When you use the `chmod` command, you're often using octal numbers:

- `chmod 755`
  - 7 (octal) = 111 (binary) = Read + Write + Execute
  - 5 (octal) = 101 (binary) = Read + Execute
  - 5 (octal) = 101 (binary) = Read + Execute

The octal number system is **NOT** used for IPv4 (it use decimal from `0-255` to represent 1 byte). One octal digit can only represents exactly three binary digits.

### Octet

In networking, an **octet** is a unit of size means "a group of 8 bits" (one byte). It is `0-255` in decimal & `00000000-11111111` in binary.

We use the word "octet" instead of "byte" because, historically, a "byte" could mean different sizes (like 6 or 7 bits) depending on the computer architecture. "Octet" is unambiguous.

The decimal range `0-255` represents all the possible values that can be stored in one 8-bit byte (256 different values).

## Hexadecimal

Hexadecimal is base-16. Digits used: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A (10), B (11), C (12), D (13), E (14), F (15)`

It's a very compact way to represent binary data. It's commonly used for colors (e.g., `#FF0000` for red), memory addresses, and MAC addresses

- One "hex" digit represents exactly four binary digits (`F = 1111`, 15 = 8 + 4 + 2 + 1)
- 2 hex digit represent 8 bits (1 byte, an octet): `FF = 11111111 = 255`

Example: The number F3 means (15 × 16) + (3 × 1) = 243 in decimal

`0 1 2 3 4 5 6 7 8 9 A B C D E F 10 11 12 13 14 15 16 17 18 19 1A 1B 1C, ...`

### Decimal -> Hexadecimal

k
