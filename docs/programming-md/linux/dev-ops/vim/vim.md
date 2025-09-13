# My definitive Vim notes

Let's learn Vim now!

Reading the vim doc: [tới đây](https://github.com/iggredible/Learn-Vim/blob/master/ch03_searching_files.md#searching-in-files-with-grep) <- regex

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

## Key bindings, keymap (operator)

Keymap in Vim do not require all the button to be pressed at once.

**Vim Grammar:**
Many command in vim are: operator (d, y, c) + \[number] + motion. The order can change like `2dw` and `d2w` are the same. You should use `d2w` and `2w`.

Undo `u` | redo `Ctrl r` or `<C-r>` (undo the undos of `u`)\
The capital `U` return the whole line to its original state. It undo all the changes on a line.

Copy whole line: `yy` or `Y` (including the linebreak character at the end of each line)\
To yank everything from your current location to the end of the line: `y$`.

`yiw` yank in a word, copy word if you're in the middle of it

### deleting | changing

**Note**: Cần phân biệt các khái niệm delete vs change,  in vs around.

Both `d` and `c` save deleted text into register.

Press `x` to delete the character under the (block) cursor in normal mode.

`dw` - delete until the start of the next word, EXCLUDING its first character. `2dw` or `d2w` delete two words | `5d5w` 5 times delete 5 words. Note that the cursor must be placed at the beginning of the word when using `dw`.

`de` - to the end of the current word, INCLUDING the last character. Cái này hơi weird, nên dùng `dw`. \
`ce` change until the end of a word.

 `d$` - to the end of the line, INCLUDING the last character & the cursor. `d0` delete to beginning of line.

`di"` to delete inside quote. If you are in the middle of a word: `diw` or delete-in-word.

`ciw` Change in word
`ci"` Change in quotes.  Example: `print("Tran Kim Phuong")`. You can also use it with square brackets `()` and curly brackets `{}`.
`ca"` Change around quotes. Giống change in quote nhưng sẽ delete (cut) luôn 2 cái `""` quotes

Vim with **markup (like HTML tags)**

- `cit` change in tag like `<p>`, `<div>`.
- `dat` delete around tag, `dit` delete in tag

Delete whole line: `dd` (can be combine with number like `2dd`). `dd` also copy the deleted line. In other words, all "deleting" key bindings in Vim is actually cutting. So you can always use `p` to paste (put).  
Change the whole line: `cc`  
Delete the rest of the line from cursor block -> EOL: `D` (uppercase `d`) | `C` changes the rest of the line

`dd` + `p`, will put the deleted line to the line below the cursored line not after the cursor.
To put back text that has just been deleted, type   p .  This puts the deleted text AFTER the cursor (if a line was deleted it will go on the line below the cursor).

## Vim motions

Count Your Move.

A **hard wrap** inserts actual line breaks in the text at wrap points, with soft wrapping the actual text is still on the same line but looks like it's divided into several lines.

word & WORD navigation:

- Go to the beginning of the _next_ word/WORD: `w` | `W`. Dùng capital `W` để jump css class names like `display-sm-none`. `W` will also skip hyphens.
- Jump of the end of the next word: `e` & `E`. Giống dùng `w`.

- Jump to the beginning of the _previous_ word: `b` or `B` (works similar to `w` and `W`).\
- Go to the end of the previous word `ge` or `gE`

So what are the similarities and differences between a word and a WORD? Both word and WORD are separated by blank characters. A word is a sequence of characters containing only `a-zA-Z0-9_` (alphanumeric + `_`). A WORD is a sequence of all characters except white space (a white space means either space, tab, and EOL). To learn more, check out :h word and :h WORD and [this article](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md)

- Go to beginning of the line: `0`
- Go to end of the line: `$`

Additionally, you can use `^` to go to the first non-blank character in the current line and `g_` to go to the last non-blank character in the current line. If you want to go to the column `n` in the current line, you can use `n|`.

You can scan for next character with `f{char}`/`t{char}`. `f` takes you to the first letter of the match and `t` takes you till (right before) the first letter of the match. So:

- If you want to search for "h" and land on "h", use `fh`.
- If you want to search for first "h" and land right before the match, use `th`.

If you want to go to the _next_ occurrence of the last `f` search, use `;`. To go to the previous occurrence of the last current line match, use `,`. A good tip to go anywhere in a line is to look for least-common-letters like "j", "x", "z" near your target.

`F` and `T` are the backward counterparts of `f` and `t`. To search backwards for "h", run `Fh`. To keep searching for "h" in the same direction, use `;`. Note that `;` after a `Fh` searches backward and `,` after `Fh` searches forward.

Scan document for next/previous match: `/pattern<CR>` or `?pattern<CR>`

### Sentence and Paragraph Navigation

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

### Vim grammar

> Command Count Motion

- Motion is anything that moves the cursor
- Command are: `y`, `d`, `c`, `v`

Vim key bindings can be combined with number keys

- `5j`: go down 5 lines
- `15l`: jump 15 characters to the right
- `3u`: undo 3 times\

Almost every key commands can be combine with numbers

[This article] from `Learn Vim the Smart Way` explain Vim's Text Objects

### indentation

`>>` shift current line to the right\
`<<` shift current line to the left\
If you have selected the whole line in `visual mode`, you just press `>` once

automatically indent: `==` can also be used on selection of code\
Auto-indent the whole file: `gg=G`

### Vim Composability and Grammar

Vim grammar is subset of Vim's composability feature. Composability means having a set of general commands that can be combined (composed) to perform more complex commands. The true power of Vim's composability shines when it integrates with external programs. Vim has a filter operator (!) to use external programs. Learn more [here](https://github.com/iggredible/Learn-Vim/blob/master/ch04_vim_grammar.md#composability-and-grammar)

### Search in file & substitute

Search is a form of Command Line mode. Depending on how we entered Command Line mode, we can browse our Ex Command or search history with the `<Down>` and `<Up>` keys.

The search command: `/<search phrase>` search forward | `?<search phrase>` search backward
`n` to jump to the next instance | `N` jump to previous instance

To go back to where you came from press  `CTRL-o`  (Keep Ctrl down while pressing the letter o).  Repeat to go back further.  `CTRL-I` goes forward. Cái này giống `n` và `N` ở trên.

NOTE: When the search reaches the end of the file it will continue at the start, unless the 'wrapscan' option has been reset.

In normal mode, move the cursor to any word. Press `*` to search forwards for the next occurrence of that word, or press `#` to search backwards.

MATCHING PARENTHESES search: type  `%`  to find a matching ),], or }. You can jump from the open parentheses to the closing one and vice versa. **NOTE**: This is very useful in debugging a program with unmatched parentheses!

[Find and replace](https://vegastack.com/tutorials/find-and-replace-in-vim-vi/#:~:text=The%20%3Asubstitute%20(%3As)%20command,other%20mode%20to%20regular%20mode.)

Learn Vim the Smart Way's [article](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md#search-navigation) covers in much more detail

The substitute command

`:s/thee/the` substitute 'the' for 'thee'. Note that this command only changes the _first occurrence_ of "thee" in the cursored line.  
Type  `:s/thee/the/g`. Adding the  g  flag means to substitute globally in _the line_, change all occurrences of "thee" in the line.

To change every occurrence of a character string between two lines, type   `:#,#s/old/new/g` where #,# are the line numbers of the range of lines where the substitution is to be done.
Type  `:%s/old/new/g`       to change every occurrence in the whole file.
Type   `:%s/old/new/gc`    to find every occurrence in the whole file, with a prompt whether to substitute or not.

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

## The Dot Command

## Different modes of Vim

The default mode is **normal mode**. Pressing `Esc` to go back into normal mode.

Type `:` to enter **command mode**. You can use TAB completion in command mode.

`:!{command}` to execute an external shell command. NOTE: It is possible to execute any external command this way, also with arguments.

To insert (retrieve, read) the contents of a disk file in the current pwd and puts it below the cursor position, type  `:r FILENAME` (vimtutor Lesson 5.4: RETRIEVING AND MERGING FILES)

NOTE:  You can also read the output of an external command.  For example, `:r !ls`  reads the output of the ls command and puts it below the cursor.

`:e ~/.vimrc` switch to editing a new file.

To enter **insert mode**:\
`i` before the focus block | `I` insert to beginning of the line\
after the cursor (appending): `a` | `A` append to end of line\
open a new line below the cursor: `o` | `O` open new line above.

**Visual mode** is used for selection for yank, deleting, change. . Press `v` to enter visual mode. Capital `Shift v` will enter visual line mode (select the current line).

`viw` select current word. `viW` with a capital `W` select words with hyphen `-`
`vi(` will select everything inside parentheses `()` and enter visual mode. `va"`

- Delete selection: `d`.
- Change selection: `c`, delete & enter insert mode.\
-Copy (yanking): `y`, including the cursor.

Paste after cursor block: `p` | `P` will paste before the cursor block.\
Note that using `p` after `yy` or `dd` will paste the new line _below_ the current line and `P` will paste the new line above the current line.

visual line mode: `Shift v`\
visual block mode: `Ctrl v` used to select column-wise

`v  motion  :w FILENAME`  saves the Visually selected lines in file FILENAME (vimtutor Lesson 5.3: SELECTING TEXT TO WRITE)

Replace mode: To replace the character selected by the cursor: `r`. Type  `rx`  to replace the character at the cursor with the character "x". You can replace "x" for other character.

Type a capical `R` will enters **Replace mode** until  `<ESC>`  is pressed. Replace mode is like Insert mode, but every typed character deletes an existing character (vimtutor Lesson 6.3: ANOTHER WAY TO REPLACE).

`s` delete the character under the cursor and enter insert mode.

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

## Vimscript

## Terminology

[What is Ex command in Vim](https://www.cduan.com/technical/vi/vi-2.shtml). Ex command có dạng `:command` bắt đầu bằng ":". Ex command == command-line commands

## References

[Learn Vim (the Smart Way)](https://github.com/iggredible/Learn-Vim/tree/master). Có một trang same content nhưng ko phải trên Github

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/)

The [Wiki article](https://en.wikipedia.org/wiki/Vim_(text_editor)) about Vim is a very good introduction. It also has information about Neovim as well.

[vimgolf](https://www.vimgolf.com/) challenges

[vimbegood](https://github.com/ThePrimeagen/vim-be-good?tab=readme-ov-file) plugin by ThePrimegen

