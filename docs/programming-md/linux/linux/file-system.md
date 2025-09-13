# The File Systems

> On a UNIX system, everything is a file; if something is not a file, it is a process.

On a standard Linux system you will find the layout generally follows the scheme presented below.

[Figure 3-1. Linux file system layout](https://tldp.org/LDP/intro-linux/html/sect_03_01.html#AEN1977)

This is a layout from a RedHat system. The names are not even required; they are only a convention.

[Table 3-2. Subdirectories of the root directory](https://tldp.org/LDP/intro-linux/html/sect_03_01.html#AEN2004)

A Linux system, just like UNIX, makes no difference between a file and a directory, since a directory is just a file containing names of other files. Programs, services, texts, images, and so forth, are all files. Input and output devices, and generally all devices, are considered to be files, according to the system.

There are special files that are more than just files (named pipes and sockets, for instance).

While it is reasonably safe to suppose that everything you encounter on a Linux system is a file, there are some exceptions:
- Special files: the mechanism used for input and output. Most special files are in `/dev`
- Links: a system to make a file or directory visible in multiple parts of the system's file tree.
- (Domain) sockets: a special file type, similar to TCP/IP sockets, providing inter-process networking protected by the file system's access control.
- Named pipes: act more or less like sockets and form a way for processes to communicate with each other, without using network socket semantics.

Depending on which type of Linux environment you are running, you may run into several different file systems. Some of them are **ext2, ext3, and ext4**. **XFS, JFS**, and a few others are also used. `ext3` is a journaling extension to the ext2 file system on Linux. ext4 is the successor to ext3. Journaling is a method of recording data that results in massively reduced time spent recovering a file system after a crash. XFS is very fast and also uses B-Trees for its file indexing.

Though `*/bin` seems to suggest by name that the contents therein are BINaries, the BS* `hier` man-page does **not** seem to suggest it. It only seems to suggest that the contents be executable. So you can store your bash scripts inside bin directories.

Mind the difference between `/`, the root directory and `/root`, the home directory of the root user.

Usually store your scripts in `/usr/local/bin`, unless you don't want other users to have access to them, in which case `$HOME/bin`.

`/usr/local/bin` may be in the default PATH, but `$HOME/bin` will certainly need to be added to PATH.

**Adding `$HOME/bin` to PATH:**
```bash
PATH=${PATH}:$HOME/bin
export PATH
```

`/usr`—Contains non-essential command-line binaries, libraries, header files, and other data. At least it is non-essential to the system. The dotfiles is actually essential to the users.

Khi cắm USB external hard drive vào ubuntu thì nó sẽ ở `/media/anhao/<hard drive name>`

[Distinguish all the */bin and */sbin and /top on the file system](https://unix.stackexchange.com/questions/8656/usr-bin-vs-usr-local-bin-on-linux)

## Basic Commands

- `cp` to copy files
- `mv` to move & rename files

`rm` to remove files `rmdir` to remove empty directories. (Use **ls `-a`** to check whether a directory is empty or not). The rm command also has options for removing non-empty directories with all their subdirectories, read the Info pages for these rather dangerous options.\
The interactive behavior of the rm, cp and mv commands can be activated using the -i option. In that case the system won't immediately act upon request. Instead it will ask for confirmation, so it takes an additional click on the Enter key to inflict the damage. Customize your shell environment to make this option the default.

`mv ../filename.txt .` move to current directory

rename directory `mv Oldfolder Newfolder`

`mkdir dir1 dir2` create multiple directories in one go

`mkdir -p x/y/z` create nested directories in a single command. The `-p` switch create parents directories.

cp 2 files into 1 directory: `cp <filename> <filename1> <foldername>/`

`cp -r /path/to/source/directory /path/to/destination/directory` cp directory. Lưu ý là `directory` chứ ko phải `directory/` (sẽ copy content). [Read more](https://www.freecodecamp.org/news/how-to-copy-a-directory-in-linux-with-the-cp-command/).

`-R` recursive copy (copy all underlying files and subdirectories). The general syntax is `cp [-R] fromfile tofile`

`df` (which stands for _disk full_ or _disk free_) show how much of your disk is still free; information about the partitions and their mount points\
supports the `-h` or _human readable_ option which greatly improves readability

How can you find out which partition a directory is on? Using the `df` command with a dot (.) as an option shows the partition the current directory belongs to, and informs about the amount of space used on this partition

The **df** command only displays information about active non-swap partitions. These can include partitions from other networked systems

since the search path contains only paths to directories containing executable programs, **which** doesn't work for ordinary files. The **which** command is useful when troubleshooting "Command not Found" problems.\
Using the `which` command also checks to see if a command is an alias for another command `which -a ls`. If this does not work on your system, use the alias command: `alias ls`

## ls

A description of the full functionality and features of the **ls** command can be read with `info coreutils ls`

To find out more about the kind of data we are dealing with, we use the **file** command. See `info file` for a detailed description.

In DOS, use `dir`

`ls -l` meanings:
- `-` regular file
- `d` directory
- `l` link
- `c` special file
- `s` socket
- `p` named pipe
- `b` block device

`ls -F`
- `nothing` regular files
- `*` for executables\
- `/` for directories\
- `@` for symbolic links

As a user, you only need to deal directly with plain files, executable files, directories and links. The special file types are there for making your system do what you demand from it and are dealt with by system administrators and programmers.

**Options**

`ls -l -h` == `ls -lh` == `ls -hl`

`-a` show hidden dotfiles

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

## grep - search text in file

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

sort for concatenate. It allows you to concatenate multiple files together and have the aggregate input piped into another command.

quicky add line number counts by using `-n` flag.

### "less is more"

[less](https://en.wikipedia.org/wiki/Less_(Unix)) is a terminal pager program on Unix, Windows, and Unix-like systems used to view (but not change) the contents of a text file one screen at a time. It is similar to more, but has the extended capability of allowing both forward and backward navigation through the file. `less` is the GNU version of `more` and has extra features allowing highlighting of search strings, scrolling back etc. 

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

**On Linux**

**On MacOS**

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

**References**

[how to add folder to PATH](https://phoenixnap.com/kb/linux-add-to-path) by PHOENIXNAP very informative

## Mounting

All partitions are attached to the system via a mount point. The mount point defines the place of a particular data set in the file system. Usually, all partitions are connected through the _root_ partition. On this partition, which is indicated with the slash `/`, directories are created. These empty directories will be the starting point of the partitions that are attached to them.

During system startup, all the partitions are thus mounted, as described in the file `/etc/fstab`. Some partitions are not mounted by default, for instance if they are not constantly connected to the system, such like the storage used by your digital camera. If well configured, the device will be mounted as soon as the system notices that it is connected, or it can be user-mountable, i.e. you don't need to be system administrator to attach and detach the device to and from the system.

Device files defined based on the controllers they are using.
1. For [IDE controllers device](https://www.ibm.com/support/pages/ide-controllers-servers) file name is - `hda, hdb, hdc..`
2. For SCSI and SATA controllers device file name is - `sda, sdb, sdc..`

**References:** 

[What is meant by mounting a device in Linux?](https://unix.stackexchange.com/questions/3192/what-is-meant-by-mounting-a-device-in-linux)

[mounting-a-device-role-of /dev /media-and /mnt-and-the-mount-command](https://unix.stackexchange.com/questions/13975/mounting-a-device-role-of-dev-media-and-mnt-and-the-mount-command)

## Partitioning

Linux uses more than one partition on the same disk, even when using the standard installation procedure.

One of the goals of having different partitions is to achieve higher data security in case of disaster. By dividing the hard disk in partitions, data can be grouped and separated. When an accident occurs, only the data in the partition that got the hit will be damaged, while the data on the other partitions will most likely survive. This principle dates from the days when Linux didn't have **journaled file systems** and power failures might have lead to disaster. This is currently the most important reason for partitioning.\
 A simple example: a user creates a script, a program or a web application that starts filling up the disk. If the disk contains only one big partition, the entire system will stop functioning if the disk is full. If the user stores the data on a separate partition, then only that (data) partition will be affected, while the system partitions and possible other data partitions keep functioning.

There are two kinds of major partitions on a Linux system:
- data partition: normal Linux system data, including the root partition containing all the data to start up and run the system; and
- swap partition: expansion of the computer's physical memory, extra memory on hard disk.

  Most systems contain a root partition, one or more data partitions and one or more swap partitions. Systems in mixed environments may contain partitions for other system data, such as a partition with a FAT or VFAT file system for MS Windows data.

The standard root partition (indicated with a single forward slash, /) is about 100-500 MB, and contains the system configuration files, most basic commands and server programs, system libraries, some temporary space and the home directory of the administrative user. A standard installation requires about 250 MB for the root partition.

Swap space (indicated with _swap_) is only accessible for the system itself, and is hidden from view during normal operation. Swap is the system that ensures, like on normal UNIX systems, that you can keep on working, whatever happens. On Linux, you will virtually never see irritating messages like _Out of memory, please close some applications first and try again_, because of this extra memory. The swap or virtual memory procedure has long been adopted by operating systems outside the UNIX world by now.

Linux generally counts on having twice the amount of physical memory in the form of swap space on the hard disk. When installing a system, you have to know how you are going to do this. An example on a system with 512 MB of RAM:
1. 1st possibility: one swap partition of 1 GB
2. 2nd possibility: two swap partitions of 512 MB
3. 3rd possibility: with two hard disks: 1 partition of 512 MB on each disk.

The kernel is on a separate partition as well in many distributions, because it is the most important file of your system. If this is the case, you will find that you also have a `/boot` partition, holding your kernel(s) and accompanying data files.

The rest of the hard disk(s) is generally divided in data partitions, although it may be that all of the non-system critical data resides on one partition, for example when you perform a standard workstation installation. When non-critical data is separated on different partitions, it usually happens following a set pattern:
- a partition for user programs (_/usr_)
- a partition containing the users' personal data (_/home_)
- a partition to store temporary data like print- and mail-queues (_/var_)
- a partition for third party and extra software (_/opt_)

Everything is put together on one large partition, swap space twice the amount of RAM is added and your **generic workstation** is complete, providing the largest amount of disk space possible for personal use, but with the disadvantage of possible data integrity loss during problem situations.\
On a server, system data tends to be separate from user data. Programs that offer services are kept in a different place than the data handled by this service.

### fdisk

Most Linux systems use `fdisk` at installation time to set the partition type. The standard Linux partitions have number 82 for swap and 83 for data, which can be journaled (ext3) or normal (ext2, on older systems). The **fdisk** utility has built-in help, should you forget these values.

## Linking files

**Hard link**: Associate two or more file names with the same inode. Hard links share the same data blocks on the hard disk, while they continue to behave as independent files.\
There is an immediate disadvantage: hard links can't span partitions, because inode numbers are only unique within a given partition.\
Each regular file is in principle a hardlink.

Soft link or **symbolic link** (or for short: symlink): a small file that is a pointer to another file. A symbolic link contains the path to the target file instead of a physical location on the hard disk. Since inodes are not used in this system, soft links can span across partitions.\
Symbolic links are always very small files, while hard links have the same size as the original file.

The two link types behave similar, but are not the same, as illustrated in the scheme below:

[Figure 3-2. Hard and soft link mechanism](https://tldp.org/LDP/intro-linux/html/sect_03_03.html#AEN3699)

Removing the target file for a symbolic link makes the link useless. Nhìn figure above là hiểu liền.

The command to make links is ln. In order to create symlinks, you need to use the -s option: `ln -s targetfile linkname`

# File Security & User Management

### User Management

If root access is needed and a user has root access, they can run a command as root instead with the sudo command. The `sudo` command (superuser do) is used to run a command with root access. There is a file called the `/etc/sudoers` file, this file lists users who can run sudo. You can edit this file with the `visudo` command.

**/etc/passwd & /etc/shadow**

Remember that usernames aren't really identifications for users. The system uses a **user ID (UID)** to identify a user. To find out what users are mapped to what ID, look at the `/etc/passwd` file: `cat /etc/passwd`. Other information in this file includes: user's shell, home directory\
The password is not really stored in this file, it's usually stored in the `/etc/shadow` file. If you see an `x` that means the password is stored in the /etc/shadow file, a `*` means the user doesn't have login access and if there is a blank field that means the user doesn't have a password.

`/etc/passwd` contains other users which are not human (system users). Remember that users are really only on the system to run processes with different permissions. Sometimes we want to run processes with pre-determined permissions. For example, the daemon user is used for daemon processes.

The `/etc/shadow` file is used to store information about user authentication. It requires superuser read permissions.\
In most distributions today, user authentication doesn't rely on just the /etc/shadow file, there are other mechanisms in place such as PAM (Pluggable Authentication Modules) that replace authentication.

`/etc/group` store group names and users inside that group. You can also use the command `groups`

You can use the `adduser` or the `useradd` command. The adduser command contains more helpful features such as making a home directory and more. There are configuration files for adding new users that can be customized depending on what you want to allocate to a default user. Example: `$ sudo useradd bob`

To remove a user, you can use the `userdel` command.

Change the password of yourself or another user (if you are root): `$ passwd bob`

### Permissions

View permission of a specific file `ls -l /etc/shadow`.

On a Linux system, every file is owned by a user and a group user. There is also a third category of users, those that are not the user owner and don't belong to the group owning the file (everyone else). For each category of users, read, write and execute permissions can be granted or denied.

[Table 3-7. Access mode codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3805)

[Table 3-8. User group codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3825)

You should know what your user name is. If you don't, it can be displayed using the `id` command, which also displays the default group you belong to and eventually other groups of which you are a member. Your user name is also stored in the environment variable $USER, use `echo $USER`

#### chmod - Modifying permissions

There are two main ways of assigning permissions: Symbolic method and Numeric method.

**Symbolic method**

- u,g,o,a (user, group, other, all)
- +, -, = (add, remove, set exact)
- r, w, x (read, write, execute)

Example:

`chmod ug+rw test.txt` add the read and write permissions to a file named test.txt for user and group

**Numeric method**

This is the best way to learn and practice permissions.

For a directory (file thì tương tự):
- Start at 0
- 4: read-`r` permission lets you view or read the directory.
- 2: write-`w` permission means able to create file into that dir such as `touch`.
- 1: execute-`x` means being able to `cd` into that directory.

Example:

`-rw-r-x---` == (4+2)(4+1)(0)== `chmod 650 test.txt`

#### Ownership Permissions

In addition to modifying permissions on files, you can also modify the group and user ownership of the file as well.

set the owner of myfile to patty: `sudo chown patty myfile`

set the group of myfile to whales: `sudo chgrp whales myfile`

If you add a colon and groupname after the user you can set both the user and group at the same time: `sudo chown patty:whales myfile`

**References:**

[Changing Permissions Numerically](https://www.youtube.com/watch?v=0SGGCklKa1U&list=PLT98CRl2KxKHaKA9-4_I38sLzK134p4GJ&index=22)

[redhat guide](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit)

[Table 3-9. (Numeric) File protection with chmod](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3908)

### Special permission

Special permissions make up a fourth access level in addition to **user**, **group**, and **other**. Special permissions allow for additional privileges over the standard permission sets (as the name suggests). There is a special permission option for each access level.

**user + s (pecial)**

A file with **SUID** always executes as the user who owns the file, regardless of the user passing the command. If the file owner doesn't have execute permissions, then use an uppercase **S** here.

**group + s (pecial)**

Commonly noted as **SGID**, this special permission has a couple of functions:
- If set on a file, it allows the file to be executed as the **group** that owns the file (similar to SUID)
- If set on a directory, any files created in the directory will have their **group** ownership set to that of the directory owner

## Access Control Lists (ACLs)

## Configuration files

most configuration files are stored in the `/etc` directory.

[Table 3-3. Most common configuration files](https://tldp.org/LDP/intro-linux/html/sect_03_02.html#AEN2485)

## The most common devices

Devices, generally every peripheral attachment of a PC that is not the CPU itself, is presented to the system as an entry in the `/dev` directory.

[Table 3-4. Common devices](https://tldp.org/LDP/intro-linux/html/sect_03_02.html#AEN2726)

# Window File system

In the Windows environment, you will find one of three file systems, each with different indexing methods. The first is known as the FAT (12, 16, or 32) file system. This system uses what is known as a **File Allocation Table** to index the files on the disc. This file allocation table is very simple to implement and use, but can be somewhat slow. It divides hard disks into one or more partitions (parts) that become letters, like C:, D:, etc.

The second file system you may encounter when using Windows is known as NTFS (New Technology File System). NTFS uses binary trees that, while complex, allow for very fast access times. It builds on the features of FAT, adds new features, and changes a few others. It is a recoverable file system, which means that it keeps track of actions in the file system.

The third file system, exFAT, is a lightweight file system used primarily in flash storage applications and SD cards. It has large file size and partition size limits, which means you can store files over 4GB on a flash drive or SD card that is formatted with exFAT.

# MacOS File system

[macOS Standard Directories: Where Files Reside](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW6)

In the macOS environment you will find the HFS+ file system. **HFS Plus** or **HFS +** uses something known as a B-Tree to index the files on the hard discs. B-Trees (which are different than a binary tree) allow for fast access time, much like the binary tree.  As of June 2016, Apple has implemented their new file system “APFS,” which also uses the B-Tree to index files.  This has become the replacement for HFS+ on macOS High Sierra and onward, as well as iOS 10.3+.

The `local domain` contains resources such as apps that are local to the current computer and shared among all users of that computer.
- `/Applications` chứa app tải về trên mạng

The `system domain` contains the system software installed by Apple. The resources in the system domain are required by the system to run. Users cannot add, remove, or alter items in this domain.