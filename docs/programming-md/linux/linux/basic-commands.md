# Linux everyday Survival notes

## Basic Terminal commands & Keyboard shorcuts

Phân biệt command option (preceded with `-` or `--`) vs command argument. The argument(s) to a command are specifications for the object(s) on which you want the command to take effect. An example is `ls /etc`, where the directory `/etc` is the argument to the **ls** command.\
You can think of an option as a way of executing the command. The argument is what you execute it on.

`which -a ls` show that the `ls` command is in the `/bin` directory (show full path)

`.` and `..` acts as hard links.  
`./a.out` => run files

`echo tran kim phuong`, `whoami`, `touch <file name>`

`date`, `cal` calender

`pwd` display present working directory

`clear` (alias `c`)

`ls` stands for list storage/

`ls -l` (recommended) `ll` is an alias of it (not an actual linux command). Hình như `la` cũng vậy. Nhưng mình không biết setting nằm ở file nào.

`which <command>` locate the executable file associated with the given command.

`diff` compare the contents of two files and display the differences between them

On Ubunto, `Ctrl Alt f1` till `f6` to enter [virtual terminal](https://en.wikipedia.org/wiki/Virtual_console). To come back to the graphical session, press `Ctrl Alt f7`

`tty` print the file name of the terminal connected to standard input

To write comments in the shell, use the `#` symbol

`pushd`: `cd` and then push the new directory to the top of the directory stack (on the leftmost of `dirs` or the top-most of `dirs -p`)

Neofetch

The `cron` command-line utility is a job scheduler on Unix-like operating systems. Users who set up and maintain software environments use cron to schedule jobs (commands or shell scripts), also known as cron jobs, to run periodically at fixed times, dates, or intervals. It typically automates system maintenance or administration—though its general-purpose nature makes it useful for things like downloading files from the Internet and downloading email at regular intervals.

`passwd` change the password for the current user

`file FILENAME` display file type of file with name `filename`

`apropos STRING` search the whatis database for strings

In most cases, when issuing a command or starting a program as a non-privileged user, the system will warn you or prompt you for the root password when root access is required. Once you're done, _leave_ the application or session that gives you root privileges _immediately_.

`Alt` == meta key

### Terminal line editing basic

`Ctrl a` move cursor to the beginning of the command line.\
`^E` Move cursor to the end of the command line.

`Ctrl+W` to erase the previous word, `Ctrl+U` to erase the whole line

`^L`  == `clear`

`Ctrl d`, `exit` or `logout` Log out of the current shell session

`^C` End a running program and return the prompt.

Search command history: `Ctrl R` -> type a sub-string that appears in your `history` commands -> press `Enter` or press right/left arrow key to make changes before running.

`^Z` Suspend a program

`cd` between two directories with long path names. Go back to the last directory that you were in `cd -`. To go back even further, use `pushd` and `popd`. `cd` in deeply nested file system.

`/` to enter search mode. `n` to go jump to the next match (spam `n` as fast as you can). `Shift N` to jump back up.\
Works in man pages, vim, less, git logs. This is because programs like man pages and git actually use `less` as their **text pager**. When you type `man something` and start scrolling down, you're actually interacting with the `less` program, not `man`. You can test this out by pressing `h`, you'll be greeted by the documentation for the `less` program.

`Alt .` auto copy-paste the last argument of the last command (the previously-ran comman). Usages: `cp` file then `vim` that file

`ArrowUp` and `ArrowDown` Browse history. Go to the line that you want to repeat, edit details if necessary, and press Enter to save time.

Shift+PageUp and `Shift+PageDown` Browse terminal buffer (to see text that has "scrolled off" the screen).

`Tab` Command or filename completion; when multiple choices are possible, the system will either signal with an audio or visual bell, or, if too many choices are possible, ask you if you want to see them all, upon which you can hit `Tab Tab` to shows file or command completion possibilities.

**Tips:**

Quickly read files and raw data using `less`. \
Also by piping the output of commands into `less`, you can read through it and then discard the output all without needing to create temporary files.

**References:**

`man readline` default shortcuts in the terminal

## man pages and get help

`man COMMAND` view man page

shell-builtins do not have man pages. Phải search trong `man bash` such as `help`, `type`  
Thường không có man page thì thường là bash built-in và phải dùng `help [COMMAND]`

Most GNU commands support the `-h` or `--help` options which gives a short explanation about how to use the command and a list of available options. Try: `ls -h`, `vim --help`

`type [COMMAND]` tell you if a command is built-in or not

man -> type a few characters -> `TAB`

**TRFM** => Read the friendly manual!

An **ellipsis** `...`  consists of three evenly spaced periods. In the context of Linux man pages, it means the argument is repeatable.

`[FILE]` trong man page equal file and directory.

Run `man man` or `man 7 man-pages` to learn how to use man pages.

Manual sections:

1. General commands; Executable programs or shell commands
2. System calls (functions provided by the kernel)
3. Library calls or sub-routines (functions within program libraries covering in particular the C standard library)
4. Special files (usually devices, those found in `/dev`) and [drivers](https://en.wikipedia.org/wiki/Device_driver)
5. File formats and conventions, e.g. `/etc/passwd`
6. Video Games, Screensavers and user-maintained programs
7. Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
8. System administration commands and [daemons](https://en.wikipedia.org/wiki/Daemon_(computing)) (usually only for root)
9. Kernel routines \[Non standard] (FreeBSD, SVR4, Linux)

Section 2 and 3 is for low-level Linux programming.

There might be multiple pages for the same keyword in different sections. For example, running the command `man chown` by default will open section 1 `CHOWN(1)`. You can explicitly look into section 2 using `man 2 chown`.

**Layout:**

- SYNOPSIS: In the case of a command, a formal description of how to run it and what command line options it takes. For program functions, a list of the parameters the function takes and which header file contains its declaration
- DESCRIPTION: A textual description of the functioning of the command or function. For programs, this section often includes explanations of available command line options.
- EXAMPLES: Some examples of common usage.
- SEE ALSO: A list of related commands or functions.

Other sections may be present, but these are not well standardized across man pages. Common examples include: OPTIONS, EXIT STATUS, RETURN VALUE, ENVIRONMENT, BUGS, FILES, AUTHOR, REPORTING BUGS, HISTORY and COPYRIGHT.

**Alternatives:**

The [tldr pages](https://tldr.sh/) are a community effort to simplify the beloved man pages with practical examples.

A short index of explanations for commands is available using the `whatis` command. Try `whatis ls`

If you don't know where to get started and which man page to read, **apropos** gives more information. Say that you don't know how to start a browser, then you could enter the following command: `apropos browser`

GNU project invented info pages. Có support hyperlink through the documentation.

`yelp` command [here](https://www.commandlinux.com/man-page/man1/yelp.1.html)

`info COMMAND` read Info pages on command. Info không dùng `less`, nó dùng Stand-alone GNU Info reader.

The [ArchWiki](https://wiki.archlinux.org/title/Main_page) page

**References:**\

[Mastering Linux Man Pages - A Definitive Guide](https://www.youtube.com/watch?v=RzAkjX_9B7E) by Linux Training Academy

## wc

obtain word and line counts from text files and streams.

How many lines are in this file?

Count the number of byte present too.

## Alias

`alias` list all alias in your terminal

`unalias <alias>` remove an alias

## fzf

fzf

## sed - search & replace

search & replace operation on text files or streams.

Replace all instances of "cat" with "dog".

csv data that contains numbers that are erroneously padded with double quote. In order to correctly process this data, you need to get rid of the double quotes. This can be accomplished with the following `sed` command:  `cat data.csv | sed 's/"//g'`.

The true power of `sed` comes from the fact that it enables you to perform (really) complex changes that can be described by regular expressions. Ex: swap text.

**Back references** allow you to match a variable part of text and then move it around or changes it later depending on how you want to re-write it.

## awk

awk giống `sed` can be used to perform text replacements. But `sed` is easier for regex-based replacements. Whereas `awk` can perform arbitrary computations that `sed` can't.

AWK is a _domain-specific language_ designed for text processing and typically used as a data extraction and reporting tool. Like sed and grep, it is a filter, and it is a standard feature of most Unix-like operating systems.

extract the first column.

## sort

By default, `sort` uses lexicographical sort which may not always be what you want.

`-n` do numerical sort.\
`-r` sort in the reverse order\
`-R` sort the list Randomly. You'll get a different order in each time.

`sort`'s output can be piped directly into a number of other useful commands.

## tsort

`tsort` stands for [topological sort](https://en.wikipedia.org/wiki/Topological_sorting).

## tee

split off a stream so that its output can be simultaneously sent to a file and stdout. Named after the T-splitter in plumping.

Debug why a complicated shell pipe ins't working. Dùng để tạo `.log` files for debugging shell pipes.

## comm

compare the contents of two sorted files.

Determine which lines are common to both files or determine which lines are missing from either file. More formally, `comm` allows you to compute the [union](https://en.wikipedia.org/wiki/Union_(set_theory)) (hội), [intersection](https://en.wikipedia.org/wiki/Intersection_(set_theory)) (giao) and [relative complement](https://en.wikipedia.org/wiki/Complement_(set_theory)) of the lines found in two files.

Btw, you should learn [set theory](https://en.wikipedia.org/wiki/Set_theory) (lý thuyết tập hợp).

## uniq

Answer questions about the uniqueness of lines in a file. Just like `comm`, this command also requires that its input be sorted first.

List of unique products that are found in the sales information.

`-c` count

## tr

perform simple character replacements in text or binary data. Useful in cleaning up the results of other commands.

Remove unwanted carriage returns from a file that was created on Windows.

## Checking Resource usage - htop

`free` Display amount of free and used memory (RAM) in the system. `free -m` Display the amount of memory in mebibytes.
On MacOS, use the Activity Monitor app

`df -h` (disk free) report file system disk space usage. The `-h` means "human readable" which display the values in Gigabytes
`df -i` list inode information instead of block usage. The file system have a limited number of files that it can store.

`htop` allows the user to interactively monitor the system’s vital resources or server’s processes in real time. Trên MacOS thì dùng [Activity Monitor](https://support.apple.com/en-vn/guide/activity-monitor/welcome/mac)

`uptime` Tell how long the system has been running, load average

[Linux ate my ram](https://www.linuxatemyram.com) for further learning

## Managing systemd Units

`systemclt status <unit name>` show status of a unit

`sudo systemclt {disable|enable} <unit name>` disable/enable unit

`sudo systemclt {stop|start} <unit name>` stop/start unit

`sudo systemclt restart <unit name>` restart the unit (when changing configuration for example)

## Viewing logs

`dmesg` display all messages from the kernel ring buffer.

`tail -f /var/log/syslog` The `-f, --follow` output appended data as the file grows (helpful)

`journalctl` print log entries from the systemd journal
`journalctl -u <unit name>` print the log of a specific systemd unit
`journalctl -fu <unit name>` follow log

## Archive & Transfering data

### tar

In computing, [tar](https://en.wikipedia.org/wiki/Tar_(computing)) is a computer software utility for collecting many files into one archive file, often referred to as a tarball, for distribution or backup purposes. The name is derived from "tape archive", as it was originally developed to write data to sequential I/O devices with no file system of their own, such as devices that use magnetic tape.

### curl

`curl` stands for "client url"

On MacOS, curl comes pre-installed.

You can use curl with a REST-api.

## FAQs

[Why is the -r recursive flag capital in some and lower in others?](https://superuser.com/questions/438900/why-is-the-r-recursive-flag-capital-in-some-and-lower-in-others)

## References

[Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents)

[linux journey](https://linuxjourney.com/)
