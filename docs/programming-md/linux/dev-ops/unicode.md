# Unicode Notes

Unicode is a single, large set of characters including all presently used scripts of the world. Unicode comes with two main encodings, UTF-8 and UTF-16.

**ASCII** is limited to 128 characters, while Unicode includes every character in every writing system worldwide.

In internationalization, **CJK characters** is a collective term for graphemes used in the Chinese, Japanese, and Korean writing systems, which each include Chinese characters. It can also go by CJKV to include Chữ Nôm, the Chinese-origin logographic script formerly used for the Vietnamese language, or CJKVZ to also include Sawndip, used to write the Zhuang languages.

- Characters are arranged by script and function in blocks
- Smallest blocks: Kanbun, Katakana Phonetic Extensions,... (16 characters each)
- Largest block: CJK Unified Ideographs Extension B, from U+20000 to U+2A6DF (42720 characters)
- Maybe more than one block for a script (example: 0600..06FF Arabic; 0750..077F Arabic Supplement)

The original version of Unicode was designed as a 16-bit encoding, which limited the support to 65,536 (2^16) code points. Version 2.0 of the Unicode Standard increased the range from `U+0000..U+10FFFF.` These ranges are grouped in **planes** - 65,536 (2^16) code points per plane. Planes are subdivided into Unicode **blocks**. Each Unicode block is a contiguous range of code points that share a common purpose, such as supporting a single script.

- Current sturcture: 1+16=17 planes:
  * BMP (base multilingual plane, plane 0, modern-use)
  * Plane 1, SMP (supplementary multilingual plane, historic scripts,...)
  * Plane 2, SIP (Supplementary Ideographic Plane, really rare ideographs)
  * Planes 3-13: currently unused
  * Plane 14, SSP (Supplementary Special-purpose Plane, tags, variant selectors,...)
  * Planes 15 and 16: Private Use

The Unicode Standard specifies that the complete range of Unicode code points can be converted to unique code unit sequences using one of seven **Unicode encoding schemes** or **Unicode Transformation Formats (UTF)**. This table compares the Unicode encoding schemes.

A BOM (Byte Order Mark) is a special sequence of bytes at the beginning of a text file, and one of its primary functions is to tell a program what encoding to use when reading the file.

## Unicode Characters, Code Points, and Graphemes

What a regex engine sees as a character and what this tutorial means by a character is more accurately called a Unicode code point. What most people see as a character is more accurately called a Unicode grapheme.

All Unicode regex engines discussed in this tutorial treat any single Unicode code point as a single character. When this tutorial tells you that the dot matches any single character, this translates into Unicode parlance as “the dot matches any single Unicode code point”. In Unicode, `à` can be encoded as two code points: U+0061 (a) followed by U+0300 (grave accent). In this situation, `.` applied to `à` will match `a` without the accent. `^.$` will fail to match, since the string consists of two code points. `^..$` matches `à`.

The Unicode code point U+0300 (grave accent) is a **combining mark**. Any code point that is not a combining mark can be followed by any number of combining marks. This sequence, like U+0061 U+0300 above, is displayed as a single grapheme on the screen.

Unfortunately, à can also be encoded with the single Unicode code point U+00E0 (a with grave accent). The reason for this duality is that many historical character sets encode “a with grave accent” as a single character. 