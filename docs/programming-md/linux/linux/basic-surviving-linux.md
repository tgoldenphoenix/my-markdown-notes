# Basic Linux Surviving

Phân biệt command option (preceded with `-` or `--`) vs command argument. The argument(s) to a command are specifications for the object(s) on which you want the command to take effect. An example is `ls /etc`, where the directory `/etc` is the argument to the **ls** command.  
You can think of an option as a way of executing the command. The argument is what you execute it on.

`which -a ls` show that the `ls` command is in the `/bin` directory (show full path)

`.` and `..` acts as hard links.
`./a.out` => run files

`echo tran kim phuong`

`whoami`

`touch <file name>`

“Touching” an existing file with touch updates its time stamp without making any changes. This can be useful if, for some reason, you want to change how various commands like ls list or display a file. (It can also be helpful if you’d like your boss to think that you’ve been hard at work on a data file that, in fact, you haven’t opened for weeks.) 

`date`, `cal` calender

`gedit` is an UI text editor on linux

`pwd` display present working directory

`clear` (alias `c`)

`ls` stands for list storage

`ls -l` (recommended) `ll` is an alias of it (not an actual linux command). Hình như `la` cũng vậy. Nhưng mình không biết setting nằm ở file nào.

`which <command>` locate the executable file associated with the given command.

`diff` compare the contents of two files and display the differences between them

On Ubunto, `Ctrl Alt f1` till `f6` to enter [virtual terminal](https://en.wikipedia.org/wiki/Virtual_console). To come back to the graphical session, press `Ctrl Alt f7`

`tty` print the file name of the terminal connected to standard input

To write comments in the shell, use the `#` symbol

`pushd`: `cd` and then push the new directory to the top of the directory stack (on the leftmost of `dirs` or the top-most of `dirs -p`)

The `cron` command-line utility is a job scheduler on Unix-like operating systems. Users who set up and maintain software environments use cron to schedule jobs (commands or shell scripts), also known as cron jobs, to run periodically at fixed times, dates, or intervals. It typically automates system maintenance or administration—though its general-purpose nature makes it useful for things like downloading files from the Internet and downloading email at regular intervals.

`passwd` change the password for the current user

`file FILENAME` display file type of file with name `filename`

`apropos STRING` search the whatis database for strings

In most cases, when issuing a command or starting a program as a non-privileged user, the system will warn you or prompt you for the root password when root access is required. Once you're done, _leave_ the application or session that gives you root privileges _immediately_.

`Alt` == meta key

`Ctrl Alt t` open the terminal

## Moving in the Terminal prompt

- Emac mode shell key bindings
  - `^b` & `^f` move backward/forward one char
  - `Alt b/f` move backward/forward one word
  - `^a` & `^e` go to start/end of prompt
  - `^p` & `^n` steps backward/forward through commands in `history`.
  - `^r` searches incrementally through your history to find old commands.
  - `^k` delete from cursor to end of line
  - `^h` delete the previous character (equal to backspace)
  - `^w` delete the previous word
  - `^u` delete the whole line
  - `^l` == `clear`
  - `^d`, `exit` or `logout` Log out of the current shell session
  - `^c` End a running program and return the prompt.
  - `^z` Suspend a program

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

## Ctrl-d vs exit command

`Ctrl+D` Sends the End-of-File (EOF) character or signal. When a shell (like Bash) receives an EOF signal on an empty command line, it interprets it as a signal to terminate the current session.

`exit` is an explicit shell command that tells the shell to terminate immediately.

How about `Ctrl c` and `Ctrl z`?

## man pages and get help

- Anything between square brackets (`[` and `]`) is optional.
- Anything followed by an ellipsis (`…`) can be repeated.
- Curly braces (`{` and `}`) mean that you should select one of the items 
separated by vertical bars (`|`).

Use `/` to search in man pages giống trong vim.

For example, the specification:  
`bork [ -x ] { on | off } filename …`  
would match any of the following commands:

`bork on /etc/passwd`
`bork -x off /etc/passwd /etc/smartd.conf`
`bork off /usr/lib/tmac`

`man COMMAND` view man page

shell-builtins do not have man pages. Phải search trong `man bash` such as `help`, `type`  
Thường không có man page thì thường là bash built-in và phải dùng `help [COMMAND]`

Most GNU commands support the `-h` or `--help` options which gives a short explanation about how to use the command and a list of available options. Try: `ls -h`, `vim --help`

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

### Read man panges

`man <title>` formats a specific manual page and sends it to your terminal through `more`, `less`, or whatever program is specified in your `PAGER` environment vari-able. title is usually a command, device, filename, or name of a library routine. The sections of the manual are searched in roughly numeric order, although sec-tions that describe commands (sections 1, 8, and 6) are usually searched first.

The form `man section title` gets you a man page from a particular section. Thus, on most systems, `man sync` gets you the man page for the sync command, and `man 2 sync` gets you the man page for the sync system call. 

**Alternatives:**

The [tldr pages](https://tldr.sh/) are a community effort to simplify the beloved man pages with practical examples.

A short index of explanations for commands is available using the `whatis` command. Try `whatis ls`

If you don't know where to get started and which man page to read, **apropos** gives more information. Say that you don't know how to start a browser, then you could enter the following command: `apropos browser`

GNU project invented info pages. Có support hyperlink through the documentation.

`yelp` command [here](https://www.commandlinux.com/man-page/man1/yelp.1.html)

`info COMMAND` read Info pages on command. Info không dùng `less`, nó dùng Stand-alone GNU Info reader.

The [ArchWiki](https://wiki.archlinux.org/title/Main_page) page

References:

[Mastering Linux Man Pages - A Definitive Guide](https://www.youtube.com/watch?v=RzAkjX_9B7E) by Linux Training Academy

### info

The man system is great if you happen to know the name of the command or program you’re after. But suppose the command name is the bit that you’re missing. Type `info` command.

## Basic Commands

Tên file có space thì phải để trong double quote.

`less "My excellent file.txt"`

- `rm` to remove files
- `rmdir` to remove empty directories. It can only remove empty directories. 

`rm -r directory-name` remove directory

(Use `ls -a` to check whether a directory is empty or not). The rm command also has options for removing non-empty directories with all their subdirectories, read the Info pages for these rather dangerous options.\
The interactive behavior of the rm, cp and mv commands can be activated using the -i option. In that case the system won't immediately act upon request. Instead it will ask for confirmation, so it takes an additional click on the Enter key to inflict the damage. Customize your shell environment to make this option the default.

`rm file1` delete file1 from the directory

`rm file*` deletes all files in the current directory whose names begin with the letters file.

`rm -r *` wipe out the current directory

---

- `cp` to copy files
- `mv` to move & rename files

cp 2 files into 1 directory: `cp <filename> <filename1> <foldername>/`

`cp -r /path/to/source/directory /path/to/destination/directory` cp directory. Lưu ý là `directory` chứ ko phải `directory/` (sẽ copy content).

`-R` recursive copy (copy all underlying files and subdirectories). The general syntax is `cp [-R] fromfile tofile`

`$ cp file1 newdir` => creates a copy of file1 within the directory newdir.

By the way, the `cp` command knows what to do with this command line because it’s smart enough to recognize newdir as a directory rather than a file. If there were no directory called newdir in the current location, cp would instead make a new copy of file1 named newdir. If you’re anything like me, at some point you’re probably going to accidentally misspell a command and end up with an odd new file rather than the directory you were after. In any case, check everything to confirm it all works out the way it was supposed to.

`mv ../filename.txt .` move a file to current directory

rename directory `mv Oldfolder Newfolder`

`$ mv file2 newdir` moves `file2` to `newdir` directory

You can copy, move, or delete directories using the same commands as for files, adding the -r flag where necessary. Remember that you might be moving more than just the directory you see: any existing layers of unseen nested levels will also be dragged along for the ride. 

---

`mkdir dir1 dir2` create multiple directories in one go

`mkdir -p x/y/z` create nested directories in a single command. The `-p` switch create parents directories.

---

`df` (which stands for _disk full_ or _disk free_) show how much of your disk is still free; information about the partitions and their mount points\
supports the `-h` or _human readable_ option which greatly improves readability

How can you find out which partition a directory is on? Using the `df` command with a dot (.) as an option shows the partition the current directory belongs to, and informs about the amount of space used on this partition

The `df` command only displays information about active non-swap partitions. These can include partitions from other networked systems

since the search path contains only paths to directories containing executable programs, **which** doesn't work for ordinary files. The **which** command is useful when troubleshooting "Command not Found" problems.\
Using the `which` command also checks to see if a command is an alias for another command `which -a ls`. If this does not work on your system, use the alias command: `alias ls`

## find & locate

These are the real tools, used when searching other paths beside those listed in the search path (using `which`).

`find` sfind files and directory in a directory hierarchy. This command not only allows you to search file names, it can also accept file size, date of last change and other file properties as criteria for a search. The most common use is for finding file names: `find <path> -name <searchstring>`\
This can be interpreted as "Look in all files and subdirectories contained in a given path, and print the names of the files containing the search string in their name" (not in their content).

- filter by file type, file name and a number of other options.
- Another application of find is for searching files of a certain size

One of the most useful features of `find` is its ability to execute arbitrary shell commands against each file that matches the search. For example:

- count total number of lines in all files and sub-directories under the current directory.
- run a string replacement using `sed` against all files under the current directory
- find all file under `.` that have the `.jpg` extension and print out the width x height of each image.

 When using `which` to remove files, it is best to first test without the `-exec` option that the correct files are selected, after that the command can be rerun to delete the selected files.

After cloning a repository or un-zipping an archive. You'll wonder "How many files are here and where is everything?" => `find .`. Nếu nhiều quá thì `find . | less` and use `/` to search for something you might be interested in.

**Examples:**

`find .` print all files and directories in and under the current directory.

`find . -name *.py`\
`find . -not -name *.py`

`find . -not -name '*.py' -delete` => delete all, keep only `.py`. Use single quote to pass wildcard pattern _unchange_ in this case.

### locate

Later on (in 1999 according to the man pages, after 20 years of find), `locate` was developed. This program is easier to use, but more restricted than find, since its output is based on a file index database that is updated only once every day. On the other hand, a search in the locate database uses less resources than find and therefore shows the results nearly instantly. Most Linux distributions use `slocate` these days, security enhanced locate, the modern version of locate that prevents users from getting output they have no right to read.  On most systems, locate is a symbolic link to the slocate program

## Alias

`alias` list all alias in your terminal

`unalias <alias>` remove an alias

## sudo

By default, the user created during the initial Linux installation will have sudo powers.

When illustrating command-line examples throughout this book, I use a command prompt of $ for commands that don’t require administrator privileges and, instead of `$ sudo`, I use `#` for those commands that do. Thus a sudo command will look like this: `# nano /etc/group`

## fzf

fzf

## cut

> separate lines into fields

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

> sort lines

`sort`'s output can be piped directly into a number of other useful commands.

- Options:
  - `-t`: Set field separator (the default is whitespace)
  - `-k` Specify the columns that form the sort key
  - `-n` do numerical sort. By default, `sort` uses lexicographical sort (dictionary sort)
  - `-r` sort in the reverse order
  - `-R` sort the list Randomly. You'll get a different order in each time.
  - `-u` Output unique records only

Both commands below use the `-t:` and `-k3,3` options to sort the /etc/group file by its third colon-separated field, the group ID. The first sorts numerically and the second alphabetically.

```bash
$ sort -t: -k3,3 -n /etc/group1
root:x:0:
bin:x:1:daemon
daemon:x:2:
…
$ sort -t: -k3,3 /etc/group 
root:x:0:
bin:x:1:daemon
users:x:100:
…
```

sort accepts the key specification `-k3` (rather than `-k3,3`), but it probably doesn’t do what you expect. Without the terminating field number, the sort key continues to the end of the line.

### tsort

`tsort` stands for [topological sort](https://en.wikipedia.org/wiki/Topological_sorting).

## tee

> copy input to two places

split off a stream so that its output can be simultaneously sent to a file and stdout. Named after the T-splitter in plumping.

Debug why a complicated shell pipe ins't working. Dùng để tạo `.log` files for debugging shell pipes.

The device `/dev/tty` is a synonym for the current terminal. For example,

```bash
find / -name core | tee /dev/tty | wc -l
```

prints both the pathnames of files named core and a count of the number of core files that were found.

## cat

`cat` (concatenate) print a file to the screen where it can be read, but not edited. This works pretty well for shorter documents.

If the file you want to read contains more lines than will display in a single screen, you don't use `cat`.

## head - tail, less - more

> read the beginning or end of a file

These commands display **ten lines** by default.

For interactive use, `head` is more or less obsoleted by the `less` command, which paginates files for display. But `head` still finds plenty of use within scripts.

In practice, you should only use `less`. Don't use `more` & `head`.

Instead of exiting immediately after printing the requested number of lines, `tail -f` waits for new lines to be added to the end of the file and prints them as they appear— great for monitoring log files.  
Type `<Control-C>` to stop monitoring.

## grep

> search text in files

`grep` searches its input text and prints the lines that match a given pattern. Its name is based on the **g**/regular-expression/**p** command from the old **ed** editor that came with the earliest versions of UNIX (and still does).

- Options:
  * `-c` to print a count of matching lines
  * `-i` to ignore case when matching
  * `-v` to print nonmatching (rather than matching) lines
  * `-l` (lowercase L) makes grep print only the names of matching files rather than printing each line that matches.

```bash
$ sudo grep -l mdadm /var/log/* 
/var/log/auth.log 
/var/log/syslog.0
```

Above command shows that log entries from mdadm have appeared in two different log files.

Search for pieces of matching text in text files such as a csv file that store historical purchase information for products in a store giống dạng excel.

`grep Sneaker sales.csv` Only show lines in file `sales.csv` that contain the string "Sneaker". Work with millions of lines of text.

`grep` can be used with _regular expression_. Ex: extract all the different model number from a csv of sale record.

By default, grep will print the entire line of text where a match is found.\
`-o` flag extract only the matched part of the text.

find and locate are often used in combination with grep to define some serious queries.

**Other useful features:**\

- recursive searches `-R` that look through all files & sub-directory, showing files and line numbers.
- disabling case-sensitivity of the matching.
- inverting matching logic by showing only lines that don't match instead.

## Viewing file content

### cat

`cat` is sort for "concatenate". It allows you to concatenate multiple files together and have the aggregate input piped into another command.

quicky add line number counts by using `-n` flag.

### "less is more"

[less](https://en.wikipedia.org/wiki/Less_(Unix)) is a terminal pager program used to view (but not change) the contents of a text file one screen at a time. It is similar to `more`, but has the extended capability of allowing both forward and backward navigation through the file.

`less` is the GNU version of `more` and has extra features allowing highlighting of search strings, scrolling back etc.

Khi dùng `less cat more` để view file, `Shift G` move to end of the file. Also works in man pages, vim.

`q` to quit less

`spacebar` next page, `b` to go back one page

`h` open documentation of `less`

`/` to search

less support vim key-bindings.

`^` means Ctrl

`^F` `Ctrl F` Forward one window

### head & tail

`head` selectively output only the first few lines of a file or stream. Ex: the first 10 lines.

`-n` negative number thì count backward from the end of the file

`head` can also print out the first few character instead of the first few lines.

`tail` does the opposite of `head`. The tail command has a handy feature to continuously show the last n lines of a file that changes all the time. This `-f` option is often used by system administrators to check on log files.

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

## comm

compare the contents of two sorted files.

Determine which lines are common to both files or determine which lines are missing from either file. More formally, `comm` allows you to compute the [union](https://en.wikipedia.org/wiki/Union_(set_theory)) (hội), [intersection](https://en.wikipedia.org/wiki/Intersection_(set_theory)) (giao) and [relative complement](https://en.wikipedia.org/wiki/Complement_(set_theory)) of the lines found in two files.

Btw, you should learn [set theory](https://en.wikipedia.org/wiki/Set_theory) (lý thuyết tập hợp).

## uniq

> print unique lines

Just like `comm`, this command also requires that its input be sorted first (usually by being run through `sort`).

- `uniq` is similar in spirit to `sort -u`, but it has some useful options that sort does not emulate:
  - `-c` to count the number of instances of each line
  - `-d` to show only dupli-cated lines
  - `-u` to show only nonduplicated lines

For example, the command below shows that 20 users have `/bin/bash` as their login shell and that 12 have /bin/false. (The latter are either pseudo-users or users whose accounts have been disabled.)

```bash
$ cut -d: -f7 /etc/passwd | sort | uniq -c
   20 /bin/bash
   12 /bin/false
```

## wc (word cound)

> count lines, words, and characters

Run without options, it displays all three counts:

```bash
$ wc /etc/passwd
 32  77 2003 /etc/passwd
```

Count the number of byte present too.

In the context of scripting, it is more common to supply a `-l, -w, or -c` option to make wc’s output consist of a single number.

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

## Managing `systemd` Units

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

## curl

`curl` stands for "client url"

On MacOS, curl comes pre-installed.

You can use curl with a REST-api.

```
$ curl "https:/ /awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
```

The `-o` (output flag) instructs `curl` to save the fetched data (instead of printing it to the terminal).

## wget

- `wget` is optimized for simple, robust file downloading (especially recursive and background downloads).
- `curl` is optimized for data transfer and protocol flexibility, making it the primary tool for API interaction and scripting.

`wget` saves output to a file by default, while `curl` Prints to standard output (stdout) by default.

## Read system logs

On nearly all modern Linux distributions, you can access all system logs through journalctl: 

`# journalctl`

As you’ll quickly see, running journalctl without any arguments will drown you in a torrent of data. You’ll need to find some way to filter for the information you’re after. Allow me to introduce you to grep:

`# journalctl | grep filename.php`

You can use grep in sequence to narrow your results further:

`# journalctl | grep filename.php | grep error`

In case you’d prefer to see only those lines that don’t contain the word error, you’d add -v (for inverted results): 

`# journalctl | grep filename.php | grep -v error`

## ACL

ACL (Access Control List) is used in both Linux and networking because it is a universal computer security concept—the method used to implement granular authorization.

The reason the name is the same is that the goal is identical: to have a list of rules that the system checks in order to decide whether to allow or deny access to a resource.

- Linux (Filesystem) ACL
  * Resource: A specific file or folder.
  * Purpose: Granular Permissions (Who can touch this file?).

- Networking (Firewall/Router) ACL
  * Resource: A network interface or an entire subnet.
  * Purpose: Traffic Filtering (What data is allowed on this wire?).

## Terminologies

An **interactive shell** in Linux is a command-line interface where you type commands and get responses back directly. It's the primary way many users interact with the operating system.

## FAQs

[Why is the -r recursive flag capital in some and lower in others?](https://superuser.com/questions/438900/why-is-the-r-recursive-flag-capital-in-some-and-lower-in-others)

## References

[Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents)

[linux journey](https://linuxjourney.com/)
