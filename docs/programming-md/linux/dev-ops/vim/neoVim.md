# Neovim & VimTeX

Vim kick start: 23:13

Try these plugins:

- nvim-surround
- powerline

## General Notes

The primeagen có terminal mà khi mở vim cái background transparent hiện wallpaper luôn. Kể cả ở ngoài terminal cũng transparent luôn

Don't use the text editor (nvim) as a terminal like the built-in terminal in VS-code. Use nvim + tmux for a separate terminal. The Primeagen

Typing `\vim` will ignore the alias in case your 'vim' is aliased to 'nvim'

`ripgrep` is good to have. Run `which rg` to check. Fuzzy finder like Telescope will use something like ripgrep to find your files.

Confused between: `vim.g` and `vim.opt` => read `:h vim.o`, `:h vim.opt`

Trong i3 có mod key, trong vim có **leader key**.

Plugins are installed (by package manager such as Lazy.nvim and Packer) inside `~/.local/share/nvim`

- [vim.o](https://neovim.io/doc/user/lua-guide.html#_vim.o): behaves like `:set`
- vim.go: behaves like :setglobal

- `vim` is for latex
- `lvim` is lazy vim for markdown notes

Neovim uses the Lua programming language for plugin development (and configuration) in addition to the native VimScript that its sibling uses.

However, where most Vim plugins also run on Neovim, the inverse is not always true, and there are Lua plugins that only work with Neovim.

## General Keybinding Commands

`<space> qq` close neovim

`<space>us` enable spell check

## Lua & Vimscript

`:help lua`

`:help lua-guide-api` to learn about the vim-specific APIs.

After understanding a bit more about Lua, you can use `:help lua-guide` as a reference for how Neovim integrates Lua.\
`:help lua-guide`\
(or HTML version): <https://neovim.io/doc/user/lua-guide.html>

`nvim .` open the current pwd inside the default Netrw directory listing

`vim.g.mapleader` là set global config\
`vim.opt.number` là set option. See `: vim.opt`

[This](https://stackoverflow.com/questions/73358168/where-can-i-check-my-neovim-lua-runtimepath) => neovim runtime path
[This article](https://zignar.net/2022/11/06/structuring-neovim-lua-plugins/) covers how to find neovim plugin/ and ftplugin/ directories

5:43 of [this video](https://www.youtube.com/watch?v=m8C0Cq9Uv9o) show you how to set options with Lua (terminal method)

When you're writing more complicated Vimscript later in this book you may find yourself wanting to "print some output" to help you debug problems. Plain old `:echo` will print output, but it will often disappear by the time your script is done. Using `:echom` will save the output and let you run `:messages` to view it later.

Variable vs option in vim [here](https://stackoverflow.com/questions/944229/vim-options-variables-and-converting-between-the-two) and [here](https://www.reddit.com/r/neovim/comments/12d95do/can_someone_help_me_understand_the_diff_bw_vimg/). Doc [here](https://neovim.io/doc/user/lua.html#vim.g) and [here](https://neovim.io/doc/user/eval.html#g%3A)
How to view variable [here](https://stackoverflow.com/questions/9193066/how-do-i-inspect-vim-variables)

how to call Lua from within Vimscript using `:help :lua` and `:help :lua-heredoc`

Use `:help vim.cmd()` to run vimL inside .lua files.

lua array index start with `1` not `0`

References:

[Guide to using Lua in Nvim](https://neovim.io/doc/user/lua-guide.html#lua-guide) or `:help lua-guide` on how Neovim integrates Lua\
[Lua engine in nvim doc](https://neovim.io/doc/user/lua.html#_introduction) or `:help lua`\
[Learn X in Y Lua](https://learnxinyminutes.com/docs/lua/)

## Kickstart nvim & Lazy nvim

Cài ripgrep, check `which rg`.

[Install nerd font on Linux](https://www.youtube.com/watch?v=cBOaYidGaCQ)

To check the current status of your plugins, run `:Lazy`
run `:Lazy update` to update plugins

Thường mình sẽ dùng `opts` để config vì nó được automatically passed to config() function. Mình chỉ viết config() ra luôn khi dùng opts không được[READ MORE](https://lazy.folke.io/spec#spec-setup)

`:Inspect` để coi all the currently-applied hightlight

## Lazyvim Distro Basic

To get the best Vim editing experience, you want a GPU accelerated terminal. What’s that mean? Basically that you will be using the chip designed to render photo-realistic video for rendering source code.

- kitty has good documentation, alacritty; wezterm bad documentation
- Windows Terminal does claim to be GPU accelerated

Install terminal font and configure your terminal to use it.

The `Lazy.nvim plugin manager` should not be confused with `LazyVim` itself, though both are maintained by the same person.

`<space>l` open the plugin manager. Press `q` to close it.

Sync to running install, clean, and update in a single action

press the spacebar to enter “Space mode”. Space mode is a LazyVim concept; it does not exist in a raw Neovim installation.

`Dashboard mode` là cái dashboard khi mới mở neovim lên

### The `lazy.nvim` plugin manager

- `:Lazy`
- `:LazyExtras`

any Lua files inside the `lua/plugins` subdirectory will automatically be loaded by LazyVim, no matter what their name is. You can disable default lazyvim plugins, install new one or pass option to default lazyvim plugins.

- `.config/nvim/lua/config/keymaps.lua` This is typically where you configure or modify keybindings that are not specific to plugins, but rather modify core Neovim or LazyVim functionality.
- In the `keys` field of the Lua table passed to a plugin. This is typically where you map global Normal mode keybindings to set up a plugin.
- In the `opts` (options) argument passed into a plugin’s configuration. The format of the options for any one plugin are plugin-specific, but many plugins prefer to set up keymaps on your behalf through options instead of having you do the mapping yourself. This is especially true if the keymaps define a different “mode” or only apply if the plugin is currently open or active.

To be clear, `keys` is a LazyVim concept (technically, it’s actually part of the underlying Lazy.nvim plugin manager). Any plugin configuration can have a keys array table, and those keybindings will be merged with the default Neovim keybindings, the LazyVim keybindings, your custom global keybindings, AND any other plugin keybindings.

Each item in the keys table is another Lua table with (in this case) three fields. The first two fields are positional and represent the keybinding name and the Lua callback function that gets called whenever that keybinding is invoked. The third field is a named field, `desc`, which provides a string description that will be shown in the Space mode menu.

Special keys are indicated to Vim’s keybinding engine using angle brackets, so you will often see notations such as <Space>, <Right>, <Left> or `<BS>`, `<leader>`.

---

If you supply an `opts` table, it will be merged with the default LazyVim one (if there is one).

You’ll need to read each plugin’s documentation to know exactly what options are available for it. You’ll also need to review the default configuration that LazyVim sets up for that plugin so you understand how it will merge.

the `opts` entry in a Lazy.nvim plugin’s configuration table can also be a function instead of a static table. The function accepts the previous opts table as it was configured by LazyVim as an argument. Your function needs to **modify** this table to suit your desired behaviour.  
The function based version of opts does not return a new opts table; it needs to modify the one that was passed in.

---

The opts table depends entirely on what the plugin expects. You want to read the documentation on the github page of the plugin.

Most modern Lua plugins will be documented as having to call a `setup` function with a Lua table containing the configuration. If the plugin you are trying to set up does not have explicit Lazy.nvim instructions, don’t worry: Whatever gets passed into that setup function is what you need to include in the `opts` passed to the LazyVim plugin manager.

```lua
require('guess-indent').setup {
  auto_cmd = true,  -- Set to false to disable automatic execution
  override_editorconfig = false,
  -- more configs...
}
```

## Moving around in a File

### Seek mode: `flash.nvim`

If you can see the code you want to navigate to (i.e. because the file is currently open and the code is scrolled into view), `flash.nvim` is almost always the fastest way to move your cursor there.

To invoke `flash.nvim`, press the `s` key in Normal mode (seek). The text fades to a uniform colour and there’s a little lightning symbol in the status bar indicating that Flash mode is active.  
Since you know where you want the cursor to be, your eyes are probably looking right at it, and you know exactly what character is at that location. So after entering Seek mode, simply type the character you want to jump to.

If you have multiple files open in split windows (which we’ll discuss in Chapter 9), Seek mode can be used to move your cursor anywhere on the screen, not just in the currently active split.

Remember, `flash.nvim` seek mode only works if the text you want to jump to is visible on the screen. You can’t label something you can’t see!

The default `s` in vim deletes the character under the cursor & enter insert mode.

`flash.nvim` sẽ auto-generate `jump labels`. Mình chọn label mà mình muốn jump đến.

---

`flash.nvim` also modify the `f`, `F`, `t`, `T` motions:

The default `f` only work inside current line, not anymore.

You can use `3f` (with a count) to jump. Shift `F` to find or jump backward (can also be counted). You can still use `;` & `,` to jump.

`t` & `T` for "till" work similarly

`dsfoos` to delete text between the current cursor position and the label `s` that pops up when you use Seek mode to seek to `foo`. Note that Seek mode always jumps to the **beginning** of the word you searched for. This means that if the foo you jump to is after the current cursor location, the oo will not be deleted, but the f will. But if the foo you jump to is before the current cursor location, all three letters of foo will be deleted.

### Z Mode

The `z` menu mode (normal mode) is an eclectic mix of cursor positioning, code folding, and random commands.

## Unimpaired Mode

Like the sentence and paragraph motions (`(`, `)`, `{` and `}`), the square brackets (`[` & `]`) allow you to move to the previous or next _something_, except the something depends what key you type after the square bracket.

Collectively, these pairs of navigation techniques are sometimes referred to as `Unimpaired mode`, as they harken back to a foundational Vim plugin called `vim-unimpaired` by a famous Vim plugin author named `Tim Pope`. LazyVim doesn’t use this plugin directly, but the spirit of the plugin lives on.

The commands to work with `(`, `<`, and { are quite a bit more nuanced than they look. They **don’t** blindly jump to the next (if you started with `]`) or previous (if you used `[`) parenthesis, angle bracket, or curly bracket. If you wanted to do that, you could just use `f(` or `F(`.  
Instead, they will jump to the next **unmatched** parenthesis, angle bracket, or curly bracket. That effectively means that keystrokes such as `[(` or `]}` mean “jump out”. So if you are in the middle of a block of code surrounded by `{}` you can easily jump to the end of that block using `]}` or to the beginning of it using `[{`, no matter how many other curly-bracket delimited code blocks exist inside that object. This is useful in a wide variety of programming contexts, so invest some time to get used to it.

As a shortcut, you can also use `[%` and `]%` where the `%` key is basically a placeholder for “whatever is bracketing me.” They will jump to the beginning or end of whichever parenthesis, curly bracket, angle bracket, or square bracket you are currently in.

That last one (square bracket), is important, because unlike the others, `[[` and `]]` do not jump out of square brackets, so using `[%` and `]%` is your only option if you need to jump out of them.

### Jump by Reference

Instead of jumping out of square brackets as you might expect, the easy to type `[[` and `]]` are reserved for a more common operation: jumping to other references to the variable under the cursor (in the same file).

This feature typically uses the language server for the current language, so it is usually smarter than a blind search. Only actual uses of that function or variable are jumped to instead of instances of that word in the middle of other variables, types, or comments as would happen with a search operation.

As you move your cursor, LazyVim will automatically highlight other variable instances in the file so you can easily see where `]]` or `[[` will move the cursor to.

### Jump by Language Features

The `[c`, `]c`, `[f`, `]f`, `[m`, and `]m` keybindings allow you to navigate around a source code file by jumping to the previous or next class/type definition, function definition, or method definition. The usefulness of these features depends a bit on both the language you are using and the way the Language Service for the language is configured, but it works well in common languages.

By default, those keybindings all jump to the start of the previous or next class/function/method. If you instead want to jump to the end, just add a Shift keypress: `[C`, `]C`, `[F`, `]F`, `[M`, and `]M` will get you there.

Note that these are not the same as “jump out” behaviour: if you have a nested or anonymous function or callback defined inside the function you are currently editing, the `]F` keybinding will jump to the end of the nested function, not to the end of the function after the one you are currently in.

I personally don’t use these keybindings very much as there are other ways to navigate symbols in a document that we will discuss later. But if you are editing a large function and you want to quickly jump to the next function in the file, `]f` is probably going to get you there faster than using `j` with a count you need to calculate, or even a `Control-d` followed by `S` to go to seek mode.

### Jump to End of Indention

If you are working with indentation-based code such as Python or deeply nested tag-based markup such as HTML and JSX, you may find the `[i` and `]i` pairs helpful.

These are provided as part of the `snacks` suite of plugins, via `snacks.indent`. This plugin helps visualize the levels of indentation in a file.

the unimpaired commands `[i` or `]i` to jump out of the current indentation level; it will go either to the top or the bottom of whichever indentation line is currently highlighted.

### Jumping to Diagnostics

Jump to diagnostics are `[d` and `]d`. If you only want to focus on errors and ignore hints and warnings, you can use `[e` and `]e`. Analogously, the `[w` and `]w` keybindings navigate between only warnings.

If you are editing a file in a language that enables spellcheck, or you have enabled it explicitly with <Space>us, misspellings can be jumped to with `[s` and `]s`.

Finally, if you use `TODO` or `FIXME` comments in your code, you can jump between them using `[t` and `]t`.

### Jumping to Git Revisions

`[h` and `]h` allow you to jump to the next git “hunk”. A “git hunk” just refers to a section of a file that contains modifications that haven’t been staged or committed yet.

## Text Objects

`a` and `i` means around and inside (though in my head I always just pronounce them as “a” and “in”). The difference is that a operations tend to select everything that inside selects plus a bit of surrounding context that depends on the object that is defined.

### Textual Objects

The operators `w`, `s`, and `p` are used to perform an operation on an entire word, sentence, or paragraph, as defined previously: word is contiguous non-punctuation, sentence is anything that ends in a `.`, `?`, or `!`, and paragraph is anything separated by two newlines.

The difference between `around` and `inside` contexts with these objects is whether or not the surrounding whitespace is also affected.

- `diw` delete inside word
- `daw` delete around word; delete the word and **one** surrounding space character, so everything lines up correctly afterward with a single space
  - `daW`
- `dis` & `das` delete sentence
- `dip` & `dap`; when in `inside` mode, the blank line after the paragraph being deleted will still be there, but in `around` mode, it will remove the extra blank.

- `around` usually correctly sync up the white spaces after deletion more than `inside`
- Typically, I use `i` when I am changing a word, sentence or paragraph, with a `c` verb, since I want to replace it with something else that will need to have surrounding whitespace. But I use a when I am deleting the textual object with `d` because I don’t intend to replace it, so I want the whitespace to behave as if that object never existed.

### Quotes and Brackets

`di[`, `da(`, `ci{` or `ca<`.

These actually work with counts so you can delete the “third surrounding curly brackets” instead of the “nearest surrounding curly brackets” if you want to. I can never remember where to put the count, though! If your memory is better than mine, the syntax is to place the count **before** the a or i. So for example, `d2a{` will delete everything inside the second-nearest set of curly brackets. I’m not sure if that makes sense, so here’s a visual:

```javascript
class Foo {
    function bar() {
       let obj = {fizz: 'buzz'}
    }
}
```

- If my cursor is on the colon between fizz and 'buzz' then you can expect the following effects:
  - di{ will delete fizz: 'buzz' but leave the surrounding curly brackets.
  - c2i{ will remove the entire let obj = line and leave my cursor in Insert mode inside the curly brackets defining the function body.
  - c2a{ will do the same thing, but also remove those curly brackets, so I’m left with a function bar() that has no body.
  - d3i{ will remove the entire function and leave me with an empty Foo class.

You can also delete things between certain pieces of punctuation. For example, `ci*` and `ca_` are useful for replacing the contents of text marked as bold or italic in Markdown files.

If you want to operate on the entire buffer, use the `ag` or `ig` text object. So `cag` is the quickest way to scrap everything and start over and yig will copy the buffer so you can paste it into a pastebin or chatbot. The `g` may seem like an odd choice, but it has a symmetry to the fact that gg and G jump to the beginning or end of the file. If you need a mnemonic, think of yig as “yank in global”.

### Language Features

- LazyVim adds some helpful operators to perform a command on an entire function or class definition, objects, and (in HTML and JSX), tags. These are summarized below:
  - `c`: Act on class or type
  - `f`: Act on function or method
  - `o`: Act on an “object” (the mnemonic is a stretch) such as blocks, loops, or conditionals
  - `t`: Act on an HTML-like tag (works with JSX)
  - `i`: Act on a “scope”, which is essentially an indentation level (only available if the aforementioned `mini.indentscope` extra is installed)

### Next and Last Text Object

The text object feature is great if you are already inside the object you want to operate on, but LazyVim is configured (using a plugin called `mini.ai`) so that you can even operate on objects that are only near your cursor position.

Once installed, the next and last text objects can be accessed by prefixing the object you want to access with a `l` or `n`.

Consider the Foo.bar Javascript class again:

```javascript
class Foo {
    function bar() {
       let obj = {fizz: 'buzz'}
    }
}
```

If my cursor is on the `{` of the `function bar` line, I can type `cin{` to delete the contents of the `fizz: 'buzz'` object and place my cursor there in insert mode. I can save myself an entire navigation with just one extra n keystroke.

### Seeking Surrounding Objects

The flash.nvim plugin that gave us Seek mode, has another trick up its sleeve: the holy grail of text objects. After specifying a verb, you can use the `S` key (there is no `i` or `a` required) to be presented with a bunch of **paired labels** around the primary code objects surrounding your cursor.

The labels in this image are in green, and (typically) go in alphabetical order from “innermost” to “outermost”. The primary difference from Seek mode is that **each label comes in pairs**; there are two `a` labels, two `b` labels, and so on. The text object is whatever is between those labels.

If the next character I press is `a` (or enter, to accept the default), then I will change everything inside the curly brackets defining the obj. If I press `b`, it will also replace those curly brackets. Pressing `c` will change the entire assignment and d will change the contents of the function. Hitting e replaces the curly brackets as well, and f changes the full function definition. The g label is the contents of the class, while h changes the entire class.

This is a super useful tool when you need to change, delete, or copy a complex structure that does not immediately map to any of the other objects.

---

Seeking Surrounding Objects Remotely => This function would be useful If I worked with code a lot in neovim. [read here](https://lazyvim-ambitious-devs.phillips.codes/course/chapter-7/#_seeking_surrounding_objects_remotely)

## Operating on Surrounding Pairs

We’ve already seen the text objects to operate on the contents of pairs of quotation marks or brackets, but what if you want to keep the content but change the surrounding pair?

Maybe you want to change a double quoted string such as `"hello world"` to a single quoted `'hello world'`. Or maybe you are changing a `obj.get(some_variable)` method lookup to a `obj[some_variable]` index lookup, and need to change the surrounding parentheses to square brackets.

LazyVim ships with the `mini.surround` plugin for this kind of behaviour, but it’s not installed by default. It is an extra.

### Add Surrounding Pair

The default verb for adding a surrounding pair is `gsa`. That will place your editor in operator-pending mode, and you now have to type the motion or text object to cover the text you want to surround with something. Once you have finished inputting that object, you need to type the character you want to surround it with, such as " or ( or ). The difference between the latter two is that, while both will surround the text with parentheses, the ( will also put extra spaces inside the parentheses.

- `gsai[(` will select the contents of a set of square brackets (using `i[`) and place parentheses separated by spaces inside the square brackets. So if you start with [foo bar] and type gsai[(, you will end up with `[( foo bar )]`.
- `gsai[)` does the same thing, except there are no spaces added, so the same `[foo bar]` will become `[(foo bar)]`.
- `gsaa[)` will place the parentheses outside the square brackets, because you selected with `a[` instead of `i[`. So this time, our example becomes `([foo bar])`.
- `gsa$"` will surround all the text between the current cursor position and the end of the line with double quotation marks.
- `gsaSb'` will surround the text object that you select with the label `b` after an `S` operation with single quotation marks.

### Delete Surrounding Pair

Deleting a pair is a little easier, as you don’t need to specify a text object. Just use `gsd` followed by the indicator of whichever pair you want to remove.

So if you want to delete the [] surrounding the cursor, you can use `gsd[`.

If you want to delete deeply nested elements, you need to put a count before the `gsd` verb. So use `2gsd{` to delete the second set of curly braces outside your current cursor position. For example, if your cursor is inside the `def` of the string `{abc {def}}`, typing `2gsd{` results in `abc {def}`, leaving the “inner” curly braces around `def`, but removing the second outer set around the whole.

### Replace Surrounding Pair

Replacing is similar to deleting, except the verb is `gsr` and you need to type the character you want to replace the existing character with after you type the existing character.

So if you have the text `"hello world"` and your cursor is inside it, you can use `gsr"'` to change the double quotes to single quotes: `'hello world'`.

### Navigate Surrounding Characters

To move your cursor to the beginning or end of the pair. You can often do this using Seek mode, Find mode, or the Unimpaired mode commands such as [(, but there are other commands that are more syntax aware if you need them.

The vim built-in is the `%` motion.

The mini.ai plugin provides a similar feature using the `g[` and `g]` shortcuts. These shortcuts both need to be followed by a character type, so e.g. `g[(` will jump back to the nearest surrounding open parenthesis, and `g]]` will jump to the nearest closing square bracket. If you give it a count, it will jump out of that many surrounding pairs.

### XML or HTML Tags

The `mini.surround` plugin is mostly for working with pairs of characters, but it can also operate on html-like tags.

Say you have a block of text and you want to surround it with the `<p>` tags. String together the command `gsaapt`. That is `gsa` for “add surrounding” followed by `ap` for “around paragraph”. So we’re going to add something around a paragraph. Instead of a quote or bracket, we say the thing we are going to add is a `t` for tag.

mini.surround will understand that you want to add a tag, and pop up a little prompt window to enter the tag you want to add. Type the `p` for the tag you want to create. You don’t need angle brackets; just the tag name.  
If the tag you want to add has attributes, you can add them to the prompt. `mini.surround` is smart enough to know that the attributes only go on the opening tag.

## Clipboard, Registers, and Selection

The LazyVim configuration sets up the Neovim clipboard system to work with the OS clipboard automatically.

`d` & `c` in neovim (not in `vim`) saved text into the system clipboard.

Visual mode keymaps are independent of Normal mode keymaps, and plugins occasionally neglect to set them up for both modes. LazyVim is really good about keymaps, though, so you will rarely be surprised.

## Buffers and Layouts

The absolute easiest way to switch between buffers is using the H and L (i.e. Shift-h and Shift-l) keys. You will switch the buffer visible in the currently active window to whichever buffer is to the left or right of the current buffer in the buffer line.  
Alternatively, you can use the `[b` and `]b` commands which map to the same thing.

`<Space><Backtick>` jumps between the current file and the file that was most recently opened in the current window. In Vim parlance, this is referred to as “the alternate file.”

If you have too many buffers opened, the buffer line cannot show them all. `<Space><comma>` opens a filterable, scrollable list of currently open buffers. This is useful if you are working on large projects that have so many files that searching through them with `<Space><Space>` is difficult. Open the relatively low number of files that you actually need to access as active buffers, so they are easy to filter in the `<Space><comma>` buffer list.

`<Space>bo` Close all buffers other than the active one

`<Space>.` open the **scratch buffer**.

### Windows

In vim, window = pane or split.

The window management commands are collected in the `<Space>w` submode menu

- `<Space>wv` split window in half vertically
  - In a file explorer, use `Ctrl-v` to open a new file in vertical split
- `<Space>ws` split window in half horizontally
  - In a file explorer, use `Ctrl-s` to open a new file in horizontal split

To move your cursor between window splits, use `Ctrl` + `h j k l`.  
The `mrjones2014/smart-splits.nvim plugin` can be configured to navigate between Vim windows and Kitty, Wezterm, or Tmux panes with the same keybindings.

- You can close a window at any time using one of three keybindings:
  - `<Space>wq` closes the window, and if it is the only window open, exits (quits) Neovim.
  - `<Space>wc` closes the window unless it is the only window open, in which case it displays an error and refuses to close.
  - `<Space>wd` deletes the window. It is actually doing exactly the same operation as `<Space>wc`, but it is helpful for muscle memory for its symmetry with <Space>bd to “delete” an open buffer.
- In all three cases, the buffer remains open in the bufferline. Only the window split is closed.

If, instead, you want to close all splits except the active one, use `<Space>wo` for “only this window” or “close other” (whichever is easier to remember).

---

The easiest way to resize Vim splits is to use… the mouse. There is a vertical bar between vertical splits that you can click and drag on. The mouse cursor doesn’t change to give you any feedback that you can click and drag on it, but it works.

To change everything to a “default” size, use `<Space>w=` which will make all the windows “equally high and wide.”

---

## Folding

Most fold operations are accessible from the z mode menu accessible by typing z in Normal mode.

- To collapse a section of code into a fold, use whatever navigation operations you like to get to that section and type zc for “collapse fold”.
- To open it again, use zo for “open fold”.
- Alternatively, if you only want to remember a single keybinding, za will toggle a fold, collapsing if you are not on a folded line and expanding if you are.

If you have collapsed some folds and want to quickly get back to a point where there are no folds collapsed, use zR to open all folds. I had no idea what mnemonic is supposed to work with R, but an early reader helpfully pointed out that zr is “reduce folding”, so zR is “Reduce folding BUT BIGGER”.

You can even nest folds by folding already folded code. If you want to open folds recursively, use zO, which will open a fold and any folds that are nested under that fold.

The way LazyVim is configured, you don’t have much control over what gets folded, but it will usually do something close to what you expect based on where your cursor is in the document. “What you expect” depends on both the LSP and the TreeSitter grammar for the language you are working on, but I find it best to just let it do its thing and not disagree with it.

If you find that you want way more control over code folding, I recommend reading :help folding in its entirety. More than likely, you’ll decide that actually, you don’t want more control over folding and just want LazyVim to handle it for you!

## Session

LazyVim has built-in session management enabled by default. Simply close LazyVim with `<Space>qq` and you’re done. Tomorrow morning, use your terminal to cd to the folder you were working in. Open the session to the dashboard with the nvim command and hit s to be on your way.

Sessions are scoped to folders. So every time you change your working directory a different session is stored and kept up to date until you exit. To open a session, either cd to a directory in your terminal before opening Neovim, or use the :cd command after opening it.  
Alternatively, use the `<space>qS` command after you open Neovim. This will open a picker with all the folders you have recently worked in. Select one of those folders and LazyVim will automatically :cd to that folder and open the session.

## Programming Language Support

Visual Studio Code brought the world the concept of language servers, and all other text editors jumped on the idea.

Neovim build support for language servers into the editor itself.

In addition, Neovim also has built-in support for TreeSitter, a powerful library for parsing and identifying abstract syntax trees in source code while it is being edited, and LazyVim is configured with the plugins needed to make TreeSitter Just Work.

Language Server protocol gives us support for things like code navigation, signature help, auto-completion, certain highlighting and formatting behaviours, diagnostics, and more. TreeSitter gives us better syntax highlighting, code folding, and syntax based navigation such as provided by the S command you already know.

### Mason.nvim

The Lazy Extras may not install everything you need. For example, instead of the default Typescript formatting and linting tools, I prefer to use a new hyper-fast up-and-comer called Biome.

To install things like this, you can use the Mason.nvim plugin, which is pre-installed with LazyVim. To open Mason, use the `<Space>cm` keybinding. The window that pops up looks similar to the Lazy.nvim and Lazy Extras floating window, although it ships with annoyingly unrelated keybindings.

Mason is a very large database of programming language support tooling, including language servers, formatters, and linters, along with the instructions to install them.

Mason.nvim does assume a certain baseline is already installed on your system; for example if you are going to install something that is Rust-based, you better have a `cargo` binary, and if you are going to install something that requires Python support, Python and pip need to be available. In most cases, if you are coding in a given language, you already have the tools Mason needs to do its thing. The main thing that Mason takes care of is ensuring that the tools are installed in such a way that other Neovim plugins can find them.

use i to install the package under the cursor. The only other command you will use frequently in Mason is Shift-U to update all installed tools, and you can look up the rest with `g?`.

### Validating Things Installed Cleanly

As good as both LazyExtras and Mason are at installing language servers, linting, and formatting tools, setting them up is one of the places most likely to go wrong, no matter which editor you are using. So now is a good time to introduce several commands to validate that things are working as expected.

First, LazyVim pops up notifications in the upper right corner, as you have seen with the plugin updates. These disappear after a few seconds. Every once in a while, you need to be able to refer back to them.  
The secret is to use the keybinding `<Space>sn` to open the “Noice” search menu. Noice is the plugin that provides those little popups. Most often, you’ll want to follow this with either an a or an l to see all recent Noice messages, or just the last one.

The second command you’ll need for debugging LSPs is `<Space>cl`, which runs the command `:LspInfo`. It displays information about any language servers that are currently running and which buffers they are attached to.

If your LSP is having temporary problems—like showing incorrect diagnostics or unable to find a file you know is there—sometimes it just needs to be given a good kick with `:LspRestart`. The Svelte language server has a nasty habit of not picking up new files, so I’ve been using this one often enough lately to add a keybinding for it.

Two other super useful commands are `:checkhealth` and `:LazyHealth`. Both provide information about the health of various installed plugins. The former is a Neovim command that plugins can register themselves with to provide plugin health information, while the latter provides LazyVim-specific information. There is a lot of overlap in the output, but I find the `:LazyHealth` output is easier to read, and the :checkhealth output to be a bit more comprehensive. So I usually use :LazyHealth first and switch to checkhealth only if :LazyHealth didn’t yield the answer I need.  
Don’t expect to see green check marks across the board; you’ll make yourself crazy. For example, my checkhealth output contains a bunch of warnings from Mason. I have warnings for languages that I don’t generally need to edit files in. So if you don’t code in Java, there’s no reason to waste cycles trying to make the java warning go away.

### Diagnostic

If the window doesn’t pop up when you navigate to the diagnostic, you can use the `<Space>cd` keybinding to invoke it as long as your cursor is positioned somewhere within the underlined text. You can make the window disappear by moving your cursor with any motion key.

### Code Actions

Navigate to a diagnostic using whatever keybindings work for you (I live by ]d) and then invoke the <Space>ca menu where c and a mean “code action.” A picker menu will pop up with a list of any actions you can take.

## Navigating Source Files

### Go To Definition

`gd` (go to definition)

Go to definition is context dependent. It is enable by the language server.

Typically, once you’ve jumped to a definition and learned what you need to learn from that file, you’ll immediately want to jump back to where you started. You can do this easily using `Control-o`, as we discussed in Chapter 3 (and Control-i can move forward in your jump history).

---

The inverse of the Go to Definition command is “Go to References”. If you are looking at a function, variable, type, etc, and want to see all the places that variable is accessed, use the `gr` command.

Unlike with a definition or declaration, there will typically be more than one reference to a given word (a variable in isolation is a useless variable, indeed). So when you type gr it usually won’t immediately jump to a location. Instead it will pop up a picker view of all the references to the word that was under your cursor, with all the preview and filtering luxury that the picker always brings.

### Hover help

Most non-modal editors show you some help or “hover” text when you hold your mouse over a word or symbol. The quantity and value of this text varies widely depending on the LSP, but usually includes a function signature and documentation for the word under the cursor.

It’s probably possible to set up Neovim to show help texts on hover, but why would you move your hand to the mouse when LazyVim has such amazing navigation on the keyboard? Instead, use the (shifted) `K` keybinding. Yeah, K is a pretty stupid mnemonic to remember, but H and ? were already taken.

---

The `nvim-treesitter-context` extra is a helpful way to know where you are in the current file. It uses treesitter to figure out which functions and types you are in, and then pins the lines that define those types to the top of the editor. Enable it as usual by visiting :LazyExtras and hitting x over the line that contains nvim-treesitter-context.

This plugin keeps track of which class or function your cursor is currently in. If the function or type definition is so long that the signature scrolls off the screen, it will helpfully copy that signature into the first line or lines of the code window, highlighted with a slightly different background colour.

The effect is quite subtle, but the definitions that make their way into this context section tend to be exactly what you need while coding. If they fit on one line I can see function signatures and return types. You really don’t notice how often you have to scroll up to see what a variable is named in a function signature until you don’t have to do it anymore! And if you DO need to scroll up to the signature, simply type the relative line number followed by k and you’re there with no searching required.

If you need to disable the context temporarily, use the keybinding `<Space>ut`. We haven’t seen much of the <Space>u menu yet, where you can toggle various User Interface effects. This is largely because the default user interface is configured well enough that you don’t want to change it often!

### Listing Symbols

Another handy LSP feature is to search all the symbols in the current file or project. If you are editing a particularly long file and need to jump to a function that is not terribly close to your cursor, you might use the `<Space>ss` command (mnemonic is “search symbols”). As hinted by the double s, this is expected to be a fairly common action.

## Searching…

TODO:

## Navigating Project, File Tree

Telescope is a fuzzy finder, dùng chung với LSP. Mở file trực tiếp đòi hỏi phải nhớ project structure trong đầu (which is helpful) và nó là cách nhanh nhất để navigate files.

`persistence.nvim` để save project state khi close vim mở lại vẫn còn đó.

`breadcrumbs.nvim` hiện breadcrum on top of the screen

plugin "lua line" change indicate which buffer is changed and require saving bằng dấu chấm giống trong VScode

Cần phân biệt search (word) in find vs search file (name).

`<space>fc` find files in the LazyVim configuration directory

[This post](https://stackoverflow.com/questions/51306648/why-is-used-to-search-recursively-through-current-directory-in-vimgrep-vim) cover the two asterisk sign `**` in vim

`netrw` is vim's builtin file explorer.

If you used a modern text editor before, you are probably familiar with windows and tabs. Vim, however, uses _three_ display abstractions instead of two: **buffers, windows, and tabs.**

Use `:buffers`, `:ls` or `:files` to see all currently opened buffers

A **buffer** is an in-memory space where you can write and edit some text. Buffer handle the multiple-files-open-at-the-same-time needs. When you open a file in Vim, the data is bound to a buffer. When you open 3 files in Vim, you have 3 buffers. You can traverse between buffers, deleting buffers. etc.

The name of the buffer is at the bottom left corner on the status line of vim.

Window and tab are representational.

A **window** is a viewport on a buffer. You can have multiple windows. More about windows [here](https://github.com/iggredible/Learn-Vim/blob/master/ch02_buffers_windows_tabs.md#windows), `:h windows`\
Khi split screen trong vim là mình đang mở 2 windows. 2 window có thể show cùng 1 buffer (cùng 1 file), hoặc 2 file khác nhau (2 buffer). Nếu close 1 split window thì cái buffer vẫn còn mở chứ không đóng theo cái window.\
Khi dùng `:help` thì cũng tự động mở split 2 window top-bottom.
You can use just one window and switch between buffers.

A **tab** is a collection of windows. Think of it like a layout for windows.

### File tree explorer (the side-bar)

- `<space>-e` root directory (e for explorer)
- `<space>-E` cwd

`<space>-o` open markdown structure view on the right side

To hide the explorer window, just press `<Space>e` again while it is visible, or press `q`.

sidebar khá là chậm. Nó chỉ nên dùng để understand the tree-structure of the project. Không nên dùng để navigate between files.

### Harpoon

Its just marks, per project, saves position on quit/change buffer

UI to swap position/delete

Each project có ~ 4 mark ứng với 4 important file and 4 fingers. Tất cả những file còn lại dùng telescope + LSP + file tree.

Muốn delete/change entries in harpoon thì phải "write" the change to the harpoon menu with a `:w` [src](https://www.reddit.com/r/neovim/comments/1axs71c/how_do_i_delete_a_file_entry_in_harpoon/).

### File Pickers & Fuzzy Finding

`<space><space>` opens “Files In Current Project” picker (root dir)

In `fuzzy search`, you can "skip" characters inside the searching keyword.

- By default, the match is case insensitive. However, if you do use any capitalized letters in your search, it switches to a case sensitive mode (this is sometimes referred to as “smart case”).
- That means that `Ch` will match all the `Chapters`, but `cH` will not match anything. More interesting, `chF` will also not match anything at all because the presence of the capitalized `F` makes the whole thing case sensitive, and the chapters are all named with a capital C, so the lowercase `c` is not able to match them.

Sometimes you will start typing a word and realize you need to match something earlier in the path to distinguish it. For example, I started typing `outline`. “Outline” is a common word in this app. There are 243 matching files, and I realize I should probably have typed `comp` in front to narrow it to just files in the component directory. I could switch to Normal mode and edit the beginning of the line, but it’s faster to just type `<space>comp`. The picker will interpret the space as “filter the lines again, fuzzy matching this new word from the beginning”. Here we can see that only `comp...outline`

You can press `Enter` to open the file you want or you can even use a sort of Seek mode. Press the `Alt-s` keys while in the picker’s input area. You’ll see a label show up beside every line in the picker. These characters are labels for each line in the picker. Simply press one of the shown letters on your keyboard, and whichever line the label associated with that letter is on will be selected. Then press Enter to actually open the file.

If you want to open multiple files from the picker, instead of pressing `Enter`, press `Tab` to select the file. Navigate to other lines and press Tab to select them as well. Press Enter to confirm your selection.

If you need to scroll the results window to see something lower down in the list, use the `Control-d` and `Control-u` keys. If you want to scroll the preview window, use Control-f, and Control-b instead.

Finally, if you are in the picker window and decide you don’t want to open any files after all (or you got the information you needed from looking at the preview), press `Escape` **twice**. Why twice? The first time you press escape will put you into normal mode so you can use all the usual normal mode commands to edit your filter.

### The Difference Between “Root” and “Cwd”

- `<space><space>` or `<space>ff` is mapped to “Find Files (Root Directory)”.
- `<space>fF`, where the second F is shifted, is mapped to an action called “Find Files (cwd)”.

`cwd` refers to whatever directory your terminal was in when you typed `nvim` to open the editor. You can `:cd path/to/directory` change directory & `:pwd` inside neovim.  
If you `cd`, `<space>fF` will be shown relative to the new directory you have changed into.

The `root directory` is not a Vim concept, but is instead a Language Server Protocol (LSP) concept. The root directory is the directory that the LSP infers is the “home” directory of the currently open file. How the LSP does this is language (and language server) dependent.  
For example, in Javascript or Typescript projects it probably searches parent directories for the presence of a package.json or tsconfig.json file to detect the root directory. whereas in a Python project it might instead look for things like pyproject.toml or `poetry.lock`. Alternatively, some LSPs might just use the presence of a `.git` folder as the “root” of the project’s workspace.

The only reason this root directory is “often the same as your cwd” is that this is usually the folder you want to work from when you are working on a project, so it’s the one you cd into before you open Neovim.

## Auto-completion & Snippet

`nvim-cmp` is a completion plugin for neovim coded in Lua. It require `L3MON4D3/LuaSnip` which is called a **snippet engine**.

super TAB là khi phím `TAB` đảm nhiệm nhiều vai trò.

[This article](https://forum.sublimetext.com/t/when-to-use-snippets-completions/20650) discusses a slight difference between snippet and auto-completion. Theo mình hiểu thì snippet là type 1 đằng ra 1 nẻo. Input type vô không cần giống output của snippet. Còn auto-complete là type khoảng 20% nó sẽ tự động (auto-) fill in 80% còn lại.

[Using fplugin](https://www.jackfranklin.co.uk/blog/using-ftplugin-in-vim/) in Neovim.

Neovim có 3 types of tex file: plaintex, tex (for LaTeX), latex. [Read here](https://neovim.io/doc/user/filetype.html#ft-tex-plugin) for more info. Or [this reddit post](https://www.reddit.com/r/neovim/comments/14milq0/why_would_nvimafterftplugintexlua_work_for_latex/). Nên khi đặt tên file trong nvim/LuaSnip phải đặt tên theo quy tắc.

Pressing `<Tab>` in visual mode will then store the visually-selected text in a LuaSnip variable called `LS_SELECT_RAW`, which we will reference later to retrieve the visual selection.

REFERENCES:

Luasnip README, LuaSnip , `luasnip.txt` (inside doc/ directory)

[ejmastnak's LuaSnip guide](https://ejmastnak.com/tutorials/vim-latex/luasnip/)

ejmastnak [github](https://github.com/ejmastnak/dotfiles/tree/main/config/nvim) dot file for nvim

Lua regex patter official doc [here](https://www.lua.org/pil/20.2.html)

[Doc.md](https://github.com/L3MON4D3/LuaSnip/blob/master/DOC.md) => LuaSnip main doc

### Luasnip

lua pattern = regex

Lua patterns use the percent sign instead of the backslash to escape characters.

- `%d` (not `\d`) digits
- `%a` letters (uppercase and lowercase). Trong normal regex không có shorthand này
- `%w` alphanumeric characters (letter & digits)
- `%s` whitespace

## `LSP` & Treesitter

`chafa /Users/anhao/Desktop/phuong.jpeg --format symbols --symbols vhalf --size 60x17 --stretch; sleep .1` => terminal graphic

## Nvim Key mapping

`n` Normal mode map. Defined using ':nmap' or ':nnoremap'.
`i` Insert mode map. Defined using ':imap' or ':inoremap'.
`v` Visual and select mode map. Defined using ':vmap' or ':vnoremap'.
`x` Visual mode map. Defined using ':xmap' or ':xnoremap'.
`s` Select mode map. Defined using ':smap' or ':snoremap'.
`c`  Command-line mode map. Defined using ':cmap' or ':cnoremap'.
`o`  Operator-pending mode map. Defined using ':omap' or ':onoremap'.

<Space>  Normal, Visual and operator pending mode map. Defined using
         ':map' or ':noremap'.
!  Insert and command-line mode map. Defined using 'map!' or
   'noremap!'.

always use `noremap` or its relatives (e.g. vnoremap) unless you have an explicit reason not to (e.g. when working with <Plug> or <SID> mappings, which are meant to be remapped

## Installing Neovim on Windows

k

## Vim is not an IDE

[IDE, text editor and PDE](https://www.youtube.com/watch?v=QMVIJhC9Veg&t=354s) video by  TJ DeVries

[This reddit post](https://www.reddit.com/r/vim/comments/kqw8o4/why_do_people_say_vim_nvim_is_not_a_great_ide/) explain why vim is not an IDE

## VimTeX

How vim distinguish filetype `tex` vs `plaintex` [here](https://superuser.com/questions/208177/vim-and-tex-filetypes-plaintex-vs-tex)

In the context of Neovim, "RPC" refers to a broader concept of remote procedure calls, while "LSP" (Language Server Protocol) is a specific standard for communication between an editor (like Neovim) and a language server, providing features like code completion, go-to-definition, and diagnostics; essentially, LSP is a type of RPC specifically designed for language-related operations within an editor, making it the preferred method for advanced language features in Neovim

close quickfix menu: `:ccl[ose]`

build pdf, open zathura

## Debug

[how to debug keymap](https://vi.stackexchange.com/questions/7722/how-to-debug-a-mapping)

[here](https://vi.stackexchange.com/questions/3964/timeoutlen-breaks-leader-and-vim-commentary) Eliminating delays on ESC in vim and zsh

[how to verbose keymap](https://www.reddit.com/r/vim/comments/lzgv2t/how_to_find_out_where_a_key_mapping_is_defined/)

how to verbose option [src1](https://www.reddit.com/r/vim/comments/lzgv2t/how_to_find_out_where_a_key_mapping_is_defined/) and [src2](https://vi.stackexchange.com/questions/34745/foldmethod-setting-ignored-when-initializing-vim)

[Vim: What's the difference between let and set?](https://stackoverflow.com/questions/9990219/vim-whats-the-difference-between-let-and-set) and also how to translate them to lua code

## References

[A guide to supercharged mathematical typesetting](https://ejmastnak.com/tutorials/vim-latex/intro/) by ejmastnak

The XDG Base Directory Specification [article](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)

[This article](https://www.freedesktop.org/wiki/Specifications/) explain what is XDG.

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/) book

Benjamin Brast-McKie [github repo](https://github.com/benbrastmckie/.config)

**VS code I want in Neovim**: multi cursor features, code auto format on save, close every other files

[github](https://github.com/evesdropper/luasnip-latex-snippets.nvim) luasnip-latex-snippets.nvim by evesdropper

[here](https://gist.github.com/dtr2300/2f867c2b6c051e946ef23f92bd9d1180) Overview of Nvim Events

**Read later**:
