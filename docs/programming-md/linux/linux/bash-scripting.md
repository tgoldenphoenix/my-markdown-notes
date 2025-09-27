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

- In the terminal:
  - Zoom In : `Ctrl + Shift + +`
  - Zoom Out: `Ctrl + -`
  - Zoom 100%: `Ctrl+0`

## Scripting ideas

Name images file.

## The basics

`echo $SHELL` to see which shell you are using

On Mac, to set bash as the default interactive shell instead of zsh type `chsh -s /bin/bash` then re-open your terminal. To turn back using zsh type `chsh -s /bin/zsh`

`which bash` will tell you where is bash on your computer. You can just type `/bin/bash` while using zsh to switch to bash (only work in current shell session)

Use `sudo chmod +x myscript.sh` to set permission for .sh files

`./myscript.zh` to run the script

Other shell: zsh, Xonsh, fish

`exec bash` to switch shell. This won't affect new terminal windows or anything, but it's convenient.  
`echo $0` to know which shell you are using.

## Output redirection, streams & pipes

There are three standard streams, `stdin` (said standard input or standard in), `stdout` (said standard output or standard out,) and `stderr` (said standard error or standard err.) `stdin` is an input stream to a program and the other two are output streams. `stdout` for all non-error text, the normal output. `stderr` is just for error information. The numerical designations for `stdin`, `stdout` and `stderr` are 0, 1 and 2 respectively.

- 0: STDIN
- 1: STDOUT
- 2: STDERR

- A `<` symbol connects the command’s STDIN to the contents of an existing file.
- The `>` and `>>` symbols redirect STDOUT:
  * `>` replaces the file’s existing contents
  * `>>` appends to them.
- To redirect both STDOUT and STDERR to the same place, use the `>&` symbol. To redirect STDERR only, use `2>`.

`echo "This is a test message." > /tmp/mymessage` => stores a single line in the file /tmp/mymessage, creating the file if necessary. The command below emails the contents of that file to user johndoe.  
`mail -s "Mail test" johndoe < /tmp/mymessage`

`ls -l > file.txt` create new file/over-write existing file. Use `>>` to append
`cat file.txt |sort |uniq` do not show duplicates

`find / -name core 2> /dev/null` => only print STDOUT to the screen, re-direct all the “permission denied” error messages to null.

`echo $?` the exit code of the previous command

To connect the STDOUT of one command to the STDIN of another, use the pipe (`|`) symbol

To execute a second command only if its precursor completes successfully (yielding an exit code of zero), you can separate the commands with an `&&` symbol.  
Conversely, the `||` symbol executes the following command only if the preceding command fails (produces a nonzero exit status).

In a script, you can use a backslash (`\`) to break a command onto multiple lines. For the converse effect—multiple commands combined onto one line—you can use a semicolon (`;`) as a statement separator.

## Globbing

Globbing is mainly used to match filenames or searching for content in a file. Globbing uses wildcard characters to create the pattern. The most common wildcard characters that are used for creating globbing patterns are described below.

Glob mean "global commands". [/etc/glob](https://en.wikipedia.org/wiki/Glob_(programming)) is a program in UNIX V6 that would expand wildcard patterns. Soon afterward, this became a shell built-in.

Globbing is used in config files such as a `.gitignore` where you might see `.cache/*`, for example

Globbing is an operation that is performed by the shell itself and it happens independently of the actual command we're running. The shell will first attemp to expand the wildcard pattern to match any files that are present before passing their expanded names to the program we want to run.  
`man 7 glob` to read more

You should be aware that the value of the arguments that get passed to a given program will actually depend on the content of your file system. Your program may be passed a different number of arguments depending on how many files match the wildcard.

We should note that while globbing might look similar to regular expressions, they’re fundamentally different. While the patterns seem similar, globbing doesn’t use regular expressions.

`*` represents any string of any given length (zero or more chars)

`?` match exactly one character

`[]` range of characters. For example `[0-9]` or `[abc]`. Ranges can apply to letters as well as digits:

- `[a-z]` = all lowercase characters of the alphabet
- `[A-Z]` = all uppercase characters of the alphabet
- `[a-zA-Z]` = all characters of the alphabet, irrespective of their case
- `[j-p]` = lowercase characters j, k, l, m, n, o or p
- `[a-z3-6]` = lowercase characters or the numbers 3, 4, 5 or 6

`[]` is used to match the character from the range. Some of the mostly used range declarations are mentioned below.

- All uppercase alphabets are defined by the range as, `[:upper:] or [A-Z]` .
- All lowercase alphabets are defined by the range as, \[:lower:] or \[a-z].
- All numeric digits are defined by the range as, \[:digit:] or \[0-9].
- All uppercase and lower alphabets are defined by the range as, \[:alpha:] or \[a-zA-z].
- All uppercase alphabets, lowercase alphabet and digits are defined by the range as, \[:alnum:] or \[a-zA-Z0-9]

By default, **hidden files and folders** don’t show up in the output of ls. To apply globbing to our hidden files and folders, we have to explicitly add a leading `.` (dot).  
`ls *` vs `ls .*`

**Examples:**

`ls *.txt`

`ls test-?.txt` => list all text files named ‘test-‘ followed by a single digit

`ls ????.txt` => all text files with a name of exactly four characters

`ls *[0-9]*` => all files with a number in their name

The shell treats strings enclosed in single and double quotes similarly, except that double-quoted strings are subject to globbing (the expansion of filename-match-ing metacharacters such as * and ?) and variable expansion.

- We use shell-style globbing characters for pattern matching:
  * A star (*) matches zero or more characters.
  * A question mark (?) matches any single character. You can use `?` for multiple times for matching multiple characters.
  * A tilde or “twiddle” (~) means the home directory of the current user

### Single vs. Double Quotes

Single and double quotes are often used in Linux bash commands or scripts, especially when dealing with filenames. Although both quote types prevent globbing and word splitting, it is important to pay attention to the quotes you use. The differences between the quote types make them noninterchangeable in some cases.

`'!$"*<>` is typically the order of precedence for those symbols.

Đôi khi phải dùng single quote to pass unchanged wildcard pattern to programs. Prevent the shell to expand them before passign to programs.

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

