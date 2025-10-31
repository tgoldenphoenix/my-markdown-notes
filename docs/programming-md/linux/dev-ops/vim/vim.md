# My definitive Vim notes

Let's learn Vim now!

Reading the vim doc: [tới đây](https://github.com/iggredible/Learn-Vim/blob/master/ch03_searching_files.md#searching-in-files-with-grep) <- regex

## Todo

tìm operator to toggle comments (maybe plug-ins)

auto-indent entire file

## Starting vim

**References**: [intro.txt](https://neovim.io/doc/user/intro.html#intro)

`vim --version` tells you the current Vim version and all available features marked with either `+` or `-`.

Open a file in vim: `vim filename.c` nếu file chưa tồn tại thì vim sẽ create a new file. Neovim command là `nvim`. You can also [open multiple files at once](https://github.com/iggredible/Learn-Vim/blob/master/ch01_starting_vim.md#opening-a-file)

Type `ctrl + z` to send vim to the _background_ and bring the terminal to the front. I like to think comic book sleeping, “Zzz," to remember the command. Once you’re ready to get back to work, type `fg` (foreground) in the terminal to bring up vim.\
You can also use `:sus`, `:suspend`, :`st`, or `:stop`, which all map to the same command.

**Source a file** in vim (for example `.vimrc`): `:w` then `:source %`

To **quit** vim: `:q[uit]` or `:q!` to force quit & dismiss any changes.\
Đang đọc `:h` mà muốn thoát luôn ra terminal không cần `:q` 2 lần thì dùng `:qa!`

**Write** changes to file (save file): `:write` or `:w`. If it is a new file, you need to give it a name before you can save it: `:w file.txt`\
write & quit: `:wq`

`:w {filename}` save the current Vim file as filename to disk (the current directory). (vimtutor lesson 5.2: MORE ON WRITING FILES)

If you want to open the file `hello.txt` and immediately execute a command, you can pass to the `vim` command the `+{cmd}` option. [read more](https://github.com/iggredible/Learn-Vim/blob/master/ch01_starting_vim.md#arguments)

You can launch Vim on split horizontal and vertical windows with the `-o` and `-O` options, respectively. [read more](https://github.com/iggredible/Learn-Vim/blob/master/ch01_starting_vim.md#opening-multiple-windows)

Also being a terminal command, you can combine `vim` with many other terminal commands. For example, you can redirect the output of the `ls` command to be edited in Vim with `ls -l | vim -`.

To learn more about `vim` command in the terminal, check out `man vim`.

## Operators

Keymap in Vim do not require all the button to be pressed at once.

Many command in vim have this "grammar": operator (d, y, c) + \[number] + motion. The order can change like `2dw` and `d2w` are the same. You should use `d2w` and `2w`.

- Undo `u` | redo `Ctrl r` or `<C-r>` (undo the undos of `u`)
- The capital `U` return the whole line to its original state. It undo all the changes on a line.

- Copy whole line: `yy` or `Y` (including the linebreak character at the end of each line)
- To yank everything from your current location to the end of the line: `y$`.

`yiw` yank in a word, copy word if you're in the middle of it

`y` yanks into register. Vim gọi là **yank & put** chứ không phải **copy & paste**.

`yt,` yank until `,`; use the `t{char}` motion

### Change & Delete

**Note**: Cần phân biệt các khái niệm delete vs change;  in vs around.

Both `d` and `c` save deleted text into register.

Press `x` to delete the character under the (block) cursor in normal mode.

`dw` - delete until the start of the next word, EXCLUDING its first character. `2dw` or `d2w` delete two words | `5d5w` 5 times delete 5 words. Note that the cursor must be placed at the beginning of the word when using `dw`.

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
To put back text that has just been deleted, type   p .  This puts the deleted text AFTER the cursor (if a line was deleted it will go on the line below the cursor).

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

- `g~` toggle case
- `g~iw` toggle case in word
- `gu{motion}` Make text lowercase (e.g., guw = "go lowercase word").
- `gU{motion}` Make text uppercase (e.g., gUw = "go uppercase word").
- `gUaw` convert current word to uppercase

---

Other prefixes

- `z` is the main prefix for all fold-related commands.

### Miscellaneous

`J` joins the current and next lines together

## Vim Motions

- Go to beginning of the line:
  - `0` Moves the cursor to the absolute start of the line (column 1).
  - `^` Moves the cursor to the first non-whitespace character on the line.
- Go to end of the line: `$`
- `g_` to go to the last non-blank character in the current line.

If you want to go to the column `n` in the current line, you can use `n|`.

### Word Navigation

- Go to the beginning of the next word: `w` or `W`:
  - `w` defines a "word" is a sequence of letters, digits, and underscores, OR a sequence of other non-blank characters (like punctuation), separated by whitespace, `:`, `-`, etc (configurable).
  - `W` defines a "WORD" is simply a sequence of any non-blank characters, separated **only by whitespace** (spaces, tabs, newlines).
- Use `w` for smaller, more fine-grain jumps, and `W` for bigger jumps across code separated only by spaces.

- Jump of the end of the current word: `e` & `E`

- Jump to the beginning of the **previous word**: `b` or `B` (works similar to `w` and `W`).
- Go to the end of the previous word `ge` or `gE`

So what are the similarities and differences between a word and a WORD? Both word and WORD are separated by blank characters. A word is a sequence of characters containing only `a-zA-Z0-9_` (alphanumeric + `_`). A WORD is a sequence of all characters except white space (a white space means either space, tab, and EOL). To learn more, check out :h word and :h WORD and [this article](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md)

### Sentence & Paragraph Navigation

Let's talk about what a sentence is first. A sentence ends with either `. ! ?` followed by an EOL, a space, or a tab. You can jump to the next sentence with `)` and the previous sentence with `(`.

- `{` Jump to the previous paragraph
- `}` Jump to the next paragraph

Match Navigation

- Select the openning curly bracket `{`, press `%` to jump to its closing bracket.
- `d%` delete till the closing bracket
- `dt(` delete everthing up until the opening bracket
- `yt{` copy everything up until the `{`

Line Number Navigation

- `gg`    Go to the first line of file
- `G`     Go to the last line of file
- `nG`    Go to line n. `:<line number>` do the same thing for example: `:1206`
- `n%`    Go to n% in file

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

## Registers & Macros

`:h registers` not `:h register`

`:register` or just `:reg` see list of registers in Vim

`"<register #>p` paste selected register
`"<register #>yy` yank the line into a selected register

`"+"` is a special register represent your computer's clipboard. You can `Cmd c` on a browser (copy into clipboard). Then in Vim insert mode type `"+p` to paste the clipboard into Vim file

Let's say you `yy` a line, then `dd` another line. Now you want to paste the line you yanked. Instead of typing `p` (deleting also copy in Vim), you type `"0p`. The `"0"` register stores the last thing that you yank, not by deleting & copy

`qa` to record a macro. `q` to stop recording
`@a` to execute the marcro `"a"`

To paste the text from register a ten times, do `10"ap`

**References:**

[DevOps Toolbox](https://www.youtube.com/watch?v=jSy8WjSyMAE)

## The Expression Register `=`

Most of Vim’s registers contain text either as a string of characters or as entire lines of text. The delete and yank commands allow us to set the contents of a register, while the put command allows us to get the contents of a register by inserting it into the document.

The **expression register** is different. It can evaluate a piece of Vim script code and return the result. Here, we can use it like a calculator. Passing it a simple arithmetic expression, such as 1+1, gives a result of 2. We can use the return value from the expression register just as though it were a piece of text saved in a plain old register.

## The Dot Command

> The dot command lets us repeat the last change.

A change could act at the level of individual characters, entire lines, or even the whole file.

`@:` can be used to repeat any Ex command

- Execute a sequence of changes: `qx{changes}q`
- repeat: `@x`
- reverse: `u`

Moving Around in Insert Mode Resets the Change: If we use the `<Up> , <Down> , <Left> , or <Right>` cursor keys while in Insert mode, a new undo chunk is created. It’s just as though we had switched back to Normal mode to move around with the `h, j, k, or l` commands, except that we don’t have to leave Insert mode. This also has implications on the operation of the dot command.

## Search in File & Substitution

Search is a form of Command Line mode. Depending on how we entered Command Line mode, we can browse our Ex Command or search history with the `<Down>` and `<Up>` keys.

- You can scan for next character with `f{char}` or `t{char}` (both are considered motions).
- `f` takes you to the first letter of the match and `t` takes you till (right before) the first letter of the match. So:
  - If you want to search for "h" and land on "h", use `fh`.
  - If you want to search for first "h" and land right before the match, use `th`.

- If you want to go to the **next occurrence** of the last `f` search, use `;`. 
- To go to the **previous occurrence** of the last current line match, use `,`.
- A good tip to go anywhere in a line is to look for least-common-letters like "j", "x", "z" near your target.

- `F` and `T` are the backward counterparts of `f` and `t`.
- To search backwards for "h", run `Fh`. To keep searching for "h" in the same direction, use `;`. Note that `;` after a `Fh` searches backward and `,` after `Fh` searches forward.

- Scan document for next/previous match: `/pattern<CR>` or `?pattern<CR>`
- Use `n` and `N` to repeat and reverse (jump to next and previous instance).

Khi đọc `help` của vim: To go back to where you came from press  `CTRL-o`  (Keep Ctrl down while pressing the letter o).  Repeat to go back further.  `CTRL-I` goes forward. Cái này giống `n` và `N` ở trên.

When the search reaches the end of the file it will continue at the start, unless the `wrapscan` option has been reset.

In normal mode, move the cursor to any **word** (not character) > press `*` to search forwards for the next occurrence of that word, or press `#` to search backwards. Sau đó dùng `n` and `N`.

- The `%` motion command jumps to the next matching bracket `(), [] or {}}`. You can jump from the open parentheses to the closing one and vice versa.
  * If your cursor is on an opening bracket like `(`, `{`, or `[`, pressing `%` jumps to the corresponding closing bracket.
  * If your cursor is on a closing bracket like `)`, `}`, or `]`, pressing `%` jumps back to the corresponding opening bracket.
  * It's useful for quickly navigating code blocks or checking if brackets are balanced.

### The `:substitute` command

The `:substitute` command in Vim (often shortened to `:s`) is used to find and replace text.

It is an Ex command.

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

## Different Modes in Vim

The default mode is **normal mode**. Pressing `Esc` to go back into normal mode.

Type `:` to enter **command mode**. You can use TAB completion in command mode.

`:!{command}` to execute an external shell command. NOTE: It is possible to execute any external command this way, also with arguments.

To insert (retrieve, read) the contents of a disk file in the current pwd and puts it below the cursor position, type  `:r FILENAME` (vimtutor Lesson 5.4: RETRIEVING AND MERGING FILES)

NOTE:  You can also read the output of an external command.  For example, `:r !ls`  reads the output of the ls command and puts it below the cursor.

`:e ~/.vimrc` switch to editing a new file.

- To enter insert mode:
  * `i` before the focus block or `I` to insert to beginning of the line (equal `^i`)
  * After the cursor (appending): `a` or `A` append to end of line (equal `$a`)
  * open a new line below the cursor: `o` | `O` (upper-case) open new line above (equal `ko`).

### Insert Mode

- `^h` delete back one character (equal backspace)
- `^w` delete back one word
- `^u` delete back to start of line

These commands are not unique to Insert mode or even to Vim. We can also use them in Vim’s command line as well as in the bash shell.

- `^[` switch back to Normal Mode (equal `<Esc>`)
- `^o` switch to Insert Normal Mode

Insert Normal Mode: when we’re in Insert mode and we want to run only one Normal command and then continue where we left off in Insert mode.

When the current line is right at the top or bottom of the window, I sometimes want to scroll the screen to see a bit more context. The `zz`command redraws the screen with the current line in the middle of the window, which allows us to read half a screen above and below the line we’re working on. I’ll often trigger this from Insert Normal mode by tapping out `<C-o>zz`. That puts me straight back into Insert mode so that I can continue typing uninterrupted.

From insert mode: `<C-r>{register}` paste text from `{register}`

---

The expression register is addressed by the `=` symbol. From Insert mode we can access it by typing `<C-r>=`. This opens a prompt at the bottom of the screen where we can type the expression that we want to evaluate. When done, we hit `<CR>`, and Vim inserts the result at our current position in the document

!!! Cái này hiện chưa làm được

---

insert unusual characters as **digraphs** (pairs of characters that are easy to remember).

From insert mode: `<C-k>{char1}{char2}`

- `<C-k>` then `>>` will type »
- `<C k>12` type ½
- `14`, `34`
- `:h digraphs-default`, `:h digraphs-table`

---

**Replace mode** is identical to Insert mode, except that it overwrites existing text in the document.

To replace the character under the cursor: `r`. For example: type  `rx`  to replace the character at the cursor with the character "x". You can replace "x" for other character.

A capical `R` will enters Replace mode until  `<ESC>`  is pressed. Replace mode is like Insert mode, but every typed character deletes an existing character.

### Visual Mode

**Visual mode** is used for selection for yank, deleting, change. . Press `v` to enter visual mode. Capital `Shift v` will enter visual line mode (select the current line).

- `viw` select current word. `viW` with a capital `W` select words with hyphen `-`
- `vi(` will select everything inside parentheses `()` and enter visual mode. `va"`
- `vit` select in tag (html tag like `<a href="#">one</a>` will select `one`)

- Delete selection: `d`.
- Change selection: `c`, delete & enter insert mode.
- Copy (yanking): `y`, including the cursor.

Paste after cursor block: `p`; `P` will paste before the cursor block.\
Note that using `p` after `yy` or `dd` will paste the new line below the current line and `P` will paste the new line above the current line.

`v  motion  :w FILENAME`  saves the Visually selected lines in file FILENAME (vimtutor Lesson 5.3: SELECTING TEXT TO WRITE)

`s` delete the character under the cursor and enter insert mode.

Each time we move our cursor in Visual mode, we change the bounds of the selection.

- Visual Mode in Vim has three sub-modes:
  * In **character-wise** Visual mode (`v`), we can select anything from a single character up to a range of characters within a line or spanning multiple lines. This is suitable for working at the level of individual words or phrases.
  * If we want to operate on entire lines, we can use **line-wise** Visual mode (`V` uppercase) instead.
  * Finally, **block-wise** Visual mode (`<C-v>`) allows us to work with columnar regions of the document.

`gv` Reselect the last visual selection  
The `gv`command is a useful little shortcut. It reselects the range of text that was last selected in Visual mode. No matter whether the previous selection was character-wise, line-wise, or block-wise, the `gv`command should do the right thing. The only case where it might get confused is if the last selection has since been deleted.

We can switch between the different flavors of Visual mode in the same way that we enable them from Normal mode. If we’re in character-wise Visual mode, we can switch to the line-wise variant by pressing `V`, or to block-wise Visual mode with `<C-v>`.

The range of a Visual mode selection is marked by **two ends**: one end is fixed and the other moves freely with our cursor. We can use the `o`key to toggle the free end.  
This is really handy if halfway through defining a selection we realize that we started in the wrong place. Rather than leaving Visual mode and starting afresh, we can just hit `o`and redefine the bounds of the selection.

---

When we use the dot command to repeat a change made to a visual selection, it repeats the change on the **same range of text**.

When we use the dot command to repeat a Visual mode command, it acts on the same amount of text as was marked by the most recent visual selection. This behavior tends to work in our favor when we make line-wise visual selections, but it can have surprising results with character-wise selections.

select visual mode > `U` operator to transform into upper-case

As a general rule, we should prefer operator commands (in normal mode) over their Visual mode equivalents when working through a repetitive set of changes.

Visual mode is perfectly adequate for one-off changes. And even though Vim’s motions allow for surgical preci-sion, sometimes we need to modify a range of text whose structure is difficult to trace. In these cases, Visual mode is the right tool for the job.

`Vr-` visual select entire current line, then change all character into "-"

### Operator-Pending Mode

We use it dozens of times daily, but it usually lasts for just a fraction of a second. For example, we invoke it when we run the command `dw`. It lasts during the brief interval between pressing `d` and `w`keys. Blink and you’ll miss it!

Operator-Pending mode is a state that accepts only motion commands. It is activated when we invoke an operator command, and then nothing happens until we provide a motion, which completes the operation. While Operator-Pending mode is active, we can return to Normal mode in the usual manner by pressing escape, which aborts the operation.

Many commands are invoked by two or more keystrokes (for examples, look up `:h g`,`:h z`, `:h ctrl-w`, or `:h [`), but in most cases, the first keystroke merely acts as a **prefix** for the second. These commands don’t initiate Operator-Pending mode. Instead, we can think of them as namespaces that expand the number of available command mappings. Only the operator commands initiate Operator-Pending mode.

Why, you might be wondering, is an entire mode dedicated to those brief moments between invoking operator and motion commands, whereas the namespaced com-mands are merely an extension of Normal mode? Good question! Because we can create custom mappings that initiate or target Operator-Pending mode. In other words, it allows us to create custom operators and motions, which in turn allows us to expand Vim’s vocabulary

## Vim settings

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

## Line wrapping

A **hard wrap** inserts actual line breaks in the text at wrap points, with soft wrapping the actual text is still on the same line but looks like it's divided into several lines.

## The help commands

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

## Terminology

[What is Ex command in Vim](https://www.cduan.com/technical/vi/vi-2.shtml). Ex command có dạng `:command` bắt đầu bằng ":". Ex command == command-line commands

What is **text objects** in vim??

## References

[Learn Vim (the Smart Way)](https://github.com/iggredible/Learn-Vim/tree/master). Có một trang same content nhưng ko phải trên Github

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/)

The [Wiki article](https://en.wikipedia.org/wiki/Vim_(text_editor)) about Vim is a very good introduction. It also has information about Neovim as well.

[vimgolf](https://www.vimgolf.com/) challenges

[vimbegood](https://github.com/ThePrimeagen/vim-be-good?tab=readme-ov-file) plugin by ThePrimegen

