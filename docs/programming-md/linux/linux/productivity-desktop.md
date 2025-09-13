# Productive Desktop Environment

## Ideas

karabiner
sketchybar
kitty terminal
Marta file manager
raycast thay cho spotlight
keycaster hiện key lúc quay video
neofetch
An activity monitor: htop, gotop
qutebrowser (try)
Filemanager: martar, Vifm
Use alias to shorten commands
Enable case-sensitive command completion
display the git branch

## key conflicts need to be resolve

C S 4 take screenshot vs change space (C S 5 also)
C j anki toggle suspend vs skhd
`Cmd l` dùng để jump to the url bar is conflicting.

## OS & Browser shortkeys

Window only has `Ctrl` & `Alt` keys plus a key with the window logo. Thứ tự (fn) > Ctrl > window logo > Alt.

Mac has Ctrl, option and an additional Cmd key. Bàn phìm Mac không có Alt key. Thứ tự (fn) > Ctrl > option > command. Sometimes, the Alt keys in config file is equivalent to the option key on Mac (Not the Cmd)

The Cmd (on Mac) and Alt (on Window) is also known as the Meta key (M-)

`Cmd Q` Quit application
`Cmd w` Close window

`Ctrl d` exit tmux (compare to `ctrl C`???)

## File Managers

Krusader has the most features. Nhiều dependencies (40-50 dependencies). Hard to use. It is the Emac of file manager. It is a KDE application.\
File sync between drives, comparison between drives

Nautilus (or "Files" on GNOME): simple, chỉ có điều nó là GNOME application, nếu dùng với KDE thì khó tương thích.

**Functions I need**
- Remember your position after closing.
- Dual-panel mode

### fzf, zoxide

fzf is a command line fuzzy finder

zoxide is a better `cd`

### The MacOS finder

`Cmd S .` to view dot files in finder

## Desktop Environment, Window Manager

Desktop Environment such as GNOME, KDE có nhiều programs, Window Manager là một trong những programs đó. Otherwise, you can choose to build your own desktop environment _around your Window Manager of choice_.

[KDE Plasma](https://en.wikipedia.org/wiki/KDE_Plasma) is a set of graphical shells developed by KDE for Unix-like operating systems. 

[GNOME Shell](https://en.wikipedia.org/wiki/GNOME_Shell) is the [graphical shell](https://en.wikipedia.org/wiki/Shell_(computing)#GUI) of the GNOME desktop environment starting with version 3, which was released on April 6, 2011. It provides basic functions like launching applications and switching between windows. GNOME Shell is written in C and JavaScript as a plugin for Mutter. 

In contrast to the [KDE Plasma Workspaces](https://en.wikipedia.org/wiki/KDE_Plasma), a software framework intended to facilitate the creation of multiple graphical shells for different devices, the GNOME Shell is intended to be used on desktop computers with large screens operated via keyboard and mouse, as well as portable computers with smaller screens operated via their keyboard, touchpad or touchscreen. 

[GTK](https://en.wikipedia.org/wiki/GTK) (formerly **GIMP ToolKit** and GTK+) is a free software cross-platform widget toolkit for creating graphical user interfaces (GUIs). It is licensed under the terms of the GNU Lesser General Public License, allowing both free and proprietary software to use it. It is one of the most popular toolkits for the Wayland and X11 windowing systems.

GTK, GTK+, and Qt are GUI toolkits. These are [libraries](https://en.wikipedia.org/wiki/Library_\(computing\)) that developers use to design graphical interfaces, all running on top of the [X.org Server](https://en.wikipedia.org/wiki/X.Org_Server) or [Wayland](https://askubuntu.com/a/11561/101186). These are things that you need to install as dependencies. They're the Linux "equivalent" to Windows' GDI/GDI+. When an application uses any of these, it will usually have a general "look and feel".

[Wayland](https://en.wikipedia.org/wiki/Wayland_(protocol)) is a communication protocol that specifies the communication between a display server and its clients, as well as a C library implementation of that protocol. A display server using the Wayland protocol is called a Wayland compositor, because it additionally performs the task of a compositing window manager. 

GNOME by default use Wayland.

**GNOME** and **KDE** are Desktop Environments. GNOME primarily uses the GTK+ toolkit, while KDE primarily uses the Qt toolkit. There are applications designed _for_ GNOME or KDE, such as a settings menu or a default music player, usually in the appropriate toolkit. These Desktop Environments have a set of utilities/window managers/design specification to create a more unified desktop. You can mix the two if you feel like it, but you may run into issues with colliding standards and applications (which is a bit more common on systems like Arch).

[Unity](https://en.wikipedia.org/wiki/Unity_(user_interface)) uses many of the GNOME utilities and backends (Nautilus, Rhythmbox, gvfs, etc.), so Unity is more GNOME than KDE.

[Xfce](https://en.wikipedia.org/wiki/Xfce) or XFCE (pronounced as four individual letters, /ɛks ɛf siː iː/) is a free and open-source desktop environment for Linux and other Unix-like operating systems.

### Yabai & skhd

skhd is a simple hotkey daemon for macOS.

2 màn hình là 2 display. Easch display có nhiều spaces, each space (layout/window tree) có nhiều window, mỗi window là 1 ứng dụng đang mở

**skhd** is able to hotload its config file, meaning that hotkeys can be edited and updated live while **skhd** is running.

yabai wiki page có phần **Debug output and error reporting**

Troubleshooting command `yabai -V` for `--verbose` and `skhd script` (must stop skhd and yabai service first to use these commands).

`[yabai|skhd] --[start|stop|restart]-service`

### i3 WM & nitrogen

The window key (or **super key**) is the modifier (mod key). Mình cũng có thẻ set Alt key as the mod key.

The config file is at `~/.config/i3`. `nitrogen` sets the wallpaper

**Commands**\

Enter terminal: `S Enter` `$mod+Enter`

Open `dmenu`: `mod d`

exit i3: `$mod Shift e`

To move a window to another **workspace**, simply press `$mod+Shift+num` where num is (like when switching workspaces) the number of the target workspace.

To **restart** i3 in place (and thus get into a clean state if there is a bug, or to upgrade to a newer version of i3) you can use `$mod+Shift+r`.

**Todo:**

chỉnh kill thành super q, bỏ shift

**References**

[i3 official doc](https://i3wm.org/docs/userguide.html)

## Tmux

Kitty có tích hợp multi-plexer giống tmux.

**Commands**

To enter Command mode, press `Prefix :` (the colon) from within a running tmux session.

Key tables may be viewed with the `:list-keys` command.

`tmux list-sessions` or `tmux ls` list sessions
`tmux attach`, `exit`

`prefix-%` vertical split

`tmux new-window`

`tmux kill-server`

`set` is the alias of `set-option`.
`set -g` is used to set global options and `-ga` appends values to existing settings.

## Sketchy bar (macOS)

Config location: `~/.config/sketchybar`

**Installation notes:**

Dependencies: brew install `font-hack-nerd-font`, `jq` (command line JSON processor), `font-sf-pro`, `sf-symbols`\
Hide the default MacOs menu bar

`brew services start sketchybar` services will automatically start at computer startup (why???)

`sketchybar --reload`

**References**

[video](https://www.youtube.com/watch?v=8W06wMNZmo8&t=207s) by Josean Martinez

## Keyboard manager

Macos có thể vào setting chỉnh khỏi cài software. Một số keyboard bán có cho phép tải phân mềm về chỉnh firmware. Còn không thì re-map keys bằng software như karabiner trên Mac

Terminal emulator như kitty cũng có thể chỉnh key binding such as `macos_option_as_alt yes`

A Typical layout của Hàng spacebar: L-Ctrl - Window key - Alt/Command - **Spacebar** - Alt Gr/Command - System key - \[Application key] - R-Ctrl

**Super key** (or modifier key, mod key, system key) refer to the window key or the `Cmd` key on MacBook keyboard.\
Nếu xài mac quen thì có thể set super key thành Alt thay vì window key (tương ứng với Cmd bên layout bàn phím Mac)\
The Alt key is a different key entirely nhưng một số bàn phím sẽ gộp nó với Cmd key.

The **hyper key** is just one key that you can press that will act like you are holding the ⌃ (control), ⌥ (option), ⇧ (shift) and ⌘ (command) keys all down at the same time. This gives you a lot more shortcut choices because hardly anything uses this combo by default.

Mod1 (Alt), Mod4 (Super), and Mod5 (Hyper)

Other application-specific key name:\
- Leader key của neovim: spacebar

Application key: ?

Máy window copy dùng Ctrl C, V. Máy MacOS dùng Cmd C V (tương ứng với Alt của bàn phím window.)

Hàng Shift, Caps lock và TAB thì giống nhau ở các loại layout bàn phím.

You can remap escape to caps lock as well if you want to retain a caps lock button.

**Mappings**
- Tap Capslock -> Escape
- Hold Capslock -> Ctrl (easier to use Ctrl)

**Current problem**
- Tìm giải pháp upgrade cái tiling window manager super key. Hiện tại xài Cmd/Alt là cũng closest to the spacebar cũng khá ngon rồi.
- Tìm cách khôi phục chức năng toggle capslock đễ gõ Vietnamese có dấu mà không phải hold Capslock

[sxhkd](https://github.com/baskerville/sxhkd): Simple X hotkey daemon. This is more for running commands. Kmonad is more for keyboard re-mapping.

**Reference**

[wiki keyboard layout](https://en.wikipedia.org/wiki/Keyboard_layout)

[Stack exchange](https://unix.stackexchange.com/questions/119212/understanding-window-manager-terminology-mod-keys-meta-keys-and-key-naming-co): Understanding window manager terminology: Mod Keys, Meta Keys, and key naming conventions

## Shell Configuration & Environment Variables

Use `chsh` to change login shell. Phải biết path của shell trước.

**TODOs:**
- [x] PowerLevel 10k

### Bash

_Naming convention_: global **shell environment variables** are written in ALL CAPS. Local variables that you create yourself (like in your bash scripts) are written in lower case to distinguish them.

`myname="An Hao"` create new variable in your local shell session. Use `echo $HELLOMSG` to read the variable\
`export MY_VAR="Kim Phuong"` create new variable in the environment scope.

`echo "My name is $myname"` works as expected\
You can store file paths into variables `ls $MY_DIR`

Login Shell: `/etc/profile` -> `~/.bash_profile`

### Zsh

Use `printenv` or simply `env` to list all global environment variable\
`printenv <VARIABLE_NAME>` if you know the name of the variable you are looking for. Or you can `echo $<VARIABLE_NAME>`

[Single quotes won't interpolate anything, but double quotes will.](https://stackoverflow.com/questions/6697753/difference-between-single-and-double-quotes-in-bash) For example: $variables, backticks, certain `\` escapes, etc.

By default, Zsh will try to find the user’s configuration files in the `$HOME` directory. You can change it by setting the environment variable `$ZDOTDIR`.

`files=$(ls)` store the output of the `ls` command inside a variable. The `$()`, also called **sub-shell**, allows you to execute a command in the background

**Important environment variables:**

- `$XDG_CONFIG_HOME` = `$HOME/.config`
- `$ZDOTDIR` = `\~/.config/zsh`

**Zsh Config files**

`.zshenv` is _always_ sourced. It often contains exported variables that should be available to other programs. For example, `$PATH`, `$EDITOR`, and `$PAGER` are often set in `.zshenv`. Also, you can set `$ZDOTDIR` in `.zshenv` to specify an alternative location for the rest of your zsh configuration. The file `.zshenv` **needs to be in your home directory**. It’s where you’ll set `$ZDOTDIR`. Then, every file read after `.zshenv` can go into your `$ZDOTDIR` directory.

`.zshrc` (Zsh Run Control) is for **interactive shells**. You set options for the interactive shell there with the `setopt` and `unsetopt` commands. You can also load shell modules, set your history options, change your prompt, set up zle and completion, et cetera. You also set any variables that are only used in the interactive shell (e.g. `$LS_COLORS`). This file is best for adjusting the appearance and behavior of the shell, such as setting command aliases and adjusting the shell prompt.

`.zprofile` (Zsh Profile) is for **login shells**. It is basically the same as `.zlogin` except that it's sourced before `.zshrc` whereas `.zlogin` is sourced after `.zshrc`. According to the [zsh documentation](https://zsh.sourceforge.io/Doc/), "`.zprofile` is meant as an alternative to `.zlogin` for ksh fans; the two are not intended to be used together, although this could certainly be done if desired."

`.zlogin` is for login shells. It is sourced on the start of a login shell but after `.zshrc`, if the shell is also interactive. This file is often used to start X using `startx`. Some systems start X on boot, so this file is not always very useful.

`.zlogout` is sometimes used to clear and reset the terminal. It is called when exiting, not when opening.

You should go through [the configuration files of random Github users](https://github.com/search?q=zsh+dotfiles&ref=commandbar&type=repositories) to get a better idea of what each file should contain.

**zinit package manager**

`zinit zstatus`

Zinit này tự tải plugin về và lưu trong `~/.local/share/zinit/plugins/`

promt, PowerLevel 10k
- config file trong `~/.config/zsh/.p10k.zsh`
- Nó tự cài và chỉnh emulator xài nerd font MesloLGS NF

**References:**

[emac mode](https://gist.github.com/acamino/2bc8df5e2ed0f99ddbe7a6fddb7773a6) key binds trong zsh

[Configuring Zsh Without Dependencies blog post](https://thevaluable.dev/zsh-install-configure-mouseless/) mình có dùng nhiều kiến thức trong này

[video youtube](https://www.youtube.com/watch?v=ud7YxC33Z3w&t=591s) của Dream of Autonomy có giải thích vài syntax trong `.zsh` config file

## Terminal emulator

[official doc](https://code.visualstudio.com/docs/terminal/basics#_terminal-profiles) custom terminal emulator in vscode

Zoom in/out of terminal: `C +` or `C -`. Also work in terminal applications like NeoVim

## Nerd font

font are stored in `/Users/anhao/Library/Fonts`

Những chỗ cần chỉnh font:
- VS Code: vào setting

Tóm lại cài font bằng homebrew hay gì gì đó thôi là chưa đủ. Sau khi cài phải vào chỉnh cho nhưng phần mềm như VS Code, terminal emulator dùng cái font mình vừa cài nữa.

## Managing .dotfiles (GNU stow)

Bỏ hết configuration file vào `~/dotfiles` rồi dùng stow tạo symlink ra home directory.

If correctly installed, then running the command `stow --help` should list options to use Stow. Most used commands are:

[Cách dùng](https://stackoverflow.com/questions/72063495/stow-and-replace-existing-dotfiles) `--adopt` option để Stow and replace existing dotfiles

Khi mình change file content in the symlink thì original file also got modified.

```bash
stow <packagename> # activates symlink
stow -n <packagename> # trial runs or simulates symlink generation. Effective for checking for errors
stow -D <packagename> # delete stowed package
stow -R <packagename> # restows package
```

**References**

[GNU stow official doc](https://www.gnu.org/software/stow/manual/stow.html) coi mấy cái command arguments trong này

[Manage Your Dotfiles Like a Superhero](https://www.jakewiesler.com/blog/managing-dotfiles#understanding-stow)

[blog post](https://brandon.invergo.net/news/2012-05-26-using-gnu-stow-to-manage-your-dotfiles.html) by Brandon Invergo

[stack overflow](https://stackoverflow.com/questions/76054795/how-to-make-stow-ignore-ds-store-files) on How to make stow ignore DS_Store files using `~/.stow-global-ignore`

## Logi option+

`sudo pkill -9 -f logiop` [sourse](https://www.reddit.com/r/logitech/comments/17oz6sy/comment/kwjpcy6/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

## Terms

[X.Org Server](https://en.wikipedia.org/wiki/X.Org_Server) is the free and open-source implementation of the X Window System (X11) display server stewarded by the X.Org Foundation. 

