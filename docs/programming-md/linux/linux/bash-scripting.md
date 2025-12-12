# Bash Scripting

Bash, zsh

> If you figure out how to do something with the command ONCE, you can then do it thousands of times automatically.

A shell can best be compared with a way of talking to the computer, a language.

Khi xài linux phải biết save command, scripts for later use. Or you can save your `history` and re-factor it into a bash script.

Run `source ~/.zshrc` or `source ~/.bashrc` manually.

`sh` or Bourne Shell: the original shell still used on UNIX systems and in UNIX related environments. This is the basic shell, a small program with few features. When in POSIX-compatible mode, **bash** will emulate this shell.

Your default shell is set in the `/etc/passwd` file

To switch from one shell to another, just enter the name of the new shell in the active terminal. The system finds the directory where the name occurs using the `PATH` settings, and since a shell is an executable file (program), the current shell activates it and it gets executed. A new prompt is usually shown, because each shell has its typical appearance

If you don't know which shell you are using, either check the line for your account in `/etc/passwd` or type the command `echo $SHELL`

In the shell prompt `jake@pine $`, `jake` is user name, `pine` is the computer name.

---

There are several ways to get to a shell interface in Linux. Three of the most common are the shell prompt, Terminal window, and virtual console.

If your Linux system has no graphical user interface (or one that isn’t working at the moment), you will most likely see a `shell prompt` after you log in.

With the desktop GUI running, you can open a `Terminal emulator` program (sometimes referred to as a Terminal window) to start a shell.

Most Linux systems that include a desktop interface start multiple virtual consoles running on the computer. Virtual consoles are a way to have multiple shell sessions open at once in addition to the graphical interface you are using.

You can switch between virtual consoles by holding the Ctrl and Alt keys and pressing a function key between F1 and F6. The GUI is typically located on one of the first two virtual consoles, and the other six virtual con-soles are typically text-based virtual consoles.

---

To find out what is your default login shell, enter the following commands:

```bash
$ whoami
chris   pts/0        2019-10-21 22:45 (:0.0)
$ grep chris /etc/passwd
chris:x:13597:13597:Chris Negus:/home/chris:/bin/bash
```

To try a different shell, simply type the name of that shell (examples include `ksh`, `tcsh`, csh, sh, dash, and others, assuming that they are installed). You can try a few commands in that shell and type `exit` when you are finished to return to the bash shell.

## zsh - The Z shell

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

`.zcompdump` refers to the Zsh completion cache file. This file is a cached version of the completion functions found and initialized by the compinit command when starting a Zsh session.

The `compinit` command, which initializes Zsh's programmable completion system, is responsible for creating and updating the zcompdump file.

By default, zcompdump files are typically found in your home directory (e.g., `~/.zcompdump-<hostname>-<zsh-version>`). However, their location can be customized using the compinit -d <dumpfile> option or by setting the `ZSH_COMPDUMP` environment variable.

## Terminal Emulator (Kitty)

`sudo apt install kitty`

- In the terminal:
  - Zoom In : `Ctrl + Shift + +`
  - Zoom Out: `Ctrl + -`
  - Zoom 100%: `Ctrl+0`

## Scripting ideas

Name images file.

## The Basics

`Ctrl Alt t` open the terminal

`bash` is great for simple scripts that automate things you’d otherwise be typing on the command line. But once a bash script gets above a hundred lines or you need features that bash doesn’t have, it’s time to move on to Perl or Python.

`echo $SHELL` to see which shell you are using

On Mac, to set bash as the default interactive shell instead of zsh type `chsh -s /bin/bash` then re-open your terminal. To turn back using zsh type `chsh -s /bin/zsh`

`which bash` will tell you where is bash on your computer. You can just type `/bin/bash` while using zsh to switch to bash (only work in current shell session)

Use `sudo chmod +x myscript.sh` to set permission for .sh files

`./myscript.zh` to run the script

Other shell: zsh, Xonsh, fish

`exec bash` to switch shell. This won't affect new terminal windows or anything, but it's convenient.  
`echo $0` to know which shell you are using.

To prepare the file for running, just turn on its execute bit:

```bash
$ chmod +x helloworld 
$ ./helloworld3 
Hello, world!

chmod +x upgrade.sh
```

If your shell understands the command `helloworld` without the `./` prefix, that means the current directory (`.`) is in your search path.

You can also invoke the shell as an interpreter directly :

```bash
$ bash helloworld 
Hello, world! 
$ source helloworld 
Hello, world!
```

The first command runs `helloworld` in a new instance of `bash`, and the second makes your existing login shell read and execute the contents of the file. The latter option is useful when the script sets up environment variables or makes other customizations that apply only to the current shell. It’s commonly used in script-ing to incorporate the contents of a configuration file written as a series of bash variable assignments.

The “dot” command is a synonym for `source`, e.g., `. helloworld`.

If you wish, you can give your bash scripts a `.sh` suffix to remind you what they are, but you’ll then have to type out the `.sh` when you run the command, since UNIX doesn’t treat extensions specially.

Save your scripts inside `~/bin` or `/usr/local/bin` & make the files executable.

- Quy trình viết bash scripts:
  * Develop the script (or script component) as a pipeline, one step at a time, entirely on the command line. Không viết bash script trong text editor giống như viết code python.
  * Send output to standard output and check to be sure it looks right (using `echo`).
  * At each step, use the shell’s command history to recall pipelines and the shell’s editing features to tweak them.
  * Until the output looks right, you haven’t actually done anything, so there’s nothing to undo if the command is incorrect.
  * Once the output is correct, execute the actual commands and verify that they worked as you intended.
  * Use `fc` to capture your work, then clean it up and save it.

bash’s built-in command `fc` is a lot like `<Control-P>`, but instead of returning the last command to the command line, it transfers the com-mand to your editor of choice.

## Linux Globbing

`grep` uses regular expressions (regex).  
The filename matching and expansion performed by the shell when it interprets command lines such as `wc -l *.pl` is not a form of regex matching. It’s a different system called **shell globbing,** and it uses a different and simpler syntax.

Glob mean "global commands". [/etc/glob](https://en.wikipedia.org/wiki/Glob_(programming)) is a program in UNIX V6 that would expand wildcard patterns. Soon afterward, this became a shell built-in.

Globbing is mainly used to match filenames or searching for content in a file. Globbing uses wildcard characters to create the pattern.

Globbing is used in config files such as a `.gitignore` where you might see `.cache/*`, for example.

Globbing is the expansion of simple pattern-matching characters such as * and ? to form filenames or lists of file-names)

Globbing is an operation that is performed by the shell itself and it happens **independently** of the actual command we're running. The shell will **first** attemp to expand the wildcard pattern to match any files that are present before passing their expanded names to the program we want to run.  
`man 7 glob` to read more.

You should be aware that the value of the arguments that get passed to a given program will actually depend on the content of your file system. Your program may be passed a different number of arguments depending on how many files match the wildcard.

We should note that while globbing might look similar to regular expressions, they’re fundamentally different. While the patterns seem similar, globbing doesn’t use regular expressions.

- We use shell-style globbing characters for pattern matching:
  * A star (`*`) matches zero or more characters.
  * A question mark (`?`) matches any single character. You can use `?` for multiple times for matching multiple characters.
  * A tilde or “twiddle” (`~`) means the home directory of the current user
  * `~user` means the home directory of user.

For example, we might refer to the startup script directories `/etc/rc0.d`, `/etc/rc1.d`, and so on with the shorthand pattern `/etc/rc*.d`.

- `[]` range of characters. For example `[0-9]` or `[abc]`. Ranges can apply to letters as well as digits:
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

Examples:

- `ls *.txt`
- `ls test-?.txt` => list all text files named ‘test-‘ followed by a single digit
- `ls ????.txt` => all text files with a name of exactly four characters
- `ls *[0-9]*` => all files with a number in their name

move all the contents of the current directory to some other location:

`$ mv * /some/other/directory/`

To move only files with names partially matching a particular sequence, try this:

`$ mv file* /some/other/directory/`

This command moves all files whose names begin with the letters file, but leaves everything else untouched. If you had files named file1, file2...file15 and wanted to move only those between file1 and file9, you’d use the question mark (`?`) instead of the asterisk: 

`$ mv file? /some/other/directory/`

The question mark applies an operation to only those files whose names contain the letters file and one other character. It would leave file10 through file15 in the current directory.

---

Single vs. Double Quotes

- Single quotes provide the strongest form of quoting (also called literal quoting). They prevent the shell from performing any interpretation or expansion of the characters contained within them.
- Double quotes generally preserve the literal value of characters, but they allow for variable expansion, globbing (the expansion of filename-matching metacharacters such as `*` and ?), command substitution, and certain escape sequences.

**Back-ticks**, are treated similarly to double quotes, but they have the additional effect of executing the contents of the string as a shell command and replacing the string with the command’s output.

```bash
$ echo "There are `wc -l /etc/passwd` lines in the passwd file." 
There are 28 lines in the passwd file.
```

`'!$"*<>` is typically the order of precedence for those symbols.

Đôi khi phải dùng single quote to pass unchanged wildcard pattern to programs. Prevent the shell to expand them before passign to programs.

```bash
$ cat big name
cat: big: No such file or directory
cat: name: No such file or directory

$ cat 'big name'
Hello world
```

Use double quote `""` to prevent Bash from splitting the variable's value into multiple words if it already contains spaces.

## Command Separators
 
You can also put more than one statement on a line by separating the statements with semicolons `;` (command separator, Sequential List).  
The return status (exit code) of the command preceding the semicolon is **ignored**, and the next command always runs.

- `git checkout main; git pull` => chạy 2 cái 1 dòng
- `git checkout main && git pull` => Execute the next command ONLY if the previous command was completely successful (exited with a zero status code).

`ls -l ; pwd ; date`

```
❯ cd /nonexistent_dir ; echo "I still run"
zoxide: no match found
I still run
```

---

- `&&` Executes the next command ONLY if the previous command succeeded (exit code 0).
- `||` là gì?

---

- As on the command line, you can break a single logical line onto multiple physical lines by escaping the newline with a backslash (`\`). Make sure there are no characters (including a space) after the backslash. That’s guaranteed to cause you grief.
  * Trong `LaTeX` thì `\` là syntax của commands.

## Shell Expansion

Command Substitution

With command substitution, you can have the output of a command interpreted by the 
shell instead of by the command itself. In this way, you can have the standard output of a command become an argument for another command. The two forms of command substitution are `$(command)` and `command` (backticks, not single quotes).

`$ vi $(find /home | grep xyzzy)`

---

Expanding arithmetic expressions

Sometimes, you want to pass arithmetic results to a command. There are two forms that 
you can use to expand an arithmetic expression and pass it to the shell: `$[expression]` 
or `$(expression)`. The following is an example:

```bash
$ echo "I am $[2020 - 1957] years old."
I am 63 years old.
```

## Background Commands

Some commands can take a while to complete. Sometimes, you may not want to tie up your shell waiting for a command to finish. In those cases, you can have the commands run in the background by using the ampersand (`&`).

Don’t close the shell until the process is completed or that kills the process.

## `xargs`

`xargs` reads items from standard input (usually separated by spaces or newlines) and builds and executes command lines using those items as arguments.

## Input & Output

The `echo` command is crude but easy. For more control over your output, use `printf`. It is a bit less convenient because you must explicitly put newlines where you want them (use “`\n`”), but it gives you the option to use tabs and enhanced number formatting in your the output.

```bash
$ echo "\taa\tbb\tcc\n" 
\taa\tbb\tcc\n 
$ printf "\taa\tbb\tcc\n"
    aa   bb   cc
```

You can use the read command to prompt for input. Here’s an example:

```bash
#!/bin/bash
echo -n "Enter your name: " 
read user_name
if [ -n "$user_name" ]; then 
echo "Hello $user_name!" 
exit 0
else
echo "You did not tell me your name!" exit 1
fi
```

The `-n` in the echo command suppresses the usual newline, but you could also have used printf here. We cover the if statement’s syntax shortly, but its effect should be obvious here. The `-n` in the if statement evaluates to true if its string argument is not null.

## Command-line arguments and functions

Command-line arguments to a script become variables whose names are num-bers. `$1` is the first command-line argument, `$2` is the second, and so on. $0 is the name by which the script was invoked. That could be something strange such as `../bin/example.sh`, so it’s not a fixed value.

The variable `$#` contains the number of command-line arguments that were sup-plied, and the variable `$*` contains all the arguments at once. Neither of these vari-ables includes or counts $0.

Không có khai báo bao nhiêu argument & types. Cứ dùng mấy cái ở trên để check.

### Exit codes

`echo $?` the exit code of the previous command. 0 means succesful, other than 0 (nonzero) is failure.

The `exit` command in Bash is used with the syntax,`exit [n]`. Its function is to terminate a script and return a value to the parent script or shell. It’s a way to signal the end of a script’s execution and optionally return a status code to the calling process.

You can define useful functions in your `~/.bash_profile` file and then use them on the command line as if they were commands. For example, if your site has standardized on net-work port 7988 for the SSH protocol (a form of “security through obscurity”), you might define

```bash
function ssh { 
/usr/bin/ssh -p 7988 $*
```

in your `~/.bash_profile`to make sure ssh is always run with the option `-p 7988`. 

Like many shells, bash has an `alias` mechanism that can reproduce this limited example even more concisely, but functions are more general and more powerful. Forget aliases and use functions.

## Output Redirection, Streams & Pipes

- Every process has at least three standard communication channels (streams) available to it:
  - `0: STDIN` (said standard input or standard in)
  - `1: STDOUT` (said standard output or standard out) for all non-error text, the normal output
  - `2: STDERR` (said standard error or standard err.) is just for error information
- The kernel sets up these channels on the process’s behalf, so the process itself doesn’t necessarily know where they lead. They might connect to a terminal win-dow, a file, a network connection, or a channel belonging to another process, to name a few possibilities.
- Most commands accept their input from STDIN and write their output to STD-OUT. They write error messages to STDERR. This convention lets you string commands together like building blocks to create composite pipelines.

- A `<` symbol connects the command’s STDIN to the contents of an existing file.
- The `>` and `>>` symbols redirect STDOUT:
  - `>` replaces the file’s existing contents
  - `>>` appends to the existing conntent.
- To redirect both STDOUT and STDERR to the same place, use the `>&` symbol. To redirect STDERR only, use `2>`.
- To connect the STDOUT of one command to the STDIN of another, use the pipe (`|`) symbol

`echo $?` the exit code of the previous command

To execute a second command only if its precursor completes successfully (yielding an exit code of zero), you can separate the commands with an `&&` symbol.  
For example `lpr /tmp/t2 && rm /tmp/t2` removes `/tmp/t2` if and only if it is successfully queued for printing.  
Conversely, the `||` symbol executes the following command only if the preceding command fails (produces a **nonzero exit status**).

In a script, you can use a backslash (`\`) to break a command onto multiple lines, help-ing to distinguish the error-handling code from the rest of the command pipeline:

```bash
cp --preserve --recursive /etc/* /spare/backup \
|| echo "Did NOT make backup"
```

For the converse effect—multiple commands combined onto one line—you can use a semicolon (`;`) as a statement separator.

## `tee`

> copy input to two places

split off a stream so that its output can be simultaneously sent to a file and stdout. Named after the T-splitter in plumping.

Debug why a complicated shell pipe ins't working. Dùng để tạo `.log` files for debugging shell pipes.

The device `/dev/tty` is a synonym for the current terminal. For example,

```bash
find / -name core | tee /dev/tty | wc -l
```

prints both the pathnames of files named core and a count of the number of core files that were found.

## Variables

Do not put spaces around the `=` symbol or the shell will mistake your variable name for a command name.

```bash
$ etcdir='/etc'
$ echo $etcdir
/etc
```

You can surround variable with curly braces `{}`:

```bash
$ echo "Saved ${rev}th version of mdadm.conf." 
Saved 8th version of mdadm.conf.
```

Local variables are all lowercase & snake_case (case sensitive).

Some environment variables, such as `PWD` for the current working directory, are maintained automatically by the shell.

The shell treats strings enclosed in single and double quotes similarly, except that **double-quoted** strings are subject to globbing (the expansion of filename-match-ing metacharacters such as `*` and ?) and variable expansion. For example:

```bash
$ mylang="Pennsylvania Dutch" 
$ echo "I speak ${mylang}."
I speak Pennsylvania Dutch. 
$ echo 'I speak ${mylang}.'
I speak ${mylang}.
```

Back quotes, also known as back-ticks, are treated similarly to double quotes, but they have the additional effect of executing the contents of the string as a shell command and replacing the string with the command’s output. For example,

```bash
$ echo "There are `wc -l /etc/passwd` lines in the passwd file." 
There are 28 lines in the passwd file.
```

### Variable Scope

Variables are global within a script, but functions can create their own lo cal vari-ables with a `local` declaration.

## Alias

`alias` list all alias in your terminal

`unalias <alias>` remove an alias

## Bash Command history

`history` print bash history  
`!594` execute command /#594 in history

Start your command with an empty space character and it will not be recorded into `history` (depend on Bash version)

## Basic math functions

Bash evaluate expression command `expr`

`*` in bash is a wildcard, not multiplication operator. So you have to type `expr 100 \* 4`

## Flow Control

When writing scripts, you don't want the script to ask you for confirmations. So you want to use options that eliminate any prompts that you can.

When you use square brackets `[]` in an if statement, bash assumes that you want to use the [test](https://www.ibm.com/docs/en/aix/7.2?topic=t-test-command) command.

The terminator for an if statement is `fi`. To chain your `if` clauses, you can use the `elif` keyword to mean “else if.”

```bash
if [ $base -eq 1 ] && [ $dm -eq 1 ]; then 
installDMBase
elif [ $base -ne 1 ] && [ $dm -eq 1 ]; then 
installBase
elif [ $base -eq 1 ] && [ $dm -ne 1 ]; then 
installDM
else
echo '==> Installing nothing'
fi
```

Both the peculiar `[]` syntax for comparisons and the command-line optionlike names of the **integer comparison operators** (e.g., `-eq`) are inherited from the original Bourne shell’s channeling of `/bin/test`. The brackets are actually a shorthand way of invoking `test` and are not a syntactic requirement of the if statement.

The table below shows the bash comparison operators for numbers and strings. bash uses textual operators for numbers and symbolic operators for strings, exactly the opposite of Perl.

| String | Numeric | True if                         |
|--------|---------|---------------------------------|
| x = y  | x -eq y | x is equal to y                 |
| x != y | x -ne y | x is not equal to y             |
| x < y  | x -lt y | x is less than y                |
| x <= y | x -le y | x is less than or equal to y    |
| x > y  | x -gt y | x is greater than y             |
| x >= y | x -ge y | x is greater than or equal to y |
| -n x   | -       | x is not null                   |
| -z x   | -       | x is null                       |

`bash` shines in its options for evaluating the properties of files (again, courtesy of its `/bin/test` legacy). Table below shows a few of bash’s many file-testing and file-comparison operators.

| Operator        | True if                           |
|-----------------|-----------------------------------|
| -d file         | file exists and is a directory    |
| -e file         | file exists                       |
| -f file         | file exists and is a regular file |
| -r file         | You have read permission on file  |
| -s file         | file exists and is not empty      |
| -w file         | You have write permission on file |
| file1 -nt file2 | file1 is newer than file2         |
| file1 -ot file2 | file1 is older than file2         |

Ngoài `elif` thì `bash` còn có `case` giống như switch-case.

---

Changes the directory to `/var/backups/`. If no such directory exists, it exits the script and issues an exit status code of 0, which signifies the command was successful: 

`cd /var/backups || exit 0` 

The `||` sequence (sometimes known as a double pipe) can be read as though it’s the word _or_. So this line means: either change directory to /var/backups/ or exit the script. If everything goes according to plan, subsequent script operations will take place in the /var/backups/ directory.

–-

`[[ ... ]]` is the preferred, modern, and safer way to perform conditional tests in Bash (compared to the older single brackets `[ ... ]`).

## Loops

Có `for...in` loop.

Có traditional `for` loop.

```bash
for (( i=0 ; i < $CPU_COUNT ; i++ )); do
    CPU_LIST="$CPU_LIST $i" 
done
```

Any whitespace-separated list of things, including the contents of a vari-able, works as a target of `for…in`. 

Câu lệnh này nếu không có `;` thì không được.

```bash
for i in {1..10}; do sleep 1; done
```

Nếu không dùng `;` thì phải enter xuống dòng.

```bash
❯ for fruit in apple banana cherry
∙ do
∙     echo "I love to eat the $fruit."
∙ done
I love to eat the apple.
I love to eat the banana.
I love to eat the cherry.
```

Có `while` loop

## Scheduling regular backups with `cron`

k

### Scheduling irregular backups with anacron

Those cron-based tools all work well for computers (like production servers) that are likely to be left running all the time. But what about executing important jobs on, say, your laptop, which is often turned off? It’s all very nice telling cron (or cron.daily and so forth) to back up your files at 5:21 on a Monday morning, but how likely is it that you’ll remember to get up to boot your laptop in time? Exactly—not likely at all. It’s time to talk about anacron. 

The one cron file we haven’t yet discussed is `/etc/anacrontab`, which is where you schedule operations to run at a set time after each system boot.

### Scheduling regular backups with systemd timers 

systemd timers come with some significant advantages, including deeper integration with other system services (including logs) and the ability to execute commands based on changes to system state (for instance, someone connecting a USB device), rather than just set times. 

## The Shebang

Use `#!/usr/bin/bash` on Linux, use `#!/bin/bash` on MacOS. Use `which bash` to know what to use.

`printenv` and `env` là 1 command giống nhau. Thường dùng để: (1) print all shell environment variables and (2) in the shebang line.

[The Difference Between #!/usr/bin/bash and #!/usr/bin/env bash](https://www.baeldung.com/linux/bash-shebang-lines)

## Environment Variables & Viewing Machine specs

`echo $0` to know which shell you are using

On Linux

On MacOS

Use the `printenv` command to display a list of currently set environment variables. Trong này sẽ có `$PATH`, `$HOME` và nhiều cái khác.

Hiển thị one environment variable: `echo $[variable name]`

`$PATH` environment variable stores a list of directories with executable files, and thus saves the user a lot of typing and memorizing locations of commands.

If you want to display the complete list of shell variables, use the `set` command. The output is very long.

`$HOME`: /Users/anhao

The value you assign to a **temporary** environment variable only lasts until you close the terminal session. This is useful for variables you need to use for one session only or to avoid typing the same value multiple times.

Assign a temporary environment variable with the `export` command. The export command also allows you to add new values to existing environment variables with the `:$` syntax.

**Permanent** environment variables are added to the `.bash_profile` file. Cũng dùng `export` command. Execute the new `.bash_profile` by either restarting the terminal window or using `source ~/.bash-profile`

Use the`unset` command to remove an environment variable.

The primary use of the `eval` command  is to interpret and execute dynamic or complex commands stored in strings or variables. This allows you to generate and run commands dynamically.

References

[how to add folder to PATH](https://phoenixnap.com/kb/linux-add-to-path) by PHOENIXNAP very informative

## Terminology

crontab: tasks that need to be executed periodically - backups, updates of the system databases, cleaning of the system, rotating logs etc.

## Commands

`sudo /usr/bin/pip3 uninstall -y $(/usr/bin/pip3 list | tail -n +3 | awk '{print $1}')` chạy trên macos zsh

## References

[GNU Bash manual](https://www.gnu.org/software/bash/manual/)

guides on [TLDP.org](https://tldp.org/)
