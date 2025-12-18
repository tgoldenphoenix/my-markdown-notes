# Basic Linux Surviving

## Command Syntax

- Command option: `-h`, `-c`, `--help`
- Command arguments: input for the command, không có hyphen (`-`). Example: file names, directory, username, device, or other item.

Sometimes, an argument is associated with an option. In that case, the argument must immediately follow the option. With single-letter options, the argument typically follows after a space. For full-word options, the argument often follows an equal sign (=). Here are some examples:

```bash
$ ls --hide=Desktop
Documents  Music     Public    Videos
Downloads  Pictures  Templates
```

The `--hide` option tells the ls command not to display the file or directory named Desktop when listing the contents of the directory.

Here’s an example of a single-letter option that is followed by an argument:

`$ tar -cvf backup.tar /home/chris`

In the tar example just shown, the options say to create (`c`) a file (`f`) named `backup.tar` that includes all of the contents of the `/home/chris` directory and its subdirectories and show verbose (`v`) messages as the backup is created. Because `backup.tar` is an argument to the `f` option, `backup.tar` must immediately follow the option.

## Basic Commands

`./a.out` => run files

`echo tran kim phuong`

`whoami`

---

```bash
$ date
Thu Jun 29 08:14:53 EDT 2019

$ date +'%d/%m/%y'
04/03/20

$ date +'%A, %B %d, %Y'
Wednesday, March 04, 2020
```

`cal` calender

---

```bash
$ hostname
mydesktop
```

`gedit` is an UI text editor on linux

`pwd` display present working directory

`clear` (alias `c`)

`diff` compare the contents of two files and display the differences between them.

On Ubunto, `Ctrl Alt f1` till `f6` to enter [virtual terminal](https://en.wikipedia.org/wiki/Virtual_console). To come back to the graphical session, press `Ctrl Alt f7`

`tty` print the file name of the terminal connected to standard input.

To write comments in the shell, use the `#` symbol. The `#` can also be used to signify running a command as `root`. 

`pushd`: `cd` and then push the new directory to the top of the directory stack (on the leftmost of `dirs` or the top-most of `dirs -p`)

The `cron` command-line utility is a job scheduler on Unix-like operating systems. Users who set up and maintain software environments use cron to schedule jobs (commands or shell scripts), also known as cron jobs, to run periodically at fixed times, dates, or intervals. It typically automates system maintenance or administration—though its general-purpose nature makes it useful for things like downloading files from the Internet and downloading email at regular intervals.

`passwd` change the password for the current user

`file FILENAME` display file type of file with name `filename`

`apropos STRING` search the whatis database for strings

In most cases, when issuing a command or starting a program as a non-privileged user, the system will warn you or prompt you for the root password when root access is required. Once you're done, _leave_ the application or session that gives you root privileges _immediately_.

`Alt` == meta key

---

The `uname` command (short for "Unix Name") is a standard utility in Unix-like operating systems (Linux, macOS, BSD, etc.) used to display fundamental information about the machine's operating system, kernel, and hardware architecture.

```bash
$ uname -a
Linux mydesktop 5.3.7-301.fc31.x86_64 #1 SMP Mon Oct 21 19:18:58 UTC      2019 x86_64 x86_64 x86_64 GNU/Linux
```

---

When you log in to a Linux system, Linux views you as having a particular identity, which includes your username, group name, user ID, and group ID. Linux also keeps track of your login session: It knows when you logged in, how long you have been idle, and where you logged in from.

To find out information about your identity, use the id command as follows:

```bash
$ id
uid=1000(chris) gid=1000(chris) groups=1005(sales), 7(lp)
```

In this example, the username is chris, which is represented by the numeric user ID (uid) 1000. The primary group for chris also is called chris, which has a group ID (gid) of 1000. It is normal for Fedora and Red Hat Enterprise Linux users to have the same primary group name as their username. The user chris also belongs to other groups called sales (gid 1005) and lp (gid 7).

You can see information about your current login session by using the `who` command. In the following example, the `-u` option says to add information about idle time and the process ID and `-H` asks that a header be printed:

```bash
$ who -uH
NAME      LINE    TIME             IDLE     PID   COMMENT
chris     tty1    Jan 13 20:57     .        2019
```

The output from this who command shows that the user chris is logged in on tty1 (which is the first virtual console on the monitor connected to the computer) and his login session began at 20:57 on January 13. The IDLE time shows how long the shell has been open without any command being typed (the dot indicates that it is currently active).  PID shows the process ID of the user’s login shell. COMMENT would show the name of the remote computer from which the user had logged in, if that user had logged in from another computer on the network, or the name of the local X display if that user were using a Terminal window (such as :0.0).

---

Ctrl-d vs exit command

`Ctrl+D` Sends the End-of-File (EOF) character or signal. When a shell (like Bash) receives an EOF signal on an empty command line, it interprets it as a signal to terminate the current session.

`exit` is an explicit shell command that tells the shell to terminate immediately.

How about `Ctrl c` and `Ctrl z`?

---

`df` (which stands for _disk full_ or _disk free_) show how much of your disk is still free; information about the partitions and their mount points\
supports the `-h` or _human readable_ option which greatly improves readability

How can you find out which partition a directory is on? Using the `df` command with a dot (.) as an option shows the partition the current directory belongs to, and informs about the amount of space used on this partition

The `df` command only displays information about active non-swap partitions. These can include partitions from other networked systems

since the search path contains only paths to directories containing executable programs, **which** doesn't work for ordinary files. The **which** command is useful when troubleshooting "Command not Found" problems.\
Using the `which` command also checks to see if a command is an alias for another command `which -a ls`. If this does not work on your system, use the alias command: `alias ls`

## Moving in the Terminal Prompt

Emac mode shell key bindings:

- `^b` & `^f` move backward/forward one char
- `Alt b/f` move backward/forward one word
- `^a` & `^e` go to start/end of prompt
- `^p` & `^n` steps backward/forward through commands in `history`.
- `^r` searches incrementally through your history to find old commands.
- `^k` delete from cursor to end of line
- `^h` delete the previous character (equal to backspace)
- `^w` delete the previous word; `alt d` Cut the word following the cursor (delete next word).
- `^u` delete the whole line
- `^l` == `clear`; clear screen
- `^d`, `exit` or `logout` Log out of the current shell session
- `^c` End a running program and return the prompt.
- `^z` Suspend a program

- `^d` delete current character (conflict)
- `^t` transpose character: Switch positions of current and previous characters
- `Alt t` transpose word: Switch positions of current and previous words
- `alt u` Change the current word to uppercase.
- `alt l` Change the current word to lowercase.
- Alt+C Capitalize word Change the current word to an initial capital.

- conflict mapping: `^a, ^f`

Many of these key binds can also be used in vim insert mode, command-line mode.

`cd` between two directories with long path names. Go back to the last directory that you were in `cd -`. To go back even further, use `pushd` and `popd`. `cd` in deeply nested file system.

`/` to enter search mode. `n` to go jump to the next match (spam `n` as fast as you can). `Shift N` to jump back up.  
Works in man pages, vim, less, git logs. This is because programs like man pages and git actually use `less` as their **text pager**. When you type `man something` and start scrolling down, you're actually interacting with the `less` program, not `man`. You can test this out by pressing `h`, you'll be greeted by the documentation for the `less` program.

`Alt .` auto copy-paste the last argument of the last command (the previously-ran comman). Usages: `cp` file then `vim` that file

`ArrowUp` and `ArrowDown` Browse history. Go to the line that you want to repeat, edit details if necessary, and press Enter to save time.

Shift+PageUp and `Shift+PageDown` Browse terminal buffer (to see text that has "scrolled off" the screen).

`Tab` Command or filename completion; when multiple choices are possible, the system will either signal with an audio or visual bell, or, if too many choices are possible, ask you if you want to see them all, upon which you can hit `Tab Tab` to shows file or command completion possibilities.

**Tips:**

Quickly read files and raw data using `less`.  
Also by piping the output of commands into `less`, you can read through it and then discard the output all without needing to create temporary files.

`man readline` default shortcuts in the terminal

---

It’s true that the familiar Ctrl-c (copy) and Ctrl-v (paste) key combinations won’t work for a Bash shell session, but Shift-Ctrl-c and Shift-Ctrl-v will. You can also cut and paste by right-clicking your mouse and selecting the appropriate operation from the menu.

## `man` pages and Getting helps

`man COMMAND` view man page. Example: `man grep`

- Anything between square brackets (`[` and `]`) is optional.
- Anything followed by an ellipsis (`…`) can be repeated.
- Curly braces (`{` and `}`) mean that you should select one of the items 
separated by vertical bars (`|`).

Use `/` to search in man pages giống trong vim (`man` use `less` as its pager program.)

- For example, the specification: `bork [ -x ] { on | off } filename …`would match any of the following commands:
  * `bork on /etc/passwd`
  * `bork -x off /etc/passwd /etc/smartd.conf`
  * `bork off /usr/lib/tmac`

- `bash` shell-builtins do not have man pages và phải dùng `help [COMMAND]`
- Most GNU commands support the `-h` or `--help` options which gives a short explanation about how to use the command and a list of available options. Try: `ls -h`, `vim --help`

`type [COMMAND]` tell you if a command is built-in or not

man -> type a few characters -> `TAB`

`TRFM` => Read the friendly manual!

An **ellipsis** `...`  consists of three evenly spaced periods. In the context of Linux man pages, it means the argument is repeatable.

`[FILE]` trong man page equal file and directory.

Run `man man` or `man 7 man-pages` to learn how to use man pages.

Sections of the man pages:

1. General commands; Executable programs or shell commands; User-level commands and applications
2. System calls (functions provided by the kernel) and kernel error codes
3. Library calls or sub-routines (functions within program libraries covering in particular the C standard library)
4. Special files (usually devices, those found in `/dev`) and [drivers](https://en.wikipedia.org/wiki/Device_driver); Device drivers and network protocols
5. Standard file formats and conventions, e.g. `/etc/passwd`
6. Video Games, Screensavers, demonstrations and user-maintained programs
7. Miscellaneous files and documents (including macro packages and conventions), e.g. man(7), groff(7)
8. System administration commands and [daemons](https://en.wikipedia.org/wiki/Daemon_(computing)) (usually only for root)
9. Kernel routines \[Non standard] (FreeBSD, SVR4, Linux); Obscure kernel specs and interfaces

Section 2 and 3 is for low-level Linux programming.

The exact structure of the sections isn’t important for most topics because man finds the appropriate page wherever it is stored. You only need to be aware of the section definitions when a topic with the same name appears in multiple sections. For example, `passwd` is both a command and a configuration file, so it has entries in both section 1 and section 4 or 5.

There might be multiple pages for the same keyword in different sections. For example, running the command `man chown` by default will open section 1 `CHOWN(1)`. You can explicitly look into section 2 using `man 2 chown`.

**Layout of a man page:**

- `SYNOPSIS`: syntax overview
  * In the case of a command, a formal description of how to run it and what command line options it takes.
  * For program functions, a list of the parameters the function takes and which header file contains its declaration
- DESCRIPTION: A textual description of the functioning of the command or function. For programs, this section often includes explanations of available command line options.
- EXAMPLES: Some examples of common usage.
- SEE ALSO: A list of related commands or functions.

Other sections may be present, but these are not well standardized across man pages. Common examples include: OPTIONS, EXIT STATUS, RETURN VALUE, ENVIRONMENT, BUGS, FILES, AUTHOR, REPORTING BUGS, HISTORY and COPYRIGHT.

---

How to read man panges?

`man <title>` formats a specific manual page and sends it to your terminal through `more`, `less`, or whatever program is specified in your `PAGER` environment vari-able. title is usually a command, device, filename, or name of a library routine. The sections of the manual are searched in roughly numeric order, although sec-tions that describe commands (sections 1, 8, and 6) are usually searched first.

The form `man section title` gets you a man page from a particular section. Thus, on most systems, `man sync` gets you the man page for the sync command, and `man 2 sync` gets you the man page for the sync system call. 

**Alternatives:**

The [tldr pages](https://tldr.sh/) are a community effort to simplify the beloved man pages with practical examples.

A short index of explanations for commands is available using the `whatis` command. Try `whatis ls`

If you don't know where to get started and which man page to read, **apropos** gives more information. Say that you don't know how to start a browser, then you could enter the following command: `apropos browser`

GNU project invented info pages. Có support hyperlink through the documentation.

`yelp` command [here](https://www.commandlinux.com/man-page/man1/yelp.1.html)

The [ArchWiki](https://wiki.archlinux.org/title/Main_page) page

References:

[Mastering Linux Man Pages - A Definitive Guide](https://www.youtube.com/watch?v=RzAkjX_9B7E) by Linux Training Academy

---

The `info` command

The man system is great if you happen to know the name of the command or program you’re after. But suppose the command name is the bit that you’re missing. Type `info` command.

`info COMMAND` read Info pages on command. `info` không dùng `less`, nó dùng Stand-alone GNU Info reader.

## Command History

`history`

`!n` Run command number. Replace the n with the number of the command line and 
that line is run.

`!!` Run previous command.

`!?string—?` Run command containing string. This runs the most recent command that contains a particular string of characters. For example, you can run the date command again by just searching for part of that command line as follows:

```bash
$ !?dat?
         date
         Fri Jun 29 16:04:18 EDT 2019
```

## Terminologies

An `interactive shell` in Linux is a command-line interface where you type commands and get responses back directly. It's the primary way many users interact with the operating system.

## FAQs

[Why is the -r recursive flag capital in some and lower in others?](https://superuser.com/questions/438900/why-is-the-r-recursive-flag-capital-in-some-and-lower-in-others)

## References

[Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents)

[linux journey](https://linuxjourney.com/)
