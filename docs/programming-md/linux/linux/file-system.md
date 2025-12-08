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

Depending on which type of Linux environment you are running, you may run into several different file systems. Some of them are **ext2, ext3, and ext4**. **XFS, JFS**, and a few others are also used. `ext3` is a journaling extension to the ext2 file system on Linux. ext4 is the successor to ext3. Journaling is a method of recording data that results in massively reduced time spent recovering a file system after a crash. XFS is very fast and also uses B-Trees for its file indexing.

These days, the most commonly used Linux file system is ext4. But Linux can also work with storage drives that were formatted using file systems from other platforms like FAT32 and NTFS.

In the shell and in scripts, spaceful filenames can be quoted to keep their pieces together. For example, the command:

`$ less "My excellent file.txt"`

preserves `My excellent file.txt` as a single argument to less. You can also escape individual spaces with a backslash.

## Terminologies

- File tree: the overall layout
- filesystem: the chunks attached to the tree

- `/`: the root directory/filesystem
- `/root`: the home directory of the root user

## Mounting

- File tree: the overall layout
- filesystem: the chunks attached to the tree

In most situations, filesystems are attached to the tree with the `mount` command. mount maps a directory within the existing file tree, called the **mount point**, to the root of the newly attached filesystem.

`$ sudo mount /dev/sda4 /users`

installs the filesystem stored on the disk partition represented by `/dev/sda4` under the path /users. You could then use ls /users to see that filesystem’s contents.

A list of the filesystems that are customarily mounted on a particular system is kept in the `/etc/fstab` file.  
The information contained in this file allows filesystems to be checked (with `fsck`) and mounted (with mount) automatically at boot time

You detach filesy stems with the `umount` command. umount complains if you try to unmount a filesystem that is in use; the filesystem to be detached must not have open files or processes whose current directories are located there, and if the file-system contains executable programs, they cannot be running.

Device files defined based on the controllers they are using:

1. For [IDE controllers device](https://www.ibm.com/support/pages/ide-controllers-servers) file name is - `hda, hdb, hdc..`
2. For SCSI and SATA controllers device file name is - `sda, sdb, sdc..`

If the first storage device on a system is called /dev/sda, then, as you might guess, the second one would be called /dev/sdb and the third, /dev/sdc.

Originally, `sda` probably stood for SCSI Device A, but I find that thinking of it as Storage Device A makes it more meaningful. You might also run into device designations like /dev/hda (hard drive), /dev/sr0 (DVD drive), `/dev/cdrom` (that’s right, a CD-ROM drive), or even /dev/fd0 (floppy drive).

---

Don’t happen to know your drive designation? No problem. Knowing that Linux organizes attached storage as block devices, you can move to the /sys/block/ directory and list its contents. Among the contents will be a directory called sda/. (Remember that sda stands for Storage Drive A.) That’s the first drive used by your system on boot:

```
$ cd /sys/block
$ ls
loop0  loop1  loop2 sda  sr0
```

A loop device is a pseudo device that allows a file to be used as though it’s an actual physical device.

Change to the sda/ directory and run ls. Among its contents, you’ll probably see files with names like sda1, sda2, and sda5. Each of these represents one of the partitions created by Linux to better organize the data on your drive:

```
$ cd sda
$ ls
alignment_offset  discard_alignment  holders    range      sda3       trace
bdi               events             inflight   removable  size       uevent
capability        events_async       integrity  ro         slaves
dev               events_poll_msecs  power      sda1       stat
device            ext_range          queue      sda2       subsystem
```

## The Organization of the File Tree

- Những directories `*/bin` chứa binaries, bash scripts, executables.
- Store your scripts in `/usr/local/bin`, unless you don't want other users to have access to them, in which case `$HOME/bin`.
- `/usr/local/bin` may be in the default PATH, but `$HOME/bin` will certainly need to be added to PATH manually.

Adding `$HOME/bin` to PATH:

```bash
PATH=${PATH}:$HOME/bin
export PATH
```

Khi cắm USB external hard drive vào ubuntu thì nó sẽ ở `/media/anhao/<hard drive name>`

---

The `/etc` directory for critical system, startup and **configuration files** that define the way individual programs and services function.

Originally, there was `/bin` for programs (essentially, executable binaries), and very soon `/dev` for device files and `/lib` for extra executable code loaded by programs (libraries). `/usr` also came in very early, first for user data, then as an extra OS area with its own `bin` and `lib` and then `man` containing the manual in electronic form. The source code was also often provided somewhere under /usr.

And there were a few files in the operating system that didn't fit in any of the existing categories. This included a passwd file containing users' passwords, and an mtab file written by mount, and the init and later rc programs executed at boot time, and over time more and more programs that were intended to be executed only for administration purpose and not as part of normal usage.

On modern unix systems, almost all system-wide configuration files are under /etc, but not all files in /etc are configuration files. `/etc` is for for critical system and configuration files.

---

`/sbin` and `/bin` for important utilities  

- `/bin` Core operating system commands; user binary files
- `/sbin` Commands needed for minimal system operability; system binary files

`/boot`Kernel and files needed to load the kernel

`/tmp` for temporary files

`/dev` is usually a real directory that’s included in the root filesystem, but some or all of it may be overlaid with other filesystems if your system has virtualized its device support.

Some systems keep **shared library files** and a few other odd things such as the C preprocessor in the `/lib` directory. Others have moved these items into `/usr/lib`, sometimes leaving `/lib` as a symbolic link

`/lib` Libraries, shared libraries, and parts of the C compiler

`/tmp` Temporary files that may disappear between reboots

`/media` Mount points for filesystems on removable media

`/mnt` Temporary mount points, mounts for removable media

`/opt` Optional software packages (not consistently used)

`/proc` Information about all running processes

`/var` store system-specific data, frequently changing content (such as log files)

`/usr` Contains non-essential command-line binaries, libraries, header files, **third-party binaries** and other data. At least it is non-essential to the system. The dotfiles is actually essential to the users.

The directories /usr and /var are also of great importance. `/usr` is where most standard programs are kept, along with various other booty such as on-line manuals and most libraries. It is not strictly necessary that /usr be a separate filesystem, but for convenience in administration it often is. Both /usr and /var must be available to enable the system to come up all the way to multiuser mode.

`/var` houses spool directories, log files, accounting information, and various other items that grow or change rapidly and that vary on each host. Since /var contains log files, which are apt to grow in times of trouble, it’s a good idea to put /var on its own filesystem if that is practical.

Home directories of users are often kept on a separate filesystem (`/home`), usually one that’s mounted in the root directory.

- `/usr/bin` Most commands and executable files
- `/usr/include` Header files for compiling C programs
- `/usr/lib` Libraries; also, support files for standard programs
- `/usr/lib64` 64-bit libraries on 64-bit Linux distributions
- `/usr/local` Software you write or install; mirrors structure of /usr
- `/usr/sbin` Less essential commands for administration and repair

## File Types

Most filesystem implementations define seven types of files. Even when develop-ers add something new and wonderful to the file tree (such as the process infor-mation under /proc), it must still be made to look like one of these seven types.

1. Regular files
2. Directories
3. Character device files
4. Block device files
5. Local domain sockets
6. Named pipes (FIFOs)
7. Symbolic links

You  c a n  d e t e r m i n e  t h e  t y p e  of  a n  e x i s t i n g  file with  `ls -ld`. The first character of the ls output encodes the type. For example, the following command demonstrates that `/usr/include` is a directory:

```bash
$ ls -ld /usr/include 
drwxr-xr-x   27 root     root         4096  Jul 15 20:57  /usr/include
```

ls uses the codes shown in Table below to represent the various types of files.

| File Type             | Symbol | Created by        | Removed by   |
|-----------------------|--------|-------------------|--------------|
| Regular file          | -      | editors, cp, etc. | rm           |
| Directory             | d      | mkdir             | rmdir, rm -r |
| Character device file | c      | mknod             | rm           |
| Block device file     | b      | mknob             | rm           |
| Local domain socket   | s      | socket(2)         | rm           |
| Named pipe            | p      | mknod             | rm           |
| Symbolic link         | l      | ln -s             | rm           |

When you use pattern matching (shell globbing), it’s a good idea to get in the habit of using the `-i` option to rm to make rm confirm the deletion of each file. This feature protects you against deleting any “good” files that your pattern inadvertently matches. For example, to delete a file named `foo<Control-D>bar`, you could use:

```bash
$ ls 
foo?bar foose kde-root

$ rm -i foo* 
rm: remove 'foo\004bar'? y rm: remove 'foose'? n
```

---

Regular files consist of a series of bytes; filesystems impose no structure on their contents. Text files, data files, executable programs, and shared libraries are all stored as regular files. Both sequential access and random access are allowed.

---

A directory contains named references to other files. You can create directories with mkdir and delete them with rmdir if they are empty. You can delete non-empty directories with `rm -r`.

A file’s name is stored within its parent directory, not with the file itself. In fact, more than one directory (or more than one entry in a single directory) can refer to a file at one time, and the references can have different names. Such an arrange-ment creates the illusion that a file exists in more than one place at the same time.

These additional references (“links,” or **hard links** to distinguish them from symbolic links, discussed below) are synonymous with the original file; as far as the filesystem is concerned, all links to the file are equivalent. The filesystem maintains a count of the number of links that point to each file and does not release the file’s data blocks until its last link has been deleted. Hard links cannot cross filesystem boundaries.

You  create h a rd  l i n k s  w it h  `ln` and remove them with `rm`. It’s easy to remember the syntax of ln if you keep in mind that it mirrors the syntax of `cp`. The command `cp oldfile newfile` creates a copy of `oldfile` called `newfile`, and `ln oldfile newfile` makes the name `newfile` an additional reference to `oldfile`. You can make hard links to directories as well as to flat files, but that’s less commonly done.

You  c a n  u s e  ls -l to see how many links to a given file exist.

Hard links are not a distinct type of file. Instead of defining a separate “thing” called a hard link, the filesystem simply allows more than one directory entry to point to the same file. In addition to the file’s contents, the underlying attributes of the file (such as ownerships and permissions) are also shared

---

Character and block device files

Device files let programs communicate with the system’s hardware and peripher-als. The kernel includes (or loads) driver software for each of the system’s devices. This software takes care of the messy details of managing each device so that the kernel proper can remain relatively abstract and hardware independent

Device drivers present a standard communication interface that looks like a regu-lar file. When the filesystem is given a request that refers to a character or block device file, it simply passes the request to the appropriate device driver. It’s impor-tant to distinguish device files from device drivers, however. The files are just ren-dezvous points that communicate with drivers. They are not drivers themselves.

---

Local domain sockets

Sockets are connections between processes that allow processes to communicate hygienically. UNIX defines several kinds of sockets, most of which involve the use of a network. Local domain sockets are accessible only from the local host and are referred to through a filesystem object rather than a network port. They are some-times known as “UNIX domain sockets.”

Although socket files are visible to other processes as directory entries, they can-not be read from or written to by processes not involved in the connection. Syslog and the X Window System are examples of standard facilities that use local do-main sockets.

Local domain sockets are created with the `socket` system call and removed with the rm command or the `unlink` system call once they have no more users.

---

Named pipes

Like local domain sockets, named pipes allow communication between two pro-cesses running on the same host. They’re also known as “FIFO files” (FIFO is short for the phrase “first in, first out”). You can create named pipes with mknod and remove them with rm.
As with local domain sockets, real-world instances of named pipes are few and far between. They rarely require administrative intervention.5
Named pipes and local domain sockets serve similar purposes, and the fact that both exist is essentially a historical artifact. Neither of them would exist if UNIX and Linux were designed today; network sockets would stand in for both.

---

symbolic links

A symbolic or “soft” link points to a file by name. When the kernel comes upon a symbolic link in the course of looking up a pathname, it redirects its attention to the pathname stored as the contents of the link. The difference between hard links and symbolic links is that a hard link is a direct reference, whereas a symbolic link is a reference by name. Symbolic links are distinct from the files they point to.

You  c re at e  s y m b o l i c  l i n k s  w it h  `ln -s` and remove them with rm. Since symbolic links can contain arbitrary paths, they can refer to files on other filesystems or to nonexistent files. Multiple symbolic links can also form a loop

A symbolic link can contain either an absolute or a relative path. For example,

`$ sudo ln -s archived/secure /var/log/secure`

links /var/log/secure to /var/log/archived/secure with a relative path. It creates the symbolic link /var/log/secure with a target of “archived/secure”,  as demonstrated by this output from ls:

```bash
$ ls -l /var/log/secure 
lrwxrwxrwx 1 root root 18 2009-07-05 12:54 /var/log/secure -> archived/secure
```

The entire /var/log directory could then be moved elsewhere without causing the symbolic link to stop working (not that moving this directory is advisable).

The file permissions that ls shows for a symbolic link, `lrwxrwxrwx`, are dummy values. Permission to create, remove, or follow the link is controlled by the containing directory, whereas read, write, and execute permission on the link target are granted by the target’s own permissions. Therefore, symbolic links do not need (and do not have) any permission information of their own.

It is a common mistake to think that the first argument to `ln -s` is interpreted relative to your current working directory. However, it is not resolved as a filename by ln; it’s simply a literal string that becomes the target of the symbolic link.

## Binary, Octal, Decimal, Hexadecimal number system

Binary numbers only use the digits 0 and 1.

Octal Number System has a base of eight and uses the numbers from 0 to 7.

The sequence `1 2 4 10 20 40 100 200 400` corresponds to the powers of 2 in decimal (base-10).

- `1` (octal) = 1 (decimal) = $2^0$
- `2` (octal) = 2 (decimal) = $2^1$
- `4` (octal) = 4 (decimal) = $2^2$
- `10` (octal) = $1 \times 8^1 + 0 \times 8^0 = 8$ (decimal) = $2^3$
- `20` (octal) = $2 \times 8^1 + 0 \times 8^0 = 16$ (decimal) = $2^4$
- `40` (octal) = $4 \times 8^1 + 0 \times 8^0 = 32$ (decimal) = $2^5$
- `100` (octal) = $1 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 64$ (decimal) = $2^6$
- `200` (octal) = $2 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 128$ (decimal) = $2^7$
- `400` (octal) = $4 \times 8^2 + 0 \times 8^1 + 0 \times 8^0 = 256$ (decimal) = $2^8$

## File Attributes & File Modes

Under the traditional UNIX and Linux filesystem model, every file has a set of **nine standard permission bits** that control who can read, write, and execute the contents of the file. Together with three other bits (Special Permission Bits, suid, sgui, sticky bit) that primarily affect the operation of executable programs, these bits constitute the file’s **mode**.

The twelve mode bits are stored together with four bits of file-type information. The four file-type bits are set when the file is first created and cannot be changed, but the file’s owner and the superuser can modify the twelve mode bits with the `chmod` (change mode) command. Use `ls -l` (or ls -ld for a directory) to inspect the values of these bits.

`ls -l` show file mode + file type

---

The permission bits

**Nine permission bits** determine what operations may be performed on a file and by whom. Traditional UNIX does not allow permissions to be set per-user (al-though all systems now support access control lists of one sort or another). Instead, three sets of permissions define access for the owner of the file, the group owners of the file, and everyone else (in that order). Each set has three bits: a read bit, a write bit, and an execute bit.

`r` là yes, cho phép read, `-` là no không cho phép => nên mới gọi là "bit"

Each user fits into only one of the three permission sets. The permissions used are those that are most specific. For example, the owner of a file always has access determined by the owner permission bits and never the group permission bits.

On a regular file, the read bit allows the file to be opened and read. The write bit allows the contents of the file to be modified or truncated; however, the ability to delete or rename (or delete and then recreate!) the file is controlled by the permis-sions on its parent directory because that is where the name-to-dataspace map-ping is actually stored.

The execute bit allows the file to be executed. Two types of executable files exist: binaries, which the CPU runs directly, and scripts, which must be interpreted by a shell or some other program.

For a directory, the execute bit (often called the “search” or “scan” bit in this con-text) allows the directory to be entered or passed through while a pathname is evaluated, but not to have its contents listed. The combination of read and execute bits allows the contents of the directory to be listed. The combination of write and execute bits allows files to be created, deleted, and renamed within the directory.

A variety of extensions such as access control lists complicate or override the traditional nine-bit permission model. If you’re having trouble explaining the system’s observed behavior, check to see whether one of these factors might be interfering.

Each permission (read, write, execute) is assigned a numeric value:

- Read (r) = 4
- Write (w) = 2
- Execute (x) = 1.

These values are summed for each user category to create a three-digit octal number.

Example:

- `rwx` (read, write, execute) = 4 + 2 + 1 = 7
- `rw-` (read, write, no execute) = 4 + 2 + 0 = 6
- `r-x` (read, no write, execute) = 4 + 0 + 1 = 5
- `---` (no permissions) = 0 + 0 + 0 = 0
-

Therefore, a common permission setting like rwxr-xr-x (owner has full permissions, group and others have read and execute) would be represented as 755 in octal notation.

---

The setuid and setgid bits

The bits with octal values 4000 and 2000 are the setuid and setgid bits. When set on executable files, these bits allow programs to access files and processes that would otherwise be off-limits to the user that runs them.

---

the sticky bit

The bit with octal value 1000 is called the sticky bit. It was of historical impor-tance as a modifier for executable files on early UNIX systems. However, that meaning of the sticky bit is now obsolete and modern systems silently ignore it.

View permission of a specific file `ls -l /etc/shadow`.

On a Linux system, every file is owned by a user and a group user. There is also a third category of users, those that are not the user owner and don't belong to the group owning the file (everyone else). For each category of users, read, write and execute permissions can be granted or denied.

[Table 3-7. Access mode codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3805)

[Table 3-8. User group codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3825)

You should know what your user name is. If you don't, it can be displayed using the `id` command, which also displays the default group you belong to and eventually other groups of which you are a member. Your user name is also stored in the environment variable $USER, use `echo $USER`

## `ls`: List and Inspect files

As a system administra-tor, you will be concerned mostly with the link count, owner, group, mode, size, last access time, last modification time, and type. You can inspect all of these with `ls -l` (or `ls -ld` for a directory; without the `-d` flag, ls lists the directory’s contents).

`ls -l` (l stands for "long") show file permissions, owner, group, file size, and time stamp.

The `h` argument when added to ls -l displays file sizes in a human-readable format—kilobytes, megabytes, and gigabytes, rather than bytes, which tend to involve a great many hard-to-count digits.  
`ls -l -h` == `ls -lh` == `ls -hl`  
`-h` == `--human-readable`

```
$ ls -lh /var/log
total 18M
-rw-r--r-- 1 root   root    0 May  3 06:25 alternatives.log
drwxr-xr-x 2 root   root 4.0K May  3 06:25 apt
[...]
```

Cái dòng đầu tiên `total 18M` là "the total disk space (in MB) consumed by files in this directory". Nếu không có `-h` thì total đơn vị là byte. Nó không phải là "number of files in this directory".

---

`ls -R` displays subdirectories and the files and subdirectories they contain, no matter how many nested layers of directories.

A description of the full functionality and features of the `ls` command can be read with `info coreutils ls`

To find out more about the kind of data we are dealing with, we use the `file` command. See `info file` for a detailed description.

In DOS (Windows), use `dir`

- `ls -l` notations:
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

`-a` show hidden dotfiles

An attribute change time is also maintained for each file. The conventional name for this time (the “ctime,” short for “change time”) leads some people to believe that it is the file’s creation time. Unfortunately, it is not; it just records the time that the attributes of the file (owner, mode, etc.) were last changed (as opposed to the time at which the file’s contents were modified).

Consider the following example:

```bash
$ ls -l /bin/gzip 
-rwxr-xr-x 3 root root 62100 May 28 2010 /bin/gzip
```

The first field specifies the file’s type and mode. The first character is a dash, so the file is a regular file.

The next nine characters in this field are the three sets of permission bits. The order is owner-group-other, and the order of bits within each set is read-write-execute. Although these bits have only binary values, ls shows them symbolically with the letters r, w, and x for read, write, and execute. In this case, the owner has all permissions on the file and everyone else has read and execute permission.

If the setuid bit had been set, the x representing the owner’s execute permission would have been replaced with an s, and if the setgid bit had been set, the x for the group would also have been replaced with an s. The last character of the permis-sions (execute permission for “other”) is shown as t if the sticky bit of the file is turned on. If either the setuid/setgid bit or the sticky bit is set but the correspond-ing execute bit is not, these bits appear as S or T.

The next field in the listing is the file’s link count. In this case it is 3, indicating that /bin/gzip is just one of three names for this file (the others are /bin/gunzip and /bin/zcat). Each time a hard link is made to a file, the file’s link count is incre-mented by 1. Symbolic links do not affect the link count.

All directories have at least two hard links: the link from the parent directory and the link from the special file “.” inside the directory itself.

The next two fields in the ls output are the owner and group owner of the file. In this example, the file’s owner is root, and the file also belongs to the group named root. The filesystem actually stores these as the user and group ID numbers rather than as names. If the text versions (names) can’t be determined, ls shows the fields as numbers. This might happen if the user or group that owns the file has been deleted from the /etc/passwd or /etc/group file. It could also indicate a problem with your NIS or LDAP database (if you use one)

The next field is the size of the file in bytes. This file is 62,100 bytes long. Next comes the date of last modification: May 28, 2010. The last field in the listing is the name of the file, `/bin/gzip`.

---

ls output is slightly different for a device file. For example:

```bash
$ ls -l /dev/tty0 
crw-rw---- 1 root root 4, 0 Jun 11 20:41 /dev/tty0
```

Most fields are the same, but instead of a size in bytes, ls shows the major and minor device numbers. /dev/tty0 is the first virtual console on this (Red Hat) sys-tem and is controlled by device driver 4 (the terminal driver).

One ls option that’s useful for scoping out hard links is -i, which makes ls show each file’s “inode number.” Without going into too much detail about filesystem implementations, we’ll just say that the inode number is an index into a table that enumerates all the files in the filesystem. Inodes are the “things” that are pointed to by directory entries; entries that are hard links to the same file have the same inode number. To figure out a complex web of links, you need both `ls -li` to show link counts and inode numbers and `find` to search for matches.

Some other ls options that are important to know are `-a` to show all entries in a directory (even files whose names start with a dot), -t to sort files by modification time (or -tr to sort in reverse chronological order), -F to show the names of files in a way that distinguishes directories and executable files, -R to list recursively, and -h to show file sizes in human-readable form (e.g., 8K or 53M).

## stat

Every object within a Linux file system is represented by a unique collection of metadata called an inode. I suppose you could say that the file system index discussed earlier is built from the metadata associated with all the many inodes on a drive.

To display inode information of a file, use `stat`:

```
$ stat myfile
  File: 'myfile'
  Size: 0             Blocks: 0          IO Block: 4096   regular empty file
Device: 802h/2050d    Inode: 55185258    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/  ubuntu)
                           Gid: ( 1000/  ubuntu)
Access: 2017-06-09 13:21:00.191819194 +0000
Modify: 2017-06-09 13:21:00.191819194 +0000
Change: 2017-06-09 13:21:00.191819194 +0000
 Birth: -
```

It’s important to be aware that when you move, copy, or delete a file or directory, all you’re really doing is editing its inode attributes, not its inode ID.

An inode, by the way, is an object used by UNIX systems to identify the disk location and attributes of files within a file system. Usually there’ll be exactly one inode for each file or directory.

## `chmod`: Change Permissions

Only the owner of the file and the superuser can change its permissions.

There are two main ways of assigning permissions: Symbolic method and Numeric method.

The octal syntax is generally more convenient for administrators, but it can only be used to specify an absolute value for the permission bits. The mnemonic syntax can modify some bits while leaving others alone.

The first argument to chmod is a specification of the permissions to be assigned, and the second and subsequent arguments are names of files on which permis-sions should be changed. In the octal case, the first octal digit of the specification is for the owner, the second is for the group, and the third is for everyone else.

Symbolic (mnemonic) method

- u,g,o,a (user, group, other, all)
- +, -, = (add, remove, set exact)
- r, w, x (read, write, execute)

The hard part about using the mnemonic syntax is remembering whether o stands for “owner” or “other”; “other” is correct. Just remember u and g by anal-ogy to UID and GID; only one possibility is left.

Example:

- `chmod ug+rw test.txt` add the read and write permissions to a file named test.txt for user and group

---

Octal syntax

This is the best way to learn and practice permissions.

For a directory (file thì tương tự):

- Start at 0
- 4: read-`r` permission lets you view or read the directory.
- 2: write-`w` permission means able to create file into that dir such as `touch`.
- 1: execute-`x` means being able to `cd` into that directory.

Example:

- `-rw-r-x---` == (4+2)(4+1)(0)== `chmod 650 test.txt`
- `chmod 711 myprog` gives all permissions to the owner and execute-only permission to everyone else.

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

user + s (pecial)

A file with **SUID** always executes as the user who owns the file, regardless of the user passing the command. If the file owner doesn't have execute permissions, then use an uppercase **S** here.

group + s (pecial)

Commonly noted as **SGID**, this special permission has a couple of functions:

- If set on a file, it allows the file to be executed as the **group** that owns the file (similar to SUID)
- If set on a directory, any files created in the directory will have their **group** ownership set to that of the directory owner

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

## Access Control Lists (ACLs)

j

## Pseudo file systems

A normal file is a collection of data that can be reliably accessed over and over again, even after a system reboot. By contrast, the contents of a Linux pseudo (or virtual) file, like those that might exist in the `/sys/` and `/proc/` directories, don’t really exist in the normal sense. A pseudo file’s contents are dynamically generated by the OS itself to represent specific values.

## Configuration files

most configuration files are stored in the `/etc` directory.

[Table 3-3. Most common configuration files](https://tldp.org/LDP/intro-linux/html/sect_03_02.html#AEN2485)

## Archive

An archive is a single file containing a collection of objects: files, directories, or a combination of both. Bundling objects within a single file (as illustrated in figure 4.1) sometimes makes it easier to move, share, or store multiple objects that might otherwise be unwieldy and disorganized.

You might need to create copies of directories and their contents so you can easily share or back them up. For that, `tar` is probably going to be your champion of choice. If, however, you need an exact copy of a partition or even an entire hard disk, then you’ll want to know about `dd`. And if you’re looking for an ongoing solution for regular system backups, then try `rsync`.

Don’t confuse archiving with compression. **Compression** is a software tool that applies a clever algorithm to a file or archive to reduce the amount of disk space it takes. Of course, when they’re compressed, files are unreadable, which is why the algorithm can also be applied in reverse to decompress them.

As you’ll see soon, applying compression to a tar archive is simple and doing so is a particularly good idea if you’re planning to transfer large archives over a network. Compression can reduce transmission times significantly.

---

## The Most common devices

Devices, generally every peripheral attachment of a PC that is not the CPU itself, is presented to the system as an entry in the `/dev` directory.

[Table 3-4. Common devices](https://tldp.org/LDP/intro-linux/html/sect_03_02.html#AEN2726)

## Window Subsystem for Linux

In the Windows environment, you will find one of three file systems, each with different indexing methods. The first is known as the FAT (12, 16, or 32) file system. This system uses what is known as a **File Allocation Table** to index the files on the disc. This file allocation table is very simple to implement and use, but can be somewhat slow. It divides hard disks into one or more partitions (parts) that become letters, like C:, D:, etc.

The second file system you may encounter when using Windows is known as NTFS (New Technology File System). NTFS uses binary trees that, while complex, allow for very fast access times. It builds on the features of FAT, adds new features, and changes a few others. It is a recoverable file system, which means that it keeps track of actions in the file system.

The third file system, exFAT, is a lightweight file system used primarily in flash storage applications and SD cards. It has large file size and partition size limits, which means you can store files over 4GB on a flash drive or SD card that is formatted with exFAT.

- Your Windows filesystem is mounted under `/mnt/c/`. So for example:
  - Your Windows Desktop: `/mnt/c/Users/<YourWindowsName>/Desktop`.
  - Your Documents: `/mnt/c/Users/<YourWindowsName>/Documents`.

Where linux actually live on Windows: `C:\Users\<YourWindowsName>\AppData\Local\Packages\<DistroName>\LocalState\ext4.vhdx`.  
That’s a virtual Linux disk. You shouldn’t modify it directly from Windows — only through WSL — or you risk corrution.

You can create a symbolic link in WSL: `ln -s /mnt/c/Users/<YourWindowsName>/Desktop ~/desktop`.

Or open the current WSL directory in File Explorer: `explorer.exe .`

### Máy everrise

`C:\Users\anhao\AppData\Local\Programs\`

MySQL: 3306

## MacOS File system

[macOS Standard Directories: Where Files Reside](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW6)

In the macOS environment you will find the HFS+ file system. **HFS Plus** or **HFS +** uses something known as a B-Tree to index the files on the hard discs. B-Trees (which are different than a binary tree) allow for fast access time, much like the binary tree.  As of June 2016, Apple has implemented their new file system “APFS,” which also uses the B-Tree to index files.  This has become the replacement for HFS+ on macOS High Sierra and onward, as well as iOS 10.3+.

The `local domain` contains resources such as apps that are local to the current computer and shared among all users of that computer.

- `/Applications` chứa app tải về trên mạng

The `system domain` contains the system software installed by Apple. The resources in the system domain are required by the system to run. Users cannot add, remove, or alter items in this domain.
