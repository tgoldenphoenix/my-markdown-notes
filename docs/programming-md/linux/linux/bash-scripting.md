# Shell Scripting (Bash, zsh, fish)

Bash, zsh

> If you figure out how to do something with the command ONCE, you can then do it thousands of times automatically.

A shell can best be compared with a way of talking to the computer, a language.

Khi xài linux phải biết save command, scripts for later use. Or you can save your `history` and re-factor it into a bash script.

Run `source ~/.zshrc` or `source ~/.bashrc` manually.

`sh` or Bourne Shell: the original shell still used on UNIX systems and in UNIX related environments. This is the basic shell, a small program with few features. When in POSIX-compatible mode, **bash** will emulate this shell.

Your default shell is set in the `/etc/passwd` file

To switch from one shell to another, just enter the name of the new shell in the active terminal. The system finds the directory where the name occurs using the `PATH` settings, and since a shell is an executable file (program), the current shell activates it and it gets executed. A new prompt is usually shown, because each shell has its typical appearance

If you don't know which shell you are using, either check the line for your account in `/etc/passwd` or type the command `echo $SHELL`

## zsh

`~/.config/zsh/`. Cái `~/.zshrc` bị override.

By default, Zsh will try to find the user’s configuration files in the `$HOME` directory. You can change it by setting the environment variable `$ZDOTDIR`.

Bash sẽ phục vụ công việc tốt hơn nếu làm nhiều SSH.
Zsh is easier to customize line editor than Bash
Use zsh, but don't use oh my zsh, configure yourself.

Useful environment variables.
Aliases.
The Zsh options.
The Zsh completion.
The Zsh prompt.
The Zsh directory stack.
How to configure Zsh to make it Vim-like.
How to add external plugins to Zsh.
External programs you can use to improve your Zsh experience.

diff $TMPDIR/.zshrc.mBj1SnoduX ~/.config/zsh/.zshrc

### Autocompletion

[Github](https://github.com/marlonrichert/zsh-autocomplete)

**References:**

[Configuring Zsh Without Dependencies](https://thevaluable.dev/zsh-install-configure-mouseless/)

[How Do Zsh Configuration Files Work?](https://www.freecodecamp.org/news/how-do-zsh-configuration-files-work/)

## Terminal Emulator (Kitty)

`sudo apt install kitty`

In the terminal:\
Zoom In : `Ctrl + Shift + +`
Zoom Out: `Ctrl + -`
Zoom 100%: `Ctrl+0`

**References:**

## Scripting ideas

Name images file.

## The basics

`echo $SHELL` to see which shell you are using

On Mac, to set bash as the default interactive shell instead of zsh type `chsh -s /bin/bash` then re-open your terminal. To turn back using zsh type `chsh -s /bin/zsh`

`which bash` will tell you where is bash on your computer. You can just type `/bin/bash` while using zsh to switch to bash (only work in current shell session)

Use `sudo chmod +x myscript.sh` to set permission for .sh files

`./myscript.zh` to run the script

Other shell: zsh, Xonsh, fish

`exec bash` to switch shell. This won't affect new terminal windows or anything, but it's convenient.\
`echo $0` to know which shell you are using.

## Redirection

1 'represents' stdout `>` and 2 stderr `2>`. 

### Output redirection & streams

`ls -l > file.txt` create new file/over-write existing file. Use `>>` to append
`cat file.txt |sort |uniq` do not show duplicates

There are three standard streams, `stdin` (said standard input or standard in), `stdout` (said standard output or standard out,) and `stderr` (said standard error or standard err.) `stdin` is an input stream to a program and the other two are output streams. `stdout` for all non-error text, the normal output. `stderr` is just for error information. The numerical designations for `stdin`, `stdout` and `stderr` are 0, 1 and 2 respectively.

`find / -name *.log 2> /dev/null` search by name at the root `/`, capture `stderr` and redirect those to `/dev/null` (purgatory)

`echo $?` the exit code of the previous command

## Bash Command history

`history` print bash history
`!594` execute command /#594 in history

Start your command with an empty space character and it will not be recorded into `history` (depend on Bash version)

## Basic math functions

Bash evaluate expression command `expr`

`*` in bash is a wildcard, not multiplication operator. So you have to type `expr 100 \* 4`

## If statements

When writing scripts, you don't want the script to ask you for confirmations. So you want to use options that eliminate any prompts that you can.

When you use square brackets `[]` in an if statement, bash assumes that you want to use the [test](https://www.ibm.com/docs/en/aix/7.2?topic=t-test-command) command

## Exit codes

`echo $?` the exit code of the previous command. 0 means succesful, other than 0 is failure.

The `exit` command in Bash is used with the syntax,`exit [n]`. Its function is to terminate a script and return a value to the parent script or shell. It’s a way to signal the end of a script’s execution and optionally return a status code to the calling process.

## The Shebang

Use `#!/usr/bin/bash` on Linux, use `#!/bin/bash` on MacOS. Use `which bash` to know what to use.

`printenv` and `env` là 1 command giống nhau. Thường dùng để: (1) print all shell environment variables and (2) in the shebang line.

[The Difference Between #!/usr/bin/bash and #!/usr/bin/env bash](https://www.baeldung.com/linux/bash-shebang-lines)

## Terminology

crontab: tasks that need to be executed periodically - backups, updates of the system databases, cleaning of the system, rotating logs etc. 

## Commands

`sudo /usr/bin/pip3 uninstall -y $(/usr/bin/pip3 list | tail -n +3 | awk '{print $1}')` chạy trên macos zsh

## References

[GNU Bash manual](https://www.gnu.org/software/bash/manual/)

guides on [TLDP.org](https://tldp.org/)