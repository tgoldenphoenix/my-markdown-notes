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

**References:**
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
