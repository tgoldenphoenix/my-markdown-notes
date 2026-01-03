# Vim Notes

Reading the vim doc: [tới đây](https://github.com/iggredible/Learn-Vim/blob/master/ch03_searching_files.md#searching-in-files-with-grep) <- regex

## Todo

tìm operator to toggle comments (maybe plug-ins)

auto-indent entire file

## Starting `vim` & Basic Commands

`vim --version` tells you the current Vim version and all available features marked with either `+` or `-`.

Open a file in vim: `vim filename.c` nếu file chưa tồn tại thì vim sẽ create a new file. Neovim command là `nvim`. You can also open multiple files at once.

Type `ctrl + z` to send vim to the _background_ and bring the terminal to the front. I like to think comic book sleeping, “Zzz," to remember the command. Once you’re ready to get back to work, type `fg` (foreground) in the terminal to bring up vim.  
You can also use `:sus`, `:suspend`, `:st`, or `:stop`, which all map to the same command.

**Source a file** in vim (for example `.vimrc`) => `:w` > `:source %`.  
`%` expands to the full path and filename of the current file.

To quit vim: `:q[uit]` or `:q!` to force quit & dismiss any changes.  
Đang đọc `:h` mà muốn thoát luôn ra terminal không cần `:q` 2 lần thì dùng `:qa!`

Write changes to file (save file): `:write` or `:w`. If it is a new file, you need to give it a name before you can save it: `:w file.txt`  
`:w {filename}` save the current Vim file as filename to disk.

write & quit: `:wq`  
`:x` is also write & quit but it only writes (saves) the file if there have been changes. If there are no changes, it just quits without saving whereas `:wp` always writes (saves) the file, even if no changes were made, and then quits.

Also being a terminal command, you can combine `vim` with many other terminal commands. For example, you can redirect the output of the `ls` command to be edited in Vim with `ls -l | vim -`.

To learn more about `vim` command in the terminal, check out `man vim`.

## Operators

Keymap in Vim do not require all the button to be pressed at once.

Many command in vim have this "grammar":

> operator (d, y, c) + \[number] + motion.

The order can change. Both `2dw` and `d2w` achived the same result in slightly different steps.

- Undo `u`
- Undo the undos of `u` => `<C-r>`
- The capital `U` return the whole line to its original state. It undo all the changes on a line.

### Yank, Put, Change & Delete

- `y` yanks into register. Vim gọi là **yank & put** chứ không phải **copy & paste**.
- Cần phân biệt các khái niệm delete vs change;  in vs around.

- Copy whole line: `yy` (including the linebreak character at the end of each line)
- To yank everything from your current location to the end of the line: `y$` or `Y` uppercase.
- `yiw` yank in a word
- `yt,` yank until `,`; uses the `t{char}` motion.

- Lowercase `p` put word after the cursor (or below current line)
- Uppercase `P` put word before the cursor (or the live above)
- Vim also provides `gp` and `gP` commands. These also put the text before or after the current line, but they leave the cursor positioned at the end of the pasted text instead of at the beginning.

---

Both delete `d` and change `c` save deleted text into register.

- `r` change the character under the cursor in normal mode
- `x` deletes the character under the (block) cursor in normal mode. Không thể dùng với `.`.
- `s` delete the character under the cursor and enter insert mode. Có thể dùng với `.` để repeat.

`dw` - delete until the start of the next word, EXCLUDING its first character. `2dw` or `d2w` delete two words | `5d5w` 5 times delete 5 words. Note that the cursor must be placed at the beginning of the word when using `dw`.
anki

- `de` - to the end of the current word, INCLUDING the last character. Cái này hơi weird, nên dùng `dw`.
- `dd` deleta whole line
- `d$` - to the end of the line, INCLUDING the last character & the cursor. `d0` delete to beginning of line.
- `di"` to delete inside quote. If you are in the middle of a word: `diw` or delete-in-word.
- `daw` delete a word => không còn thừa white space
- `diw` delete in word => còn thừa white space
- `dap` delete a paragraph; use the `ap` motion

The change operator `c` requires a motion command immediately following it to specify what text to change. It deletes the text defined by the motion and then puts you into Insert mode.

- `cw` change word
- `ce` change until the end of a word.
- `ciw` Change in word
- `ci"` Change inside double quotes.  Example: `print("Tran Kim Phuong")`. You can also use it with square brackets `()` and curly brackets `{}`.
- `ca"` Change around quotes. Giống change in quote nhưng sẽ delete (cut) luôn 2 cái `""` quotes
- `C` (uppercase) == `c$`
- `s` == `cl` changes (deletes) the single character under the cursor and puts you into Insert mode. `l` alone move one character to the right.
- `S` (upper-case) == `cc` == `^C` (upper-case) change the entire current line.

Vim with **markup (like HTML tags)**

- `cit` change in tag like `<p>`, `<div>`.
- `dat` delete around tag, `dit` delete in tag

Delete whole line: `dd` (can be combine with number like `2dd`). `dd` also copy the deleted line. In other words, all "deleting" key bindings in Vim is actually cutting. So you can always use `p` to paste (put).  
Change the whole line: `cc`  
Delete the rest of the line from cursor block -> EOL: `D` (uppercase `d`) | `C` changes the rest of the line

`dd` + `p`, will put the deleted line to the line below the cursored line not after the cursor.
To put back text that has just been deleted, type `p`.  This puts the deleted text AFTER the cursor (if a line was deleted it will go on the line below the cursor).

### Indentation

- `>` shift right
- `<` shift left
- `>>` shift current line to the right
- `<<` shift current line to the left
- If you have selected the whole line in `visual mode`, you just press `>` once

automatically indent: `==` can also be used on selection of code
Auto-indent the whole file: `gg=G`

`>G` increases the indentation from the current line until the end of **the file**.

- `=` operator auto indent
- `V` select > `=`
- `gg=G` auto-indent the entire file

`!` operator Filter {motion} lines through an external program

To make the `<`and `>`commands work properly, we should set the `shiftwidth` and ‘softtabstop’ settings to 4 and enable `expandtab`.

`:set shiftwidth=4 softtabstop=4 expandtab`

### Simple Arithmetic

The `<C-a>` and `<C-x>` commands perform addition and subtraction on numbers. When run without a count they increment by one, but if we prefix a number, then we can add or subtract by any whole number.

For example, if we posi-tioned our cursor on a 5 character, running `10<C-a>` would modify it to read 15.

if the cursor is not already positioned on a number, then the `<C-a>` command will look ahead for a digit on the cur-rent line. If it finds one, it jumps straight to it.

Vim interprets numerals with a leading zero to be in octal notation rather than in decimal. In the octal numeric system, 007 + 001 = 010, which looks like the decimal ten but is actually an octal eight.

If you work with octal numbers frequently, Vim’s default behavior might suit you. If you don’t, you probably want to add the following line to your vimrc:

`set nrformats=`

This will cause Vim to treat all numerals as decimal, regardless of whether they are padded with zeros.

By itself, `g` does nothing. It's a "prefix" that waits for a second key to execute a less common or "extended" command.

### The `g` Prefix

- `~` in normal mode toggles the case of the character currently under the cursor & moves the cursor one step to the right.
- `~` in visual mode toggles the case of the selected area.
- `g~` toggle case case over a specific distance (motion) without selecting it first.
  - `g~iw` toggle case in word
  - `g~$` toggle case to end of line
- `gu{motion}` Make text lowercase (e.g., guw = "go lowercase word").
- `gU{motion}` Make text uppercase (e.g., gUw = "go uppercase word").
- `gUaw` convert current word to uppercase

`vU` uppercases the letter under the cursor.

---

Other prefixes

- `z` is the main prefix for all fold-related commands.

### Miscellaneous

`J` joins the current and next lines together

## Vim Motions

### Line-Wise motions

Unlike many text editors, Vim makes a distinction between **real lines** and **display lines**. When the `wrap` setting is enabled (and it’s on by default), each line of text that exceeds the width of the window will display as wrapped, ensuring that no text is truncated from view. As a result, a single line in the file may be represented by multiple lines on the display

- `j` down one real line
  - `gj` down one display line
- `k` & `gk` are similar
- `0` go to beginning of real line
  - `g0` go to beginning of display line
  
- Go to beginning of the line:
  - `0` goes to beginning of real line; moves the cursor to the absolute start of the line (column 1).
  - `^` Moves the cursor to the first non-whitespace character on the line.
  - `g^` To first nonblank character of display line
- Go to end of the line: `$`
- `g$` To end of display line
- `g_` to go to the last non-blank character in the current line.

`y^` yank to the start of the line.

If you want to go to the column `n` in the current line, you can use `n|`.

Note the pattern: `j, k, 0, and $` all interact with real lines, while prefixing any of these with `g` tells Vim to act on display lines instead.

If you would prefer to have the `j` and `k` keys operate on display lines rather than on
real lines, you can always remap them. Try putting these lines into your `vimrc` file

```bash
nnoremap k gk
nnoremap gk k
nnoremap j gj
nnoremap gj j
```

These mappings make `j` and `k`move down and up by display lines, while `gj`and `gk` would move down and up by real lines (the opposite of Vim’s default behavior). I wouldn’t recommend using these mappings if you have to work with Vim on many different machines. In that case, getting used to Vim’s default behavior would be better.

### Word-Wise Navigation

- Go to start of next word: `w` or `W`:
  - `w` defines a "word" is a sequence of letters, digits, and underscores, OR a sequence of other non-blank characters (like punctuation), separated by whitespace, `:`, `-`, etc (configurable).
  - `W` defines a "WORD" is simply a sequence of any non-blank characters, separated **only by whitespace** (spaces, tabs, newlines).
- Use `w` for smaller, more fine-grain jumps, and `W` for bigger jumps across code separated only by spaces.
- Backward to start of current / previous word: `b` or `B` (works similar to `w` and `W`).

- Forward to end of current / next word: `e` & `E`
- Backward to end of previous word `ge` or `gE`

- `ea` Append at the end of the current word
- `gea` append at the end of the previous word

So what are the similarities and differences between a word and a WORD? Both word and WORD are separated by blank characters. A word is a sequence of characters containing only `a-zA-Z0-9_` (alphanumeric + `_`). A WORD is a sequence of all characters except white space (a white space means either space, tab, and EOL). To learn more, check out :h word and :h WORD and [this article](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md)

### Sentence & Paragraph Navigation

Let's talk about what a sentence is first. A sentence ends with either `. ! ?` followed by an EOL, a space, or a tab. You can jump to the next sentence with `)` and the previous sentence with `(`.

- `{` Jump to the previous paragraph
- `}` Jump to the next paragraph
- `%` jump between matching parentheses

Match Navigation

- Select the openning curly bracket `{`, press `%` to jump to its closing bracket.
- `d%` delete to the closing bracket
- `dt(` delete everthing up until the opening bracket
- `yt{` copy everything up until the `{`

- Line Number Navigation
  - `gg`    Go to the first line of file
  - `G`     Go to the last line of file
  - `{number}G` Go to line number, use `<Ctrl-o>` to jump backward. `:<line number>` do the same thing for example: `:1206`
  - `n%`    Go to n% in file

`<Ctrl-o>` to jump back-ward giống như khi đọc `:h`. Use `<Ctrl-]>` to jump to link.

Type `CTRL-g` to show your location in the file and the file status.

- `H` - move cursor to top ("high up" or "home") of window
- `M` - move to middle of window
- `L` - move to bottom ("low" or "last line") of window
- You can also pass a count to `H` and `L`. If you use `10H`, you will go to 10 lines below the top of window. If you use `3L`, you will go to 3 lines above the last line of window.
- (These relate to the currently visible area of the window; not to the full buffer.)

Scrolling

- `CTRL+f` - move cursor Forward full page
- `CTRL+b` - move cursor Backwards full page
- `CTRL+u` - move cursor Up half page
- `CTRL+d` - move cursor Down half page

- `zt` - move screen so cursor is at Top
- `zb` - move screen so cursor is at Bottom
- `zz` - center the line where the cursor is on (very useful!)
- `ZZ` - save document and quit (be careful!)

### Vim's Grammar

> Operator + Motion = Action

- Motion is anything that moves the cursor
- Command are: `y`, `d`, `c`, `v`

Vim key bindings can be combined with number keys

- `5j`: go down 5 lines
- `15l`: jump 15 characters to the right
- `3u`: undo 3 times

Almost every key commands can be combine with numbers

Vim grammar is subset of Vim's **composability feature**. Composability means having a set of general commands that can be combined (composed) to perform more complex commands. The true power of Vim's composability shines when it integrates with external programs. Vim has a filter operator (!) to use external programs. Learn more [here](https://github.com/iggredible/Learn-Vim/blob/master/ch04_vim_grammar.md#composability-and-grammar)

Vim’s grammar has just one more rule: when an operator command is invoked in duplicate, it acts upon the current line. So `dd` deletes the current line, while `>>` indents it. The `gU`command is a special case. We can make it act upon the current line by running either `gUgU` or the shorthand `gUU`.

### Marking positions

Mark thì có local mark trong 1 buffer. Unlike local marks where you can have a set of marks in each buffer, you only get one set of global marks.

Mark dùng chung với registers

Mark important points in the file that you want to jump to without remembering the line number [Tutorial](https://vim.fandom.com/wiki/Using_marks)

Learn about jump commands in Vim [here](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md#jump)

### Text Objects

Text objects allow us to interact with parentheses, quotes, XML tags, and other common patterns that appear in text.

`vit`, `vat` => vim understand XML tag (or HTML tag) `<xml>tags</xml>`

 the text objects prefixed with `i`select inside the delimiters, whereas those that are prefixed with `a` select everything including the delimiters. As a mnemonic, think of `i` as inside and `a` as around (or all).

- `iw` current word
- `aw` current word plus one space
- `iW` current WORD
- `aW` current WORD plus one space
- `is` current sentence
- `as`current sentence + one space
- `ip` current paragraph
- `ap` current paragraph plus one blank line

The `iw` text object interacts with everything from the first to the last character of the current word. The `aw`text object does the same, but it extends the range to include a whitespace character after or before the word, if one is present.

- To delete a word, `diw` left us with two adjacent spaces => use `daw`
- To change a word, use `ciw`

As a general rule, we could say that the `d{motion}` command tends to work well with `aw`, `as`, and `ap`, whereas the c{motion} command works better with `iw` and similar.

### Marks

Vim’s `marks` allow us to jump quickly to locations of interest within a document. We can set marks manually, but Vim also keeps track of certain points of interest for us automatically.

In Vim, marks and registers are two entirely separate storage systems that serve different purposes.  
While they both use a single character (like a or B) as a label, they do not share the same memory space or data.

The `m{a-zA-Z}` command marks the current cursor location with the designated letter.  
Lowercase marks are local to each individual buffer, whereas uppercase marks are **globally** accessible.

Vim does nothing to indicate that a mark has been set, but if you’ve done it right, then you should be able to jump directly to your mark with only two keystrokes from anywhere in the file.

---

- Vim provides two Normal mode commands for **jumping to a mark**. (Pay attention—they look similar!):
  - `’{mark}` (using a single quote) moves to the line where a mark was set, positioning the cursor on the first non-whitespace character.
  - The `` \`{mark} `` (using backtick) command moves the cursor to the exact position where a mark was set, restoring the line and the column at once.
- If you commit only one of these commands to memory, go with [\`{mark}]. Whether you care about restoring the exact position or just getting to the right line, this command will get you there. The only time you have to use the `’{mark}` form is in the context of an Ex command.

The `mm` and [\`m] commands make a handy pair. Respectively, they set the mark `m` and jump to it.

---

- The marks that Vim sets for us automatically can be really handy:
  - [``] Position before the last jump within current file (giống `<C-o>`)
  - `. Location of last change
  - `^ Location of last insertion
  - `[ Start of last change or yank
  - `] End of last change or yank
  - `< Start of last visual selection
  - `> End of last visual selection

### Jump Between Matching Parentheses

Vim provides a motion that lets us move between opening and closing pairs of parentheses.  
By enabling the `matchit.vim` plugin, we can extend this behavior to work on pairs of XML tags as well as on keywords in some programming languages.

- The `%` motion command jumps between opening & closing parentheses `(), [] or {}}`. You can jump from the open parentheses to the closing one and vice versa.
  - If your cursor is on an opening bracket like `(`, `{`, or `[`, pressing `%` jumps to the corresponding closing bracket.
  - If your cursor is on a closing bracket like `)`, `}`, or `]`, pressing `%` jumps back to the corresponding opening bracket.
  - It's useful for quickly navigating code blocks or checking if brackets are balanced.

When we use the `%` command, Vim automatically sets a mark for the location from which we jumped. We can snap back to it by pressing [``] (press the backtick two times).

---

Plugin `Surround.vim`

- `S"` Surround the selection with a pair of double quote marks. We could just as easily use `S)` or `S}` if we wanted to wrap the selection with opening and closing parentheses or braces.

We can also use `surround.vim` to change existing delimiters. For example, we could change `{London}` to `[London]` with the `cs}]` command, which can be read as “Change surrounding `{}` braces to `[]` brackets.” Or we could go the other way with the `cs]}` command.

## Search in File & Substitution

Search is a form of Command Line mode. Depending on how we entered Command Line mode, we can browse our Ex Command or search history with the `<Down>` and `<Up>` keys.

- You can scan for **character** within the **current line** with `f{char}` or `t{char}` (both are considered motions).
- `f` position the cursor on top of the specified character whereas `t` takes you **till** (right before) the first letter of the match. So:
  - If you want to search for "h" and land on "h", use `fh`.
  - If you want to search for first "h" and land right before the match, use `th`.
- The search start with the cursor position and continuing to the end of the **current line**.

- If you want to go to the next occurrence of the last `f` search, use `;`.
- To go to the previous occurrence of the last current line match, use `,`.
- A good tip to go anywhere in a line is to look for least-common-letters like `j, x, z`, capital letters, punctuation near your target.

- Uppercase `F` and `T` are the backward counterparts of `f` and `t`.
- To search backwards for "h", run `Fh`. To keep searching for "h" in the same direction, use `;`. Note that `;` after a `Fh` searches backward and `,` after `Fh` searches forward.

- `dt.` deletes all of the text until the end of the sentence, but not including the period symbol itself.
- `dt{` delete till `{`

- Scan document for next/previous match: `/pattern<CR>` or `?pattern<CR>`
- Use `n` and `N` to repeat and reverse (jump to next and previous instance).

- `;` & `,` khác `n` & `N` ở chỗ:
  - `; ,` jump đến hết **current line** là stop.
  - `n N` jump entire file & loop lại từ đầu nếu đã nhảy đến cuối file.

- The search command `/{text}` can be used while in visual mode & operator-pending mode.
- `d/ge<CR>` => The search command is an exclusive motion. That means that even though our cursor ends up on the “g” at the start of the word “gets,” that character is excluded from the delete operation

Khi đọc `help` của vim: To go back to where you came from press  `CTRL-o`  (Keep Ctrl down while pressing the letter o).  Repeat to go back further.  `CTRL-I` goes forward. Cái này giống `n` và `N` ở trên.

When the search reaches the end of the file it will continue at the start, unless the `wrapscan` option has been reset.

In normal mode, move the cursor to any **word** (not character) > press `*` to search forwards for the next occurrence of that word, or press `#` to search backwards. Sau đó dùng `n` and `N`.

### The `:substitute` command

The `:substitute` Ex command in Vim (often shortened to `:s`) is used to find and replace text.

`:s/target/replacement`

we can repeat the last `:substitute` command (which itself happens to be an Ex command as well) by pressing `&`.

Use `u` to reverse substitution.

`:%s/content/copy/g` change all "content" to "copy"

- `:%` Specifies the **range** as "all lines in the current file". It's equivalent to the range `1,$` (from line 1 to the last line).
- `g` The global flag, meaning replace all occurrences on each line, not just the first one.

`:s/thee/the` substitute 'the' for 'thee'. Note that this command only changes the _first occurrence_ of "thee" in the cursored line.  
Type  `:s/thee/the/g`. Adding the  g  flag means to substitute globally in _the line_, change all occurrences of "thee" in the line.

To change every occurrence of a character string between two lines, type   `:#,#s/old/new/g` where #,# are the line numbers of the range of lines where the substitution is to be done.
Type  `:%s/old/new/g`       to change every occurrence in the whole file.
Type   `:%s/old/new/gc`    to find every occurrence in the whole file, with a prompt whether to substitute or not.

## Registers

Vim’s registers are simply containers that hold text. They can be used in the manner of a clipboard for cutting, copying, and pasting text, or they can be used to record a macro by saving a sequence of keystrokes.

In Vim’s terminology, we don’t deal with a clipboard but instead with registers.

`:h registers` not `:h register`

`:register` or just `:reg` see list of registers in Vim

- `:reg a` to inspect content of register `a`
  - type c: Characterwise
  - type l: linewise; Pastes on a new line above or below the cursor.
  - type b: blockwise

`"<register #>p` paste selected register
`"<register #>yy` yank the line into a selected register

`"+"` is a special register represent your computer's clipboard. You can `Cmd c` on a browser (copy into clipboard). Then in Vim insert mode type `"+p` to paste the clipboard into Vim file

Let's say you `yy` a line, then `dd` another line. Now you want to paste the line you yanked. Instead of typing `p` (deleting also copy in Vim), you type `"0p`. The `"0"` register stores the last thing that you yank, not by deleting & copy

To paste the text from register a ten times, do `10"ap`

---

The Named Registers (`"a-"z`)

We can specify which register we want to use by prefixing the command with `"{register}`. If we don’t specify a register, then Vim will use the unnamed register.

Vim has one named register for each letter of the alphabet (see `:h quote_alpha`)

When we address a named register with a lowercase letter, it overwrites the specified register, whereas when we use an uppercase letter, it appends to the specified register.

- `"ayiw`  yank the current word into register `a`
- `"bdd` cut the current line into register b
- `"ap`paste the word from register `a`
- `"bp`paste the line from register `b`

- In insert mode, `<C-r>{register}` to paste text from register (không dùng `"{register}p`). Cái này giống `<C-v>` bình thường. Trong neovim có plugin hiện register content.
- `Ctrl-r` in normal mode is redo (undo an undo).
- `<C-r>"` insert the contents of the unnamed register
- The backtick \` and single quote `'` is used to jumps to a mark

---

The unnamed register `""`

If we don’t specify which register we want to interact with, then Vim will use the unnamed register, which is addressed by the `"` symbol (see :h quote_quote).  
To address this register explicitly, we have to use two double quote marks: for example, `""p`, which is effectively equivalent to `p` by itself.

`x` cuts the character under the cursor, placing a copy of it in the **unnamed register**. Then the `p` command pastes the contents of the unnamed register after the cursor position.  
Taken together, the `xp` commands can be considered as “Transpose/swap the next two characters.”

We can just as easily transpose the order of two lines of text. `dd` cuts the current line, placing it into the unnamed register. `p` pastes the contents of the unnamed register after the current line.  
The `ddp` sequence could be considered to stand for “Transpose the order of this line and its successor.”

`diw` cuts a word & copy it into unnamed register.

---

The Yank Register `"0`

When we use the `y{motion}` command, the specified text is copied not only into the unnamed register but also into the yank register, which is addressed by the `0` symbol.

As the name suggests, the yank register is set only when we use the `y{motion}` command. To put it another way: it’s not set by the `x`, `s`, `c{motion}`, and `d{motion}` commands.

`"0P` paste from the yank register

`:reg "0` inspect yank & unnamed register

---

The Black Hole Register `"_`

You might be wondering what Vim’s equivalent is for really deleting text—that is, how can you remove text from the document and not copy it into any registers? Vim’s answer is a special register called the black hole, from which nothing returns. The **black hole register** is addressed by the `_` symbol (see `:h quote_`), so `"_d{motion}` performs a true deletion (instead of "cut").

This can be useful if we want to delete text without overwriting the contents of the unnamed register.

---

The System Clipboard (`"+`) and Selection (`"*`) Registers

All of the registers that we’ve discussed so far are internal to Vim. If we want to copy some text from inside of Vim and paste it into an **external program** (or vice versa), then we have to use one of the `system clipboards`.

Vim’s plus register `"+` references the system clipboard and is addressed by the `+` symbol (see `:h quote+`)

If we use the cut or copy command to capture text in an external application, then we can paste it inside Vim using `"+p` command (or `<C-r>+` from the Insert mode). Conversely, if we prefix Vim’s yank or delete commands with `"+`, the specified text will be captured in the system clipboard. That means we can easily paste it inside other applications

In Windows and Mac OS X, there is no primary clipboard, so we can use the `"+` and `"*` registers interchangeably: they both represent the system clipboard.

---

The Expression Register `"=`

Vim’s registers can be thought of simply as containers that hold a block of text. The expression register, referenced by the `=` symbol (`:h quote=`), is an exception. When we fetch the contents of the expression register, Vim drops into Command-Line mode, showing an `=` prompt. We can enter a Vim script expression and then press `<CR>` to execute it. If the expression returns a string (or a value that can be easily coerced into a string), then Vim uses it.

---

More Registers

Vim provides a handful of registers whose values are set implicitly. These are known collectively as the read-only registers

- `"%`Name of the current file
- `"#` Name of the alternate file
- `".` Last inserted text
- `":` Last Ex command
- `"/` Last search pattern

---

When we use the `p` command in Visual mode, Vim replaces the selection with the contents of the specified register (see `:h v_p`)

We can get away with using the unnamed register for both the yank and put operations because there’s no delete step. Instead, we combine the delete and put operations into a single step that replaces the selection.

## Macros

We’ve already learned about the dot command, which is useful for repeating small changes. But when we want to repeat anything more substantial, we should reach for Vim’s macros.

The `q` key functions both as the “record” button and the “stop” button. To begin recording our keystrokes, we type `q{register}`, giving the address of the register where we want to save the macro.  
We can tell that we’ve done it right if the word “recording” appears in the status line.  
Every command that we execute will be captured, right up until we press `q` again to stop recording.

We can inspect the contents of register a by typing the following: `:reg a`. When reading in this format, the symbol `^[` is used to stand for the Escape key. Còn Spacebar thì sẽ hiện là spacebar không có escape gì cả.

---

The `@{register}` command executes the contents of the specified register (see `:h @`). We can also use `@@`, which repeats the macro that was invoked most recently.

- `qa` to record a macro. `q` to stop recording
- `@a` to execute the marcro `"a"`

---

Vim’s motions can fail. For example, if our cursor is positioned on the first line of a file, the `k` command does nothing. The same goes for `j` when our cursor is on the last line of a file. By default, Vim beeps at us when a motion fails.

**If a motion fails while a macro is executing, then Vim aborts the rest of the macro**. Consider this a feature, not a bug.  
We can use motions as a simple test of whether or not the macro should be executed in the current context.

Suppose that the macro was stored in the a register. Rather than executing `@a` ten times, we could prefix it with a count: `10@a`.  
The beauty of this technique is that we can be unscrupulous about how many times we execute this macro. Don’t care for counting? It doesn’t matter! We could execute `100@a` or even `1000@a`, and it would produce the same result. Bởi vì khi một motion fail thì entire macro bị aborted.

---

The Dot Formula can be an efficient editing strategy for a small number of repeats, but it can’t be executed with a count. Overcome this limitation by recording a cheap one-off macro and playing it back with a count

- `qq;.q` record the marcro `;.` into the `q` register. Now we can execute the macro with a count: `11@q`. This executes `;.` eleven times.
- The `;` command repeats the `f+` search. When our cursor is positioned after the last `+` character on the line, the `;` motion fails and the macro aborts.
- Nếu dùng `11;.` thì nó sẽ là `11;` rồi `.` one single time

In our case, we want to execute the macro ten times. But if we were to play it back eleven times, the final execution would abort. In other words, we can complete the task so long as we invoke the macro with a count of ten or more.  
Who wants to sit there and count the exact number of times that a macro should be executed? Not me. I’d rather provide a count that I reckon to be high enough to get the job done. I often use 22, because I’m lazy and it’s easy to type. On my keyboard, the `@` and `2` characters are entered with the same button

---

We can make light work out of repeating the same set of changes on a range of lines by recording a macro and then playing it back on each line. There are two ways to do this: executing the macro in series or in parallel.

record your macro on one line > visual select other lines > `:'<,'>normal @a`  
The `:normal @a` command tells Vim to execute the macro once for each line in the selection. This execute the macro on each line **parallel** independent from the others; If an iteration fails, it does so in isolation. Whereas `22@a` **executes them in series**; If one iteration fail, the rest fail.

- Execute in series, if the motion fail on one line, the remaining macros to be executed is aborted.
- Execute in parallel, the `:normal@a` command tells Vim to execute the macro once for each line in the selection.

---

Sometimes we miss a vital step when we record a macro. There’s no need to re-record the whole thing from scratch. Instead, we can **append extra commands onto the end of an existing macro**.

Use `:reg a` to inspect the contents of register `a`.

If we type `qa`, then Vim will record our keystrokes, saving them into register `a` by overwriting the existing contents of that register. If we type `qA`, then Vim will record our keystrokes, appending them to the existing contents of register a.

This little trick saves us from having to re-record the entire macro from scratch. But we can use it only to tack commands on at the end of a macro. If we wanted to add something at the beginning or somewhere in the middle of a macro, this technique would be of no use to us. In Tip 71, on page 176, we’ll learn about a more powerful method for amending a macro after it has been recorded.

---

Being able to insert a value that changes for each execution of a macro can be useful. In this tip, we’ll learn a technique for incrementing a number as we record a macro so that we can insert the numbers 1 to 5 on consecutive lines.

For this solution, we’ll use the expression register with a touch of Vim script.

The `:echo` command is fine for revealing the value that is assigned to a variable, but ideally we want to insert that value into the document. We can do that using the expression register. In Tip 16, on page 31, we saw that the expression register can be used to do simple sums and to insert the result into the document. We can insert the value stored in variable i just by running `<C-r>=i<CR>` in Insert mode.

```text
:let i=1
qa
I<C-r>=i<CR>)<spacebar>
<Esc>
:let i += 1
q
```

Before we begin recording the macro, we set the variable i to 1. Inside the macro, we use the expression register to insert the value stored in i. Then, before we finish recording the macro, we increment the value stored in the variable, which should now contain the value 2.

We can still visual select > `:'<,'>normal @a` to execute the macro parallel, the numbers still increase by one for each line normally.

---

In Tip 68, on page 168, we saw that adding commands at the end of a macro is straightforward. But what if we want to remove the last command? Or change something at the beginning of the macro? In this tip, we’ll learn how to **edit the content of a macro just as if it were plain text**.

When you run `:reg a` to inspect a macro:

- the `^[` symbol represents the Escape key. No matter whether you press `<Esc>` or `<C-[>`.
- the `<80>kb` symbol represents the backspace key.

- `:put a`paste the contents of register `a` into a new line (paste the macro out to the document file)
- Now we can edit the macro as plain text.
- Yank the Macro from the Document Back into a Registe:
  - `"add` (or `:d a`)
  - `"ay$` > `dd`: yank every character on that line except for the carriage return

The `dd` command performs a line-wise deletion. The register contains a trailing `^J` character:

```text
➾ :reg a
❮ 0f.r)wvUj^J`
```

Having followed these steps, register a now contains a new and improved macro. We can use it on the example text that we met at the start of this tip.

Why didn’t we just use the `"ap` command? In this context, the `p` command would paste the contents of the a register after the cursor position on the current line. The :put command, on the other hand, always pastes below the current line, whether the specified register contains a line-wise or a character-wise set of text

## The Dot Command

> The dot command lets us repeat the last change.

A change could act at the level of individual characters, entire lines, or even the whole file.

`@:` can be used to repeat any Ex command

- Execute a sequence of changes: `qx{changes}q`
- repeat: `@x`
- reverse: `u`

Moving Around in Insert Mode Resets the Change: If we use the `<Up> , <Down> , <Left> , or <Right>` cursor keys while in Insert mode, a new undo chunk is created. It’s just as though we had switched back to Normal mode to move around with the `h, j, k, or l` commands, except that we don’t have to leave Insert mode. This also has implications on the operation of the dot command.

## Different Modes in Vim

### Normal Mode

The default mode is **normal mode**. Pressing `Esc` to go back into normal mode.

Type `:` to enter **command mode**. You can use TAB completion in command mode.

`:!{command}` to execute an external shell command. NOTE: It is possible to execute any external command this way, also with arguments.

To insert (retrieve, read) the contents of a disk file in the current pwd and puts it below the cursor position, type  `:r FILENAME` (vimtutor Lesson 5.4: RETRIEVING AND MERGING FILES)

NOTE:  You can also read the output of an external command.  For example, `:r !ls`  reads the output of the ls command and puts it below the cursor.

`:e ~/.vimrc` switch to editing a new file.

### Insert Mode

- To enter insert mode:
  - `i` before the focus block or `I` to insert to beginning of the line (equal `^i`)
  - After the cursor (appending): `a` or `A` append to end of line (equal `$a`)
  - open a new line below the cursor: `o`; uppercase `O` will open new line above (equal `ko`).
  - `gi` go to the last place you entered Insert mode, and enter Insert mode again

- `^h` delete back one character (equal backspace)
- `^w` delete back one word
- `^u` delete back to start of line

These commands are not unique to Insert mode or even to Vim. We can also use them in Vim’s command line as well as in the bash shell.

- `^[` switch back to Normal Mode (equal `<Esc>`)
- `^o` switch to Insert Normal Mode

Insert Normal Mode: when we’re in Insert mode and we want to run only one Normal command and then continue where we left off in Insert mode.

When the current line is right at the top or bottom of the window, I sometimes want to scroll the screen to see a bit more context. The `zz`command redraws the screen with the current line in the middle of the window, which allows us to read half a screen above and below the line we’re working on. I’ll often trigger this from Insert Normal mode by tapping out `<C-o>zz`. That puts me straight back into Insert mode so that I can continue typing uninterrupted.

From insert mode: `<C-r>{register}` paste text from `{register}`. Can also be used in command-line mode.

---

**Replace mode** is identical to Insert mode, except that it overwrites existing text in the document.

To replace the character under the cursor: `r`. For example: type  `rx`  to replace the character at the cursor with the character "x". You can replace "x" for other character.

A capical `R` will enters Replace mode until  `<ESC>`  is pressed. Replace mode is like Insert mode, but every typed character deletes an existing character.

`x` delete the character under the cursor and store it in unname register by default.

---

The **expression register** is addressed by the `=` symbol. From Insert mode we can access it by typing `<C-r>=`. This opens a prompt at the bottom of the screen where we can type the expression that we want to evaluate. When done, we hit `<CR>`, and Vim inserts the result at our current position in the document

!!! Cái này hiện chưa làm được

---

insert unusual characters as **digraphs** (pairs of characters that are easy to remember).

From insert mode: `<C-k>{char1}{char2}`

- `<C-k>` then `>>` will type »
- `<C k>12` type ½
- `14`, `34`
- `:h digraphs-default`, `:h digraphs-table`

### Visual Mode

**Visual mode** is used for selection for yank, deleting, change. . Press `v` to enter visual mode. Capital `Shift v` will enter visual line mode (select the current line).

- `viw` select current word. `viW` with a capital `W` select words with hyphen `-`
- `vi(` will select everything inside parentheses `()` and enter visual mode. Similar: `va"`
- `vit` select visual in tag (html tag like `<a href="#">one</a>` will select `one`)

- Delete selection: `d`
- Change selection: `c`
- Copy (yanking): `y`

- Paste after cursor block: `p`; pressing an uppercase `P` will paste the selected text before cursor block.
- Note that using `p` after `yy` or `dd` will paste the new line below the current line and `P` will paste the new line above the current line.

`v  motion  :w FILENAME`  saves the Visually selected lines in file FILENAME

Each time we move our cursor in Visual mode, we change the bounds of the selection.

- Visual Mode in Vim has three sub-modes:
  1. In `character-wise` Visual mode (`v`), we can select anything from a single character up to a range of characters within a line or spanning multiple lines. This is suitable for working at the level of individual words or phrases.
  2. If we want to operate on entire lines, we can use `line-wise` Visual mode (`V` uppercase) instead.
  3. Finally, `block-wise` Visual mode (`<C-v>`) allows us to work with columnar regions of the document.
- We can switch between the different flavors of Visual mode in the same way that we enable them from Normal mode. If we’re in character-wise Visual mode, we can switch to the line-wise variant by pressing `V`, or to block-wise Visual mode with `<C-v>`.

---

The `I` and `A` commands placing the cursor at the start or end of the selection, respectively. So what about the `i` and `a`commands; what do they do in Visual mode?

In Visual and Operator-Pending modes, the `i` and `a` keys follow a different convention: they form the first half of a text object.

In line-wise or block-wise visual mode, `I` & `A` will allow you to have multi-corsor insertion.

---

The range of a Visual mode selection is marked by **two ends**: one end is fixed and the other moves freely with our cursor. We can use `o` to toggle the free end.  
This is really handy if halfway through defining a selection we realize that we started in the wrong place. Rather than leaving Visual mode and starting afresh, we can just hit `o`and redefine the bounds of the selection.

---

`gv` reselects the last visual selection. It reselects the range of text that was last selected in Visual mode. No matter whether the previous selection was character-wise, line-wise, or block-wise, the `gv`command should do the right thing. The only case where it might get confused is if the last selection has since been deleted.

---

When we use the dot command to repeat a change made to a visual selection, it repeats the change on the **same range of text**.

When we use the dot command to repeat a Visual mode command, it acts on the same amount of text as was marked by the most recent visual selection. This behavior tends to work in our favor when we make line-wise visual selections, but it can have surprising results with character-wise selections.

select visual mode > `U` operator to transform into upper-case

As a general rule, we should prefer operator commands (in normal mode) over their Visual mode equivalents when working through a repetitive set of changes.

Visual mode is perfectly adequate for one-off changes. And even though Vim’s motions allow for surgical preci-sion, sometimes we need to modify a range of text whose structure is difficult to trace. In these cases, Visual mode is the right tool for the job.

`Vr-` visual select entire current line, then change all character into "-"

### Command-Line Mode

> In the beginning, there was `ed`. `ed` begat `ex`, and `ex` begat `vi`, and `vi` begat `Vim`.

Command-Line mode prompts us to enter: an Ex command, a search pattern, or an expression.

`vi` traces its ancestry back to a line editor called ex, which is why we have **Ex commands**. For some line-oriented tasks, Ex commands are still the best tool for the job.

When we press the `:` key, Vim switches into `Command-Line mode`. Command-Line mode is also enabled when we press `/` to bring up a search prompt or `<C-r>=` to access the expression register.

- `:[range]delete [x]` Delete specified lines into register `[x]`
- `:[range]yank [x]` Yank specified lines into register `[x]`
- `:[line]put [x]` Put the text from register `[x]` after the specified line
- `:[range]copy {address}` Copy the specified lines to below the line specified by `{address}`
- `:[range]move {address}` Move the specified lines to below the line specified by `{address}`
- `:[range]join` Join the specified lines
- `:[range]normal {commands}`: Execute Normal mode `{commands}` on each specified line
- `:[range]substitute/{pat-tern}/{string}/[flags]` Replace occurrences of {pattern} with {string} on each specified line
- `:[range]global/{pattern}/[cmd]` Execute the Ex command `[cmd]` on all specified lines where the `{pattern}` matches

We can use Ex commands to read and write files (`:edit` and `:write`), to create tabs (`:tabnew`) or split windows (`:split`), or to interact with the argument list (`:prev/:next`) or the buffer list (:bprev/:bnext). In fact, Vim has an Ex command for just about everything

The `:normal` command provides a convenient way to make the same change on a range of lines

`Ctrl-r{register}` also work in command-line mode like in insert mode

Tab-completion in command-line mode: use `tab` and `S-tab` or `^p` & `^n` to cycle through the options. Use `^-y` to accept the option

---

Many Ex commands can be given a `[range]` of lines to act upon. We can specify the start and end of a range with either a line number, a mark, or a pattern.  
`:{start},{end}`: both the `{start}` and `{end}` are addresses. We can use line numbers, patter or a mark for addresses.  
When we specify a `[range]`, it always represents a set of contiguous lines. It’s also possible to execute an Ex command on a set of noncontiguous lines using the :global command.

The `:print` or `:p` command doesn’t perform any useful work, but it helps to illustrate which lines make up a range.

- `:{line-number}` jump to `{line-number}`
- `:1` jumps to first line of the file
- `:$` jump to last line of the file (giống `G`). In normal mode, `$` jumps to end of line.
- `:0` refers to the virtual line above first line of the file
- `:.` Line where the cursor is placed
- `:'m` Line containing mark m
- `:'<` Start of visual selection
  * `:'>` End of visual selection
  * => `:'<,'>`
- `:%` The entire file, all the lines in the current file (shorthand for `:1,$`)

Line 0 doesn’t really exist, but it can be useful as an address in certain con-texts. In particular, it can be used as the final argument in the `:copy {address}` and :move {address} commands when we want to copy or move a range of lines to the top of a file.

`:3d` jump to line 3 & delete it. Normal mode equivalent: `3G dd`.

`:2,5p` prints each line from 2 to 5, inclusive. Note that after running this command, the cursor would be left positioned on line 5.

`:.,$p` print from current line to end of file.

The command `:%p` is equivalent to running `:1,$p` (print all). Using this shorthand `:%` in combination with the `:substitute` command is very common:

`:%s/Practical/Pragmatic/` => This command tells Vim to replace the first occurrence of “Practical” with “Pragmatic” on each line.

---

You can **specify a range of lines for ex-command using visual mode**: enter visual-line mode, select, then press `:` key, the command-line prompt will be prepopulated with the range `:'<,'>`. It looks cryptic, but you can think of it simply as a range standing for the visual selection. Then we can specify our Ex command, and it will execute on every selected line.

`:'<,'>s/const/let` => substitute `const` by `let` in the visually selected range of lines

---

`:/<html>/,/<\/html>/p` => This looks quite complex, but it follows the usual form for a range: :{start},{end}. The {start} address in this case is the pattern `/<html>/`, while the `{end}` address is `/<\/html>/`. In other words, the range begins on the line containing an opening `<html>` tag and ends on the line containing the corresponding closing tag.

Suppose that we wanted to run an Ex command on every line inside the <html></html> block but not on the lines that contain the `<html>` and </html> tags themselves. We could do so using an offset:

`:/<html>/+1,/<\/html>/-1p`

The general form for an offset goes like this: `:{address}+n`. If n is omitted, it defaults to 1. The {address} could be a line number, a mark, or a pattern.

```bash
:2
:.,.+3p 
```

The `.` symbol stands for the current line, so `:.,.+3` is equivalent to :2,5 in this case

---

The `:copy` command (and its shorthand `:t`) lets us duplicate one or more lines from one part of the document to another, while the :move command lets us place them somewhere else in the document.

The format of the copy command goes like this: `:[range]copy {address}`  
We could shorten the :copy command to only two letters, as `:co`. Or we can be even more succinct by using the `:t` command, which is a synonym for :copy. As a mnemonic, you can think of it as copy TO.

- `:6copy.` => “Make a copy of line 6 and put it below the current line.” The `[range]` was line 6. For our {address}, we used the `.` symbol, which stands for the current line.
- `:t6` Copy the current line to just below line 6
- `:t.`Duplicate the current line (similar to Normal mode `yyp`). `yyp` uses a register, whereas `:t.` doesn’t. I’ll sometimes use :t. to duplicate a line when I don’t want to overwrite the current value in the default register.
- `:t$` Copy the current line to the end of the file
- `:'<,'>t0` Copy the visually selected lines to the start of the file

In this example `:6copy.`, we could have used a variant of yyp to duplicate the line we wanted, but it would require some extra moving around. We would have to jump to the line we wanted to copy (`6G`), yank it (`yy`), snap back to where we started (`<C-o>`), and use the put command (`p`) to duplicate the line. When duplicating a distant line, the `:t` command is usually more efficient.

---

The `:move` command looks similar to the :copy command: `:[range]move {address}`. We can shorten it to a single letter: `:m`.

Dùng visual-line mode select range of lines, press `:'<,'>m$` to move to the end of file.

Remember that the '<,'> range stands for the visual selection. We could easily make another visual selection and then repeat the :'<,'>m$ command to move the selected text to the end of the file. Repeating the last Ex command is as easy as pressing `@:`

---

If we want to **run a Normal-mode command on a series of consecutive lines**, we can do so using the `:normal` command. When used in combination with the dot command or a macro, we can perform repetitive tasks with very little effort

Use visual-line mode to select range of line > `:'<,'>normal .` => For each line in the visual selection, execute the Normal mode `.` command.

`:%normal A;` append a `;` to the end of every line of the file (`%`).

Before executing the specified Normal mode command on each line, Vim moves the cursor to the beginning of the line. So we don’t have to worry about where the cursor is positioned when we execute the command. This single command could be used to comment out an entire JavaScript file:

`:%normal i//`

While it’s possible to use :normal with any normal command, I find it most powerful when used in combination with one of Vim’s repeat commands: either :normal . for simple repeats or `:normal @q` for more complex tasks.

---

While the `.` command can be used to repeat our most recent Normal mode
command, we have to use `@:` instead if we want to repeat the last Ex command. Knowing how to reverse the last command is always useful, so we’ll consider that, too, in our discussion.

---

Tab-Complete Your Ex Commands

`:col<C-d>` => The `<C-d>` command asks Vim to reveal a list of possible completions.  
If we hit the `<Tab>` key, the prompt will cycle through the suggestion. We can scroll backward through the suggestions by pressing `<S-Tab>`.

Suppose we want to change the color scheme, but we can’t remember the name of the theme we want. We could use the `<C-d>` command to show all the options:

`:colorscheme <C-d>`

This time, <C-d> shows a list of suggestions based on the color schemes that are available. If we wanted to enable the solarized theme, we could just type the letters “so” and then hit the `Tab` key to complete our command.

---

Even in Command-Line mode, Vim always knows where the cursor is positioned and which split window is active. To save time, we can insert the current word (or WORD) from the active document onto our command prompt.

At Vim’s command line, the `<C-r><C-w>` mapping copies the word under the cursor and inserts it at the command-line prompt. We can use this to save ourselves a bit of typing.

While <C-r><C-w> gets the word under the cursor, we can instead use `<C-r><C-a>` if we want to get the WORD.

---

Press `:`, then press `<Up>` & `<Down>` (or, preferably, `<C-p>` & `<C-n>`) arrow key to cycle through command history.

But there’s a disadvantage to the <C-p> and <C-n> commands: unlike <Up> and <Down> , they don’t filter the command history. We can get the best of both by creating the following custom mappings:

```bash
cnoremap <C-p> <Up>
cnoremap <C-n> <Down>
```

Now try typing `:help`, followed by the <Up> key. Again, this should scroll through previous Ex commands, but instead of showing everything, the list will be filtered to only include Ex commands that started with the word “help".

Press `/` to search, press `<Up>` to cycle through search history.

---

The command-line window is like a regular Vim buffer, where each line con-tains an item from our history. This allows us to change histor-ical commands using the full modal editing power of Vim (normal & insert mode).

Press `q:`to bring up the command-line window. Close it by running `:q` (just like any ordinary Vim window) or by pressing `<CR>`. When we press the `<CR>` key, the contents of the current line are executed as an Ex command.

- `q/` Open the command-line window with history of searches
- `q:` Open the command-line window with history of Ex commands
- In Command-Line mode, we can use the `<C-f>` mapping to switch to the command-line window

- merge two records from our history into one

---

Run external shell commands in Vim.

From Vim’s Command-Line mode, we can invoke external programs in the shell by prefixing them with a bang symbol (`!`).

`:!ls` view the contents of the current directory

`:ls` calls Vim’s built-in command, which shows the contents of the buffer list.

On Vim’s command line, the `%` symbol is shorthand for the current file name. We can exploit this to run external commands that do something with the current file.

The `:!{cmd}` syntax is great for firing one-off commands, but what if we want to run several commands in the shell? In that case, we can use Vim’s `:shell` command to start an interactive shell session. The `exit` command kills the shell and returns us to Vim.

Pressing `Ctrl-z`suspends the process that’s running Vim and
returns control to bash. The Vim process sits idle in the background, allowing us to interact with our bash session as normal.

In bash, we can use the `fg` command to resume a suspended job, bringing it back into the foreground. That brings Vim back to life exactly as we left it. The `Ctrl-z` and `fg` commands are quicker and easier to use than Vim’s equivalent :shell and `exit` commands.

### Operator-Pending Mode

We use it dozens of times daily, but it usually lasts for just a fraction of a second. For example, we invoke it when we run the command `dw`. It lasts during the brief interval between pressing `d` and `w`keys. Blink and you’ll miss it!

Operator-Pending mode is a state that accepts only motion commands. It is activated when we invoke an operator command, and then nothing happens until we provide a motion, which completes the operation. While Operator-Pending mode is active, we can return to Normal mode in the usual manner by pressing escape, which aborts the operation.

Many commands are invoked by two or more keystrokes (for examples, look up `:h g`,`:h z`, `:h ctrl-w`, or `:h [`), but in most cases, the first keystroke merely acts as a **prefix** for the second. These commands don’t initiate Operator-Pending mode. Instead, we can think of them as namespaces that expand the number of available command mappings. Only the operator commands initiate Operator-Pending mode.

Why, you might be wondering, is an entire mode dedicated to those brief moments between invoking operator and motion commands, whereas the namespaced com-mands are merely an extension of Normal mode? Good question! Because we can create custom mappings that initiate or target Operator-Pending mode. In other words, it allows us to create custom operators and motions, which in turn allows us to expand Vim’s vocabulary

## Working with Files in Vim

Open Files and Save Them to Disk

The `:edit` command allows us to open files from within Vim, either by specifying an absolute or a relative filepath.

- `buffer`: The actual text/file loaded in memory.
- `window`: A "viewport" that looks into a buffer. You can have multiple windows looking at the same buffer.
- `tab`: A collection or layout of windows. It is more like a "workspace" than a browser tab.

## Vim Settings

Vim settings are stored in `~/.vimrc` file

show vim version: `:version`

`ic` 'ignorecase' ignore upper/lower case when searching. Prepend `no` or `inv` to switch an option off:   `:set noic` or `:set invic`
NOTE:  If you want to ignore case for just one search command, use  `\c` in the phrase:  `/ignore\c <ENTER>`

`is` 'incsearch' **incremental search**, show partial matches for a search phrase

Boolean settings can be toggled by appending a trailing bang `!` symbol after the setting. For example, we can toggle the `'wrap'` setting with `:set wrap!`. If we want to know the current value of a setting, we could append a trailing question mark `?` after the setting. For example, calling `:set wrap?` displays `wrap` if the `'wrap'` setting is on and `nowrap` if the '`wrap`' setting is off.

To show the current value of a setting and where that setting was last set, use the `:verbose` command. Running `:verbose set wrap?` should show you something like `Last set from ~/.vimrc line 123` on the last line in the lower left-corner of the window. The `:verbose` command is particularly helpful when debugging.

Finally, if we append a trailing `&` symbol to a setting, it will reset the specified setting to its default value. For example, `:set wrap&` resets the `'wrap'` setting to its default value of `'wrap'`.

If you are trying to get Vim to auto wrap the contents of the current buffer at 80 characters, try setting `'textwidth'` to 80, like this `:set textwidth=80`. Then return to Normal mode with `<Esc>` and press `gggqG`. Press the keys in sequence: `g``g` `g``q` `G`. In summary, `gg` jumps to the start of the file, `gq` invokes the format operator, and `G` is a motion that takes us to the last line in the current buffer.

Section 4 of [this article](https://vimandgit.com/posts/vim/beginners/neovim-and-vim-install-mac-linux-set-up-and-configure-vim.html) explain the `tabstop` and `expandtab` settings in combination

Vim stores its settings in the `~/.vimrc` configuration file. Neovim stores its settings in the `~/.config/nvim/init.vim` configuration file. You can use your existing `.vimrc` settings with Neovim: Just add `source ~/.vimrc` to `~/.config/nvim/init.vim`. This tells Neovim to source your `~/.vimrc` on startup. There are a few Vim features that won't work in Neovim. These features were intentionally removed from Neovim and they are [listed here](https://neovim.io/doc/user/vim_diff.html "Vim features that are removed from Neovim").

Vim assigns a function to almost every key on the keyboard. If we want to create our own custom mappings, which keys should we bind them to? Vim provides the `<Leader>` key as a namespace for our own user-defined commands.

## Line wrapping

A **hard wrap** inserts actual line breaks in the text at wrap points, with soft wrapping the actual text is still on the same line but looks like it's divided into several lines.

## The `:help` commands

Type `K` on any word to find its documentation. Instead of typing `:h <keyword>`

In Kickstart, MOST IMPORTANTLY, we provide a keymap `<space>sh` to [s]earch the [h]elp documentation, which is very useful when you're not exactly sure of what you're looking for. Just pressing `space` give you some suggestions.

If you experience any errors while trying to install kickstart, run `:checkhealth` for more info.

`:help user-manual` after completing vimtutor\
In nvim, run `:Tutor` or run `vimtutor` out in the terminal (2 cái hơi khác nhau).

`man vim` in the terminal

[How to use the help command](https://vimandgit.com/posts/vim/beginners/the-neovim-and-vim-help-command.html)\\
`:h[elp]`

Press `CTRL-]` to jump to a subject under the cursor.\
Press `CTRL-0` to jump back (repeat to go further back). To go back to the previous screen (pop tag), press `<C-t>` — press `CTRL` and `t` at the same time. Read more [here](https://neovim.io/doc/user/index.html#bars)

Type  `CTRL-W CTRL-W` to jump from one window to another. Lúc mở `:h` lên nó sẽ có 2 cái window. Dùng command này để nhảy.

If you want to know more about say how pressing `<C-w>c` when the help window is active — press `CTRL` and `w` at the same time, and then press `c` - will close the window. You can type `:h ^w` and search from there.

Here's one extra tip to use the help manual: suppose you want to learn more about what `Ctrl-P` does in insert mode. If you merely search for `:h CTRL-P`, you will be directed to normal mode's Ctrl-P. This is not the Ctrl-P help that you're looking for. In this case, search instead for `:h i_CTRL-P`. The appended `i_` represents the insert mode. Pay attention to which mode it belongs to.

If you ever need to look up something ("I wish Vim can do this `{keyword}`"), just type `:h {keyword}`, then `^d` or `TAB` to displayed relevant keywords to choose from. Command line completion with `CTRL-D` and `<TAB>` works for many commands.  Just try pressing CTRL-D and <TAB>.  It is especially useful for  :help .

Vim commands can be abbreviated. For example, `:join` can be abbreviated as `:j`. In general, whenever you spot a new command, always check it on `:help` to see its abbreviations.

Whenever a passage mentions a `.vimrc` option, just add that option into vimrc, save it (`:w`), then source it (`:source %`).

It is **recommended** to `:help user-manual` read through chapters 1-12 and 20-32

Just like in the terminal, we can autocomplete commands on Vim's command line. We can type in the first few letters of a command and press `<Tab>` or `<C-n>` to cycle forward through the list of possible suggestions, and `<S-Tab>` or `<C-p>` to cycle backward. Sometimes we may want to see the full list of possible suggestions on the screen, we can do this with `<C-d>`.

To know more about the help command, see `:help helphelp`.

The section 2 of this [article](https://vimandgit.com/posts/vim/beginners/the-neovim-and-vim-help-command.html) describe how to use a Mode Prefix to Identify the Mode for the Help Command.

[how to read :help fullscreen](https://vi.stackexchange.com/questions/358/how-to-full-screen-browse-vim-help)

Read `:h user-manual`. If it opens in a split, `:only` will make the user manual window the only one on the screen. If you haven't started vim, you can jump right to the manual with `vim -c 'h user-manual|only'`.

Read `:h notation` and `:h key-notation`

The E tags in vim help [read here](https://vi.stackexchange.com/questions/31114/what-are-the-e-tags-in-vim-help)
[difference between marks and tags in Vim?](https://vi.stackexchange.com/questions/16870/difference-between-marks-and-tags)

## References

[Learn Vim (the Smart Way)](https://github.com/iggredible/Learn-Vim/tree/master). Có một trang same content nhưng ko phải trên Github

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/)

The [Wiki article](https://en.wikipedia.org/wiki/Vim_(text_editor)) about Vim is a very good introduction. It also has information about Neovim as well.

[vimgolf](https://www.vimgolf.com/) challenges

[vimbegood](https://github.com/ThePrimeagen/vim-be-good?tab=readme-ov-file) plugin by ThePrimegen
