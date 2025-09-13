# Neovim & VimTeX

Vim kick start: 23:13

Try these plugins:
- nvim-surround
- powerline

## General notes

The primeagen có terminal mà khi mở vim cái background transparent hiện wallpaper luôn. Kể cả ở ngoài terminal cũng transparent luôn

Don't use the text editor (nvim) as a terminal like the built-in terminal in VS-code. Use nvim + tmux for a separate terminal. The Primeagen

Typing `\vim` will ignore the alias in case your 'vim' is aliased to 'nvim'

`ripgrep` is good to have. Run `which rg` to check. Fuzzy finder like Telescope will use something like ripgrep to find your files.

Confused between: `vim.g` and `vim.opt` => read `:h vim.o`, `:h vim.opt`

Trong i3 có mod key, trong vim có **leader key**.

Plugins are installed (by package manager such as Lazy.nvim and Packer) inside `~/.local/share/nvim`

[vim.o](https://neovim.io/doc/user/lua-guide.html#_vim.o): behaves like :set
vim.go: behaves like :setglobal\

**Save session:**

## Runtimepath

The `after/` directory [here](https://neovim.discourse.group/t/neovim-after-folder/3318/3)

## Nerds font

## Navigating project, File tree

**TODO:**
- telescope

Telescope is a fuzzy finder, dùng chung với LSP. Mở file trực tiếp đòi hỏi phải nhớ project structure trong đầu (which is helpful) và nó là cách nhanh nhất để navigate files.

`persistence.nvim` để save project state khi close vim mở lại vẫn còn đó.

`breadcrumbs.nvim` hiện breadcrum on top of the screen

Trong vim có khái niệm [mark](https://github.com/iggredible/Learn-Vim/blob/master/ch05_moving_in_file.md#marking-position).

plugin "lua line" change indicate which buffer is changed and require saving bằng dấu chấm giống trong VScode

Cần phân biệt search (word) in find vs search file (name).

Learn Vim the Smart way's [article](https://github.com/iggredible/Learn-Vim/blob/master/ch03_searching_files.md) about searching files and find phrases in files in Vim.

You can search files without plugins and search with fzf.vim plugin

[This post](https://stackoverflow.com/questions/51306648/why-is-used-to-search-recursively-through-current-directory-in-vimgrep-vim) cover the two asterisk sign `**` in vim

[This article](https://vonheikemen.github.io/devlog/tools/using-netrw-vim-builtin-file-explorer/) cover `netrw`, vim's builtin file explorer.

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

### File tree (side-bar)

sidebar khá là chậm. Nó chỉ nên dùng để understand the tree-structure of the project. Không nên dùng để navigate between files.

### Harpoon

Its just marks, per project, saves position on quit/change buffer

UI to swap position/delete

Each project có ~ 4 mark ứng với 4 important file and 4 fingers. Tất cả những file còn lại dùng telescope + LSP + file tree.

Muốn delete/change entries in harpoon thì phải "write" the change to the harpoon menu with a `:w` [src](https://www.reddit.com/r/neovim/comments/1axs71c/how_do_i_delete_a_file_entry_in_harpoon/).

**Reference:**

## Auto-completion & snippet

`nvim-cmp` is a completion plugin for neovim coded in Lua. It require `L3MON4D3/LuaSnip` which is called a **snippet engine**.

super TAB là khi phím `TAB` đảm nhiệm nhiều vai trò.

[This article](https://forum.sublimetext.com/t/when-to-use-snippets-completions/20650) discusses a slight difference between snippet and auto-completion. Theo mình hiểu thì snippet là type 1 đằng ra 1 nẻo. Input type vô không cần giống output của snippet. Còn auto-complete là type khoảng 20% nó sẽ tự động (auto-) fill in 80% còn lại.

[Using fplugin](https://www.jackfranklin.co.uk/blog/using-ftplugin-in-vim/) in Neovim.

Neovim có 3 types of tex file: plaintex, tex (for LaTeX), latex. [Read here](https://neovim.io/doc/user/filetype.html#ft-tex-plugin) for more info. Or [this reddit post](https://www.reddit.com/r/neovim/comments/14milq0/why_would_nvimafterftplugintexlua_work_for_latex/). Nên khi đặt tên file trong nvim/LuaSnip phải đặt tên theo quy tắc.

**Visual Selection**

Pressing `<Tab>` in visual mode will then store the visually-selected text in a LuaSnip variable called `LS_SELECT_RAW`, which we will reference later to retrieve the visual selection.

**REFERENCES:**\
Luasnip README, LuaSnip , `luasnip.txt` (inside doc/ directory)
[ejmastnak LuaSnip guide](https://ejmastnak.com/tutorials/vim-latex/luasnip/)

ejmastnak [github](https://github.com/ejmastnak/dotfiles/tree/main/config/nvim) dot file for nvim

Lua regex patter official doc [here](https://www.lua.org/pil/20.2.html)

[Doc.md](https://github.com/L3MON4D3/LuaSnip/blob/master/DOC.md) => LuaSnip main doc

## Lua & Vimscript

After understanding a bit more about Lua, you can use `:help lua-guide` as a reference for how Neovim integrates Lua.\
`:help lua-guide`\
(or HTML version): https://neovim.io/doc/user/lua-guide.html

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

## LSP & Treesitter



chafa /Users/anhao/Desktop/phuong.jpeg --format symbols --symbols vhalf --size 60x17 --stretch; sleep .1


## Nvim Key mapping

`n`	Normal mode map. Defined using ':nmap' or ':nnoremap'.
`i`	Insert mode map. Defined using ':imap' or ':inoremap'.
`v`	Visual and select mode map. Defined using ':vmap' or ':vnoremap'.
`x`	Visual mode map. Defined using ':xmap' or ':xnoremap'.
`s`	Select mode map. Defined using ':smap' or ':snoremap'.
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