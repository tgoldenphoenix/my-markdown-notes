# The File Systems

## Terminologies

- File tree: the overall layout
- filesystem: the chunks attached to the tree

- `/`: the root directory/filesystem
- `/root`: the home directory of the root user

## Basics

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

`.` and `..` acts as hard links.

## Working with Files & Directories

- `rm` to remove files
- `rmdir` to remove empty directories. It can only remove empty directories.

`rm -r directory-name` remove directory

(Use `ls -a` to check whether a directory is empty or not). The rm command also has options for removing non-empty directories with all their subdirectories, read the Info pages for these rather dangerous options.\
The interactive behavior of the rm, cp and mv commands can be activated using the -i option. In that case the system won't immediately act upon request. Instead it will ask for confirmation, so it takes an additional click on the Enter key to inflict the damage. Customize your shell environment to make this option the default.

`rm file1` delete file1 from the directory

`rm file*` deletes all files in the current directory whose names begin with the letters file.

`rm -r *` wipe out the current directory

When you use pattern matching (shell globbing), it’s a good idea to get in the habit of using the `-i` option to `rm` to make `rm` confirm the deletion of each file. This feature protects you against deleting any “good” files that your pattern inadvertently matches. For example, to delete a file named `foo<Control-D>bar`, you could use:

```bash
$ ls 
foo?bar foose kde-root

$ rm -i foo* 
rm: remove 'foo\004bar'? y rm: remove 'foose'? n
```

`touch <file name>`

`touch file{1..10}` Creates 10 files named file1 to file10

“Touching” an existing file with touch updates its time stamp without making any changes. This can be useful if, for some reason, you want to change how various commands like ls list or display a file. (It can also be helpful if you’d like your boss to think that you’ve been hard at work on a data file that, in fact, you haven’t opened for weeks.)

Tên file có space thì phải để trong double quote.

`less "My excellent file.txt"`

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

## Viewing File Content: `cat`, `less`, `head` & `tail`

`cat` is sort for "concatenate". It allows you to concatenate multiple files together and have the aggregate input piped into another command.

quicky add line number counts by using `-n` flag.

`cat` (concatenate) print a file to the screen where it can be read, but not edited. This works pretty well for shorter documents.

If the file you want to read contains more lines than will display in a single screen, you don't use `cat`.

---

> less is more

[less](https://en.wikipedia.org/wiki/Less_(Unix)) is a terminal pager program used to view (but not change) the contents of a text file one screen at a time. It is similar to `more`, but has the extended capability of allowing both forward and backward navigation through the file.

`less` is the GNU version of `more` and has extra features allowing highlighting of search strings, scrolling back etc.

Khi dùng `less cat more` để view file, `Shift G` move to end of the file. Also works in man pages, vim.

`q` to quit less

`spacebar` next page, `b` to go back one page

`h` open documentation of `less`

`/` to search inside `less`

less support vim key-bindings.

`^` means Ctrl

`^F` `Ctrl F` Forward one window

---

head & tail

`head` selectively output only the first few lines of a file or stream. Ex: the first 10 lines.

`-n` negative number thì count backward from the end of the file

`head` can also print out the first few character instead of the first few lines.

`tail` does the opposite of `head`.

The tail command has a handy feature to continuously show the last n lines of a file that changes all the time. This `-f` option is often used by system administrators to check on log files.

After your system is running, many kernel messages are sent to the `/var/log/messages` file. So, for example, if you want to see what happens when you plug in a USB drive, you can type `tail -f /var/log/messages` and watch as devices and mount points are created.

---

> read the beginning or end of a file

These commands display **ten lines** by default.

For interactive use, `head` is more or less obsoleted by the `less` command, which paginates files for display. But `head` still finds plenty of use within scripts.

In practice, you should only use `less`. Don't use `more` & `head`.

Instead of exiting immediately after printing the requested number of lines, `tail -f` waits for new lines to be added to the end of the file and prints them as they appear— great for monitoring log files.  
Type `<Control-C>` to stop monitoring.

## Mounting

- File tree: the overall layout
- Filesystems: the chunks attached to the tree

In most situations, filesystems are attached to the tree with the `mount` command. `mount` maps a directory within the existing file tree, called the **mount point**, to the root of the newly attached filesystem.

`$ sudo mount /dev/sda4 /users`

installs the filesystem stored on the disk partition represented by `/dev/sda4` under the path /users. You could then use ls /users to see that filesystem’s contents.

A list of the filesystems that are customarily mounted on a particular system is kept in the `/etc/fstab` file.  
The information contained in this file allows filesystems to be checked (with `fsck`) and mounted (with mount) automatically at boot time

You detach filesy stems with the `umount` command.  
`umount` complains if you try to unmount a filesystem that is in use; the filesystem to be detached must not have open files or processes whose current directories are located there, and if the file-system contains executable programs, they cannot be running.

Device files are defined based on the controllers they are using:

1. For [IDE controllers device](https://www.ibm.com/support/pages/ide-controllers-servers) file name is - `hda, hdb, hdc..`
2. For SCSI and SATA controllers device file name is - `sda, sdb, sdc..`

If the first storage device on a system is called /dev/sda, then, as you might guess, the second one would be called /dev/sdb and the third, /dev/sdc.

Originally, `sda` probably stood for SCSI Device A, but I find that thinking of it as Storage Device A makes it more meaningful. You might also run into device designations like /dev/hda (hard drive), /dev/sr0 (DVD drive), `/dev/cdrom` (that’s right, a CD-ROM drive), or even /dev/fd0 (floppy drive).

---

Don’t happen to know your drive designation? No problem. Knowing that Linux organizes attached storage as block devices, you can move to the /sys/block/ directory and list its contents. Among the contents will be a directory called sda/. (Remember that sda stands for Storage Drive A.) That’s the first drive used by your system on boot:

```bash
$ cd /sys/block
$ ls
loop0  loop1  loop2 sda  sr0
```

A loop device is a pseudo device that allows a file to be used as though it’s an actual physical device.

Change to the sda/ directory and run ls. Among its contents, you’ll probably see files with names like sda1, sda2, and sda5. Each of these represents one of the partitions created by Linux to better organize the data on your drive:

```bash
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

- `/sbin` and `/bin` for important utilities, administrative commands
  - `/bin`: Core operating system commands; user binary files
  - `/sbin`: Commands needed for minimal system operability; system binary files

`/boot` Kernel and files needed to load the kernel.

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

`/var` houses spool directories, log files, accounting information, and various other items that grow or change rapidly and that vary on each host. Since /var contains log files, which are apt to grow in times of trouble, it’s a good idea to put /var on its own filesystem if that is practical.

Home directories of users are often kept on a separate filesystem (`/home`), usually one that’s mounted in the root directory.

### `/usr`

`/usr` Contains non-essential command-line binaries, libraries, header files, **third-party binaries** and other data. At least it is non-essential to the system. The dotfiles is actually essential to the users.

The directories `/usr` and `/var` are also of great importance. `/usr` is where most standard programs are kept, along with various other booty such as on-line manuals and most libraries. It is not strictly necessary that /usr be a separate filesystem, but for convenience in administration it often is. Both /usr and /var must be available to enable the system to come up all the way to multiuser mode.

- `/usr/bin` Most commands and executable files
- `/usr/include` Header files for compiling C programs
- `/usr/lib` Libraries; also, support files for standard programs
- `/usr/lib64` 64-bit libraries on 64-bit Linux distributions
- `/usr/local` Software you write or install; mirrors structure of /usr
- `/usr/sbin` Less essential commands for administration and repair. `/sbin` có thể symbolic link to `/usr/sbin`.

### `/etc`

Most system-wide configuration files are stored in the `/etc` directory. Personal config files specific to user thì store trong home directory của user (the dotfiles inside `/home/anhao`).

[Table 3-3. Most common configuration files](https://tldp.org/LDP/intro-linux/html/sect_03_02.html#AEN2485)

if you were to look at the contents of `/etc/passwd`, you’d see a single line for every account that exists.

`syslog` is a system user.

Once upon a time, an encrypted version of each user’s password would also have been included here. For practical reasons, because the passwd file must remain readable by anyone on the system, it was felt that including even encrypted passwords was unwise. Those passwords were moved to `/etc/shadow`. Using `sudo` permissions, you should take a look at that file with its encrypted passwords on your own system. Here’s how:

`sudo cat /etc/shadow`

---

`/etc/cron*`: Directories in this set contain files that define how the crond utility runs applications on a daily (`cron.daily`), hourly (`cron.hourly`), monthly (cron. monthly), or weekly (cron.weekly) schedule.

Scripts saved to the `/etc/cron.daily/` directory will be executed each day.

---

`/etc/default`: Contains files that set default values for various utilities. For example, the file for the useradd command defines the default group number, home direc-tory, password expiration date, shell, and skeleton directory (/etc/skel) used when creating a new user account.

`/etc/httpd`: Contains a variety of files used to configure the behavior of your Apache web server (specifically, the httpd daemon process). (On Ubuntu and other Linux systems, /etc/apache or `/etc/apache2` is used instead.

`/etc/skel`: Any files contained in this directory are automatically copied to a user’s home directory when that user is added to the system. By default, most of these files are dot (.) files, such as .kde (a directory for setting KDE desktop defaults) and .bashrc (for setting default values used with the bash shell).

`/etc/systemd`: Contains files associated with the systemd facility, for managing the boot process and system services. In particular, when you run systemctl com-mands to enable and disable services, files that make that happen are stored in sub-directories of the `/etc/systemd` system directory.

- The following are some interesting configuration files in `/etc`:
  - `bashrc`: Sets system-wide defaults for bash shell users. (This may be called `bash.bashrc` on some Linux distributions.)
  - `crontab`: Sets times for running automated tasks and variables associated with the cron facility (such as the SHELL and PATH associated with cron).
  - `fstab`: Identifies the devices for common storage media (hard disk, DVD, CD-ROM, and so on) and locations where they are mounted in the Linux system. This is used by the mount command to choose which filesystems to mount when the system first boots.
  - `mtab`: Contains a list of filesystems that are currently mounted.
  - `group`: Identifies group names and group IDs (GIDs) that are defined on the system.
  - `gshadow`: Contains shadow passwords for groups.

## File Types

Most filesystem implementations define seven types of files. Even when developers add something new and wonderful to the file tree (such as the process infor-mation under /proc), it must still be made to look like one of these seven types.

1. Regular files
2. Directories
3. Character device files
4. Block device files
5. Local domain sockets
6. Named pipes (FIFOs)
7. Symbolic links

You can determine the type of an existing file with `ls -ld`. The first character of `ls` output show the type. For example, the following command demonstrates that `/usr/include` is a directory:

```bash
$ ls -ld /usr/include 
drwxr-xr-x   27 root     root         4096  Jul 15 20:57  /usr/include
```

`ls` uses the codes shown in Table below to represent the various types of files.

| File Type             | Symbol | Created by        | Removed by   |
|-----------------------|--------|-------------------|--------------|
| Regular file          | -      | editors, cp, etc. | rm           |
| Directory             | d      | mkdir             | rmdir, rm -r |
| Character device file | c      | mknod             | rm           |
| Block device file     | b      | mknob             | rm           |
| Local domain socket   | s      | socket(2)         | rm           |
| Named pipe            | p      | mknod             | rm           |
| Symbolic link         | l      | ln -s             | rm           |

---

Regular files consist of a series of bytes; filesystems impose no structure on their contents. Text files, data files, executable programs, and shared libraries are all stored as regular files. Both sequential access and random access are allowed.

---

A `directory` contains named references to other files. You can create directories with mkdir and delete them with rmdir if they are empty. You can delete non-empty directories with `rm -r`.

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

## File Modes (Permissions)

Under the traditional UNIX and Linux filesystem model, every file has a set of **nine standard permission bits** that control who can read, write, and execute the contents of the file. Together with **three other bits** (aka Special Permission Bits), `suid`, `sgui`, `sticky bit`), that primarily affect the operation of executable programs, these bits constitute the `file’s mode`.

The twelve bits of file mode are stored together with another **four bits** of `file-type` information. The four file-type bits are set when the file is first created and **cannot be changed**, but the file’s owner and the superuser can modify the twelve mode bits with the `chmod` (change mode) command. Use `ls -l` (or `ls -ld` for a directory) to inspect the values of these bits.

---

`ls -l` show both file mode + file type

When you run `ls -la`, Linux converts 16 bits stored inside the file's inode (4-bit File Type and 12-bit File Mode) into the 10-character string at the far left of your screen (e.g., `-rwxr-xr-x` or `drwxrwxrwt`).

---

The permission bits

**Nine permission bits** determine what operations may be performed on a file and by whom. Traditional UNIX does not allow permissions to be set per-user (al-though all systems now support access control lists of one sort or another). Instead, three sets of permissions define access for the owner of the file, the group owners of the file, and everyone else (in that order). Each set has three bits: a read bit, a write bit, and an execute bit.

`r` là `yes`, cho phép read, `-` là `no` không cho phép => nên mới gọi là "bit"

Each user fits into only one of the three permission sets. The permissions used are those that are most specific. For example, the owner of a file always has access determined by the owner permission bits and never the group permission bits.

On a regular file, the read bit allows the file to be opened and read. The write bit allows the contents of the file to be modified or truncated; however, the ability to delete or rename (or delete and then recreate!) the file is controlled by the permis-sions on its parent directory because that is where the name-to-dataspace map-ping is actually stored.

- The execute bit allows the file to be executed. Two types of executable files exist:
  - binaries, which the CPU runs directly
  - and scripts, which must be interpreted by a shell or some other program.

For a **directory**, the execute bit (often called the “search” or “scan” bit in this context) allows the directory to be entered or passed through while a **pathname** is evaluated, but not to have its contents listed. The combination of read and execute bits allows the contents of the directory to be listed. The combination of write and execute bits allows files to be created, deleted, and renamed within the directory.

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

Therefore, a common permission setting like `rwxr-xr-x` (owner has full permissions, group and others have read and execute) would be represented as 755 in octal notation.

---

The `setuid` and `setgid` bits

The bits with octal values 4000 and 2000 are the setuid and setgid bits. When set on executable files, these bits allow programs to access files and processes that would otherwise be off-limits to the user that runs them.

---

the `sticky bit`

The bit with octal value 1000 is called the sticky bit. It was of historical impor-tance as a modifier for executable files on early UNIX systems. However, that meaning of the sticky bit is now obsolete and modern systems silently ignore it.

View permission of a specific file `ls -l /etc/shadow`.

On a Linux system, every file is owned by a user and a group user. There is also a third category of users, those that are not the user owner and don't belong to the group owning the file (everyone else). For each category of users, read, write and execute permissions can be granted or denied.

[Table 3-7. Access mode codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3805)

[Table 3-8. User group codes](https://tldp.org/LDP/intro-linux/html/sect_03_04.html#AEN3825)

You should know what your user name is. If you don't, it can be displayed using the `id` command, which also displays the default group you belong to and eventually other groups of which you are a member. Your user name is also stored in the environment variable $USER, use `echo $USER`

---

What’s a group?

You can think of a group much the same way you might think of a regular user account: the things that both can and cannot do or access are defined by file permissions. The difference is that no one can log in to a Linux system as a group. Then why create groups, and what purpose do they serve? Here’s the scoop.

Groups are a powerful and super-efficient way to organize resources. Here’s a simple example. Consider a company with a few dozen employees who need some kind of server access, but not necessarily to the same resources. You can create a couple of groups called dev and IT, for example. When users are initially given their accounts, all the developers would be added to the dev group, and all the sysadmins would be added to IT group. Now, let’s say that a system configuration file comes into use: rather than tediously adding file permissions for each of the 10 or 15 admins or so, you can give only the IT group access. All the IT group members will automatically be added, and all the developers will remain excluded.

Every system user along with many applications will automatically be given their own groups. That explains why files you create will normally be owned by `yourname` and be part of the `yourname` group.

### Special permission

Special permissions make up a fourth access level in addition to **user**, **group**, and **other**. Special permissions allow for additional privileges over the standard permission sets (as the name suggests). There is a special permission option for each access level.

user + s (pecial)

A file with **SUID** always executes as the user who owns the file, regardless of the user passing the command. If the file owner doesn't have execute permissions, then use an uppercase **S** here.

group + s (pecial)

Commonly noted as **SGID**, this special permission has a couple of functions:

- If set on a file, it allows the file to be executed as the **group** that owns the file (similar to SUID)
- If set on a directory, any files created in the directory will have their **group** ownership set to that of the directory owner

## `ls`: List and Inspect files

`ls` stands for list storage

`ls -l` có thể chỉnh alias thành `ll` (not an actual linux command). Hình như `la` cũng vậy. Nhưng mình không biết setting nằm ở file nào.

The following two uses of options for the ls command are the same:

```bash
ls -l -a -t
ls -lat
```

- `-a` show hidden dot files
- `-t` list by time

If you use the command `ls --help` but with single hyphen (`ls -help`), the letters `h, e, l`, and `p` would be interpreted as separate options.

---

- `ls -l` (l stands for "long").
- `ls -ld` list information of the current directory `.`. Without the `-d` flag, `ls` lists the directory’s contents

- The `-h` option when added to `ls -l` displays file sizes in a human-readable format—kilobytes, megabytes, and gigabytes, rather than bytes, which tend to involve a great many hard-to-count digits.  
- `ls -l -h` == `ls -lh` == `ls -hl`
- `-h` == `--human-readable`

```bash
$ ls -lh /var/log
total 18M
-rw-r--r-- 1 root   root    0 May  3 06:25 alternatives.log
drwxr-xr-x 2 root   root 4.0K May  3 06:25 apt
[...]
```

Cái dòng đầu tiên `total 18M` là "the total disk space (in MB) consumed by files in this directory". Nếu không có `-h` thì total đơn vị là byte. Nó không phải là "number of files in this directory".

The first `root` is owner name. The second `root` is group name.

---

`ls -R` displays subdirectories and the files and subdirectories they contain, no matter how many nested layers of directories.

A description of the full functionality and features of the `ls` command can be read with `info coreutils ls`

To find out more about the kind of data we are dealing with, we use the `file` command. See `info file` for a detailed description.

In DOS (Windows), use `dir` thay cho `ls`.

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

The next field in the listing is the file’s **link count**. In this case it is 3, indicating that /bin/gzip is just one of three names for this file (the others are /bin/gunzip and /bin/zcat). Each time a hard link is made to a file, the file’s link count is incre-mented by 1. Symbolic links do not affect the link count.

All directories have at least two hard links: the link from the parent directory and the link from the special file “.” inside the directory itself.

The next two fields in the `ls` output are the owner and group owner of the file. In this example, the file’s owner is root, and the file also belongs to the group named root. The filesystem actually stores these as the user and group ID numbers rather than as names. If the text versions (names) can’t be determined, ls shows the fields as numbers. This might happen if the user or group that owns the file has been deleted from the `/etc/passwd` or `/etc/group` file. It could also indicate a problem with your NIS or LDAP database (if you use one)

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

## `chmod`: Change Mode

Only the owner of the file and the `superuser` can change its permissions.

- There are two main ways of assigning permissions:
  - Symbolic method (mask, mnemonic)
  - Numeric method (octal)

The octal syntax is generally more convenient for administrators, but it can only be used to specify an absolute value for the permission bits. The mnemonic syntax can modify some bits while leaving others alone.

The first argument to `chmod` is a specification of the permissions to be assigned, and the second and subsequent arguments are names of files on which permissions should be changed.  
In the octal case, the first octal digit of the specification is for the owner, the second is for the group, and the third is for everyone else.

### The Symbolic (mnemonic) method

- `u g o a` (user, group, other, all)
- `+ - =` (add, remove, set exact)
- `r w x` (read, write, execute)

The hard part about using the mnemonic syntax is remembering whether `o` stands for “owner” or “other”; “other” is correct. Just remember `u` and `g` by analogy to UID and GID; only one possibility is left.

Example:

- `chmod ug+rw test.txt` add the read and write permissions to a file named `test.txt` for user and group
- `chmod u+x test.sh` adds execute (`+x`) permission for the User (`u`) AND the Group (`g`).
- `chmod +x test.sh` adds execute (`+x`) permission for All (a) categories (User, Group, and Others).

This example removes the ability of others (`o`) to read the file and adds write permissions for the group (`g`).

```bash
sudo chmod o-r /bin/zcat
sudo chmod g+w /bin/zcat
```

### The numeric (octal) syntax

This is the best way to learn and practice permissions.

Each possible combination of permissions can be represented by a number between 0 and 7 (octal digit).

For a directory (file thì tương tự):

- Start at 0
- `4` = read `r` permission; lets you view or read the directory. Because $2^2=4$.
- `2` = write `w` permission; means able to create file into that dir such as `touch`. Because $2^1=2$
- `1` = execute-`x` means being able to `cd` into that directory.

- A user with all three permissions is described by the number 7 ($4+2+1=7$).
- Read and write permissions, but not execute, is `6`
- Read and execute but not write is 5
- No permissions at all is `0`.

Example:

- `-rw-r-x---` = $(4+2)(4+1)(0)$ = `chmod 650 test.txt`
- `chmod 711 myprog` == `rwx--x--x` gives all permissions to the owner and execute-only permission to everyone else.

## Change File Ownership

In addition to modifying permissions on files, you can also modify the group and user ownership of the file as well.

set the owner of myfile to patty: `sudo chown patty myfile`

set the group of myfile to whales: `sudo chgrp whales myfile`

If you add a colon and groupname after the user you can set both the user and group at the same time: `sudo chown patty:whales myfile`

If you use `sudo` to create or copy a file, the owner of the new file will be `sudo` (or `root`), not you.

---

When you extract a `.tar` archive, all the files inside will be owned by you!

## `stat`

Every object within a Linux file system is represented by a unique collection of metadata called an `inode`. I suppose you could say that the file system index discussed earlier is built from the metadata associated with all the many inodes on a drive.

To display inode information of a file, use `stat`:

```bash
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

## Partitioning

Linux uses more than one partition on the same disk, even when using the standard installation procedure.

One of the goals of having different partitions is to achieve higher data security in case of disaster.

By dividing the hard disk in partitions, data can be grouped and separated. When an accident occurs, only the data in the partition that got the hit will be damaged, while the data on the other partitions will most likely survive. This principle dates from the days when Linux didn't have **journaled file systems** and power failures might have lead to disaster. This is currently the most important reason for partitioning.  
A simple example: a user creates a script, a program or a web application that starts filling up the disk. If the disk contains only one big partition, the entire system will stop functioning if the disk is full. If the user stores the data on a separate partition, then only that (data) partition will be affected, while the system partitions and possible other data partitions keep functioning.

- There are two kinds of major partitions on a Linux system:
  - `data partition`: normal Linux system data, including the root partition containing all the data to start up and run the system; and
  - `swap partition`: expansion of the computer's physical memory, extra memory on hard disk.

Most systems contain a `root partition`, one or more data partitions and one or more swap partitions. Systems in mixed environments may contain partitions for other system data, such as a partition with a `FAT` or `VFAT` file system for MS Windows data.

The standard **root partition** (indicated with a single forward slash, `/`) is about 100-500 MB, and contains the system configuration files, most basic commands and server programs, system libraries, some temporary space and the home directory of the administrative user. A standard installation requires about 250 MB for the root partition.

Swap space (indicated with `swap`) is only accessible for the system itself, and is hidden from view during normal operation. Swap is the system that ensures, like on normal UNIX systems, that you can keep on working, whatever happens. On Linux, you will virtually never see irritating messages like `Out of memory, please close some applications first and try again`, because of this extra memory. The swap or virtual memory procedure has long been adopted by operating systems outside the UNIX world by now.

Linux generally counts on having twice the amount of physical memory in the form of swap space on the hard disk. When installing a system, you have to know how you are going to do this. An example on a system with `512 MB` of RAM:

1. 1st possibility: one swap partition of 1 GB
2. 2nd possibility: two swap partitions of 512 MB
3. 3rd possibility: with two hard disks: 1 partition of 512 MB on each disk.

The kernel is on a separate partition in many distributions, because it is the most important file of your system. If this is the case, you will find that you also have a `/boot` partition, holding your kernel(s) and accompanying data files.  
You’ll sometimes see the `/boot/` directory given its own partition (the **kernel partition**). I personally think this is a **bad idea**, and I’ve got scars to prove it. The problem is that new kernel images are written to /boot/ and, as your system is upgraded to new Linux kernel releases, the disk space required to store all those images increases. If, as is a standard practice, you assign only 500 MB to the boot partition, you’ll have six months or so before it fills up—at which point updates will fail. You may be unable to fully boot into Linux before manually removing some of the older files and then updating the GRUB menu. If that doesn’t sound like a lot of fun, then keep your /boot/ directory in the largest partition.

The rest of the hard disk(s) is generally divided in data partitions, although it may be that all of the non-system critical data resides on one partition, for example when you perform a standard workstation installation. When non-critical data is separated on different partitions, it usually happens following a set pattern:

- a partition for user programs (`/usr`)
- a partition containing the users' personal data (`/home`)
- a partition to store temporary data like print- and mail-queues (_/var_)
- a partition for third party and extra software (`/opt`)

Everything is put together on one large partition, swap space twice the amount of RAM is added and your **generic workstation** is complete, providing the largest amount of disk space possible for personal use, but with the disadvantage of possible data integrity loss during problem situations.  
On a server, system data tends to be separate from user data. Programs that offer services are kept in a different place than the data handled by this service.

- In `sda1`, `sda2`, `sdb`:
  - `sd` = storage device
  - `a b c`: Device Identifier
  - `1 2 3`: Partition Number
- `/dev/sda`: Represents the first storage device detected by the operating system. Nó sẽ có `sda1` & `sda2`.
- `/dev/sdb`: Represents the second storage device detected.

The `/dev/` directory must be virtual because its contents are not persistent and do not represent data stored on a disk. Instead, the files are dynamic interfaces to the kernel's device drivers.

---

the `/boot/efi` directory almost always has its own separate partition, known as the `EFI System Partition (ESP)`.  
The reason the ESP must be separate from your main Linux root partition (/) or even your kernel partition (/boot) is that the UEFI firmware needs to access and execute files on this partition directly, before the main operating system kernel is loaded.

### `fdisk`

Most Linux systems use `fdisk` at installation time to set the partition type. The standard Linux partitions have number 82 for swap and 83 for data, which can be journaled (ext3) or normal (ext2, on older systems). The **fdisk** utility has built-in help, should you forget these values.

## `df`

`df` displays each partition (file systems) that’s currently mounted on a Linux system, along with its disk usage and location on the file system. Adding the `-h` flag converts partition sizes to human readable formats like GB or MB, rather than bytes

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       910G  178G  686G  21% /
none            492K     0  492K   0% /dev
tmpfs           3.6G     0  3.6G   0% /dev/shm
tmpfs           3.6G  8.4M  3.6G   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           3.6G     0  3.6G   0% /sys/fs/cgroup
```

- real file system: Data is written to and read from physical sectors on the storage media.
- Pseudo file system: file systems whose files aren’t actually saved to disk but live in volatile memory and disappear when the machine shuts down

It’s pretty simple to tell which partitions are used for pseudo files: if the file designation is `tmpfs` and the number of bytes reported in the Used column is 0, then the odds are you’re looking at a temporary rather than a normal file system.

---

There is a command called `lsblk` which stands for list block devices (an optical drive like a CD or DVD).

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

## Archive

An archive is a single file containing a collection of objects: files, directories, or a combination of both. Bundling objects within a single file (as illustrated in figure 4.1) sometimes makes it easier to move, share, or store multiple objects that might otherwise be unwieldy and disorganized.

You might need to create copies of directories and their contents so you can easily share or back them up. For that, `tar` is probably going to be your champion of choice.  
If, however, you need an exact copy of a partition or even an entire hard disk, then you’ll want to know about `dd`. And if you’re looking for an ongoing solution for regular system backups, then try `rsync`.

Don’t confuse archiving with compression. **Compression** is a software tool that applies a clever algorithm to a file or archive to reduce the amount of disk space it takes. Of course, when they’re compressed, files are unreadable, which is why the algorithm can also be applied in reverse to decompress them.

As you’ll see soon, applying compression to a tar archive is simple and doing so is a particularly good idea if you’re planning to transfer large archives over a network. Compression can reduce transmission times significantly.

---

`.iso` files were images of complete operating systems, specially organized to make it easy to copy the included files to a target computer.

## Archiving files and file systems using `tar`

In computing, `tar` is a computer software utility for collecting many files into one archive file, often referred to as a **tarball**, for distribution or backup purposes.  
The name is derived from **tape archive**, as it was originally developed to write data to sequential I/O devices with no file system of their own, such as devices that use **magnetic tape**.

- To successfully create your archive, there are three things that will have to happen:
  - Find and identify the files you want to include.
  - Identify the location on a storage drive that you want your archive to use.
  - Add your files to an archive, and save it to its storage location.

Want to knock off all three steps in one go? Use `tar`.

This example copies all the files and directories within and below the current work directory and builds an archive file that I’ve cleverly named `archivename.tar`. Here I use three arguments after the `tar` command:

- the `c` tells tar to create a new archive (compress)
- `v` sets the screen output to verbose so I’ll get updates
- and `f` points to the filename I’d like the archive to get

```bash
$ tar cvf archivename.tar *
file1 # The verbose argument (v) lists the names of all the files added to the archive.
file2
file3
```

The `tar` command will never move or delete any of the original directories and files you feed it; it only makes archived copies. You should also note that using a dot (.) instead of an asterisk (*) in the previous command will include even hidden files (whose filenames begin with a dot) in the archive.

The `.tar` filename extension isn’t necessary, but it’s always a good idea to clearly communicate the purpose of a file in as many ways as possible.

---

To extract the archive, run the tar command against the name of the archive, but this time with the argument x (for extract) rather than `c`:

`$ tar xvf stuff.tar`

**WARNING**: Extracting an archive overwrites any files with the same names in the current directory without warning. Here, that’s fine, but that won’t normally be the case.

---

You won’t always want to include all the files within a directory tree in your archive. Suppose you’ve produced some videos, but the originals are currently kept in directories along with all kinds of graphic, audio, and text files (containing your notes). The only files you need to back up are the final video clips using the .mp4 filename extension. Here’s how to do that:

`$ tar cvf archivename.tar *.mp4`

That’s excellent. But those video files are enormous. Wouldn’t it be nice to make that archive a bit smaller using compression? Say no more! Just run the previous command with the `z` (zip) argument. That will tell the `gzip` program to compress the archive. If you want to follow convention, you can also add a `.gz` extension in addition to the `.tar` that’s already there. Remember: clarity. Here’s how that would play out:

`$ tar czvf archivename.tar.gz *.mp4`

You may notice that the `.tar.gz` file isn’t all that much smaller than the .tar file, perhaps 10% or so. What’s with that? Well, the `.mp4` file format is itself compressed, so there’s a lot less room for gzip to do its stuff.

---

As tar is fully aware of its Linux environment, you can use it to select files and directories that live outside your current working directory. This example adds all the .mp4 files in the `/home/myuser/Videos/` directory:

`$ tar czvf archivename.tar.gz /home/myuser/Videos/*.mp4`

---

Because archive files can get big, it might sometimes make sense to break them down into multiple smaller files, transfer them to their new home, and then re-create the original file at the other end. The split tool is made for this purpose.

In this example, `-b` tells Linux to split the `archivename.tar.gz` file into 1 GB-sized parts; archivename is any name you’d like to give the file. The operation then names each of the parts—`archivename.tar.gz.partaa`, `archivename.tar.gz.partab`, `archivename .tar.gz.partac`, and so on:

`$ split -b 1G archivename.tar.gz "archivename.tar.gz.part"`

On the other side, you re-create the archive by reading each of the parts in sequence (`cat archivename.tar.gz.part*`), then redirect the output to a new file called `archivename.tar.gz`:

`$ cat archivename.tar.gz.part* > archivename.tar.gz`

### Streaming file system archives

```bash
# tar czvf - --one-file-system / /usr /var \
  --exclude=/home/andy/ | ssh username@10.0.3.141 \
  "cat > /home/username/workstation-backup-Apr-10.tar.gz"
```

```bash
$ tar czvf - importantstuff/ | ssh username@10.0.3.141 \
  "cat > /home/username/myfiles.tar.gz"
```

Let me explain that example. Rather than entering the archive name right after the command arguments (the way you’ve done until now), I used a dash (`czvf -`). The dash **outputs data to standard output**. It lets you push the archive filename details back to the end of the command and tells tar to expect the source content for the archive instead. I then piped (|) the unnamed, compressed archive to an ssh login on a remote server where I was asked for my password. The command enclosed in quotation marks then executed cat against the archive data stream, which wrote the stream contents to a file called `myfiles.tar.gz` in my home directory on the remote host.

One advantage of generating archives this way is that you avoid the overhead of a middle step. There’s no need to even temporarily save a copy of the archive on the local machine. Imagine backing up an installation that fills 110 GB of its 128 GB of available space. Where would the archive go?

### Aggregating files with `find`

The `find` command searches through a file system looking for objects that match rules you provide. The search outputs the names and locations of the files it discovers to what’s called standard output (`stdout`), which normally prints to the screen. But that output can just as easily be redirected to another command like tar, which would then copy those files to an archive.

Here’s the story. Your server is hosting a website that provides lots of .mp4 video files. The files are spread across many directories within the `/var/www/html/` tree, so identifying them individually would be a pain. Here’s a single command that will search the /var/www/html/ hierarchy for files with names that include the file extension .mp4. When a file is found, `tar` will be executed with the argument `-r` to append (as opposed to overwrite) the video file to a file called videos.tar:

```bash
sudo find /var/www/html/ -iname <1> "*.mp4" -exec tar \
 -rvf videos.tar {} \;
```

The `-iname` flag returns both upper- and lowercase results; -name, on the other hand, searches for case-sensitive matches.

The `{}` characters tell the find command to apply the tar command to each file it finds.

In this case, it’s a good idea to run find as sudo. Because you’re looking for files in system directories, it’s possible that some of them have restrictive permissions that could prevent find from reading and, thus, reporting them.

---

And, because we’re talking about find, I should also tell you about a similar tool called `locate` that will often be your first choice when you’re in a big hurry. By default, locate searches the entire system for files matching the string that you specify. In this case, locate will look for files whose names end with the string video.mp4 (even if they have any kind of prefix):

`$ locate *video.mp4`

If you run locate head-to-head against find, locate will almost always return results far faster. What’s the secret? locate isn’t actually searching the file system itself, but simply running your search string against entries in a preexisting index. The catch is that if the index is allowed to fall out of date, the searches become less and less accurate. Normally the index is updated every time the system boots, but you can also manually do the job by running `updatedb`: `sudo updatedb`.

## `find`, `locate`, `which`

These are the real tools, used when searching other paths beside those listed in the search path (using `which`).

`find` find files and directory in a directory hierarchy. This command not only allows you to search file names, it can also accept file size, date of last change and other file properties as criteria for a search. The most common use is for finding file names: `find <path> -name <searchstring>`\
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

- `find . -name *.py`
- `find . -not -name *.py`

`find . -not -name '*.py' -delete` => delete all, keep only `.py`. Use single quote to pass wildcard pattern _unchange_ in this case.

---

`locate`

Later on (in 1999 according to the man pages, after 20 years of `find`), `locate` was developed. This program is easier to use, but more restricted than find, since its output is based on a file index database that is updated only once every day. On the other hand, a search in the locate database uses less resources than find and therefore shows the results nearly instantly. Most Linux distributions use `slocate` these days, security enhanced locate, the modern version of locate that prevents users from getting output they have no right to read.  On most systems, locate is a symbolic link to the slocate program

---

- If you want to add your own commands or shell scripts, place them in the bin directory in your home directory (such as /home/chris/bin for the user named chris).
- To make commands available to all users, add them to `/usr/local/bin`.

Unlike some other operating systems, Linux does not, by default, check the current directory for an executable **before** searching the path. It immediately begins searching the path, and executables in the current directory are run only if they are in the PATH variable or you give their absolute (such as /home/chris/scriptx.sh) or relative (for example,  ./script x.sh) location.

The path directory order is important. Directories are checked from left to right. So, in this example, if there is a command called foo located in both the /usr/bin and /bin direc-tories, the one in /usr/bin is executed. To have the other foo command run, you either type the full path to the command or change your PATH variable.

`which <command>` locate the executable file associated with the given command.

`type` giống `which`

```bash
$ type bash
bash is /bin/bash

❯ type which
which is a shell builtin

❯ type case
case is a reserved word
```

---

If a command is not in your PATH variable, you can use the `locate` command to try to
find it. Using locate, you can search any part of the system that is accessible to you. (Some files are only accessible to the root user.) For example, if you wanted to find the loca-tion of the chage command, you could enter the following:

```bash
$ locate chage
/usr/bin/chage
/usr/sbin/lchage
/usr/share/man/fr/man1/chage.1.gz
/usr/share/man/it/man1/chage.1.gz
/usr/share/man/ja/man1/chage.1.gz
/usr/share/man/man1/chage.1.gz
/usr/share/man/man1/lchage.1.gz
/usr/share/man/pl/man1/chage.1.gz
/usr/share/man/ru/man1/chage.1.gz
/usr/share/man/sv/man1/chage.1.gz
/usr/share/man/tr/man1/chage.1.gz
```

Notice that locate not only found the chage command, it also found the lchage command and a variety of man pages associated with chage for different languages. The locate command looks all over your filesystem, not just in directories that contain com-mands. (If locate does not find files recently added to your system, run `updatedb` as root to update the locate database.)

`which -a ls` show that the `ls` command is in the `/bin` directory (show full path)

`whereis aws`

- `which`:
  - f
- `whereis`

## `dd`

Archiving partitions with `dd`

There’s all kinds of stuff you can do with `dd` if you research hard enough, but where it shines is in the ways it lets you play with partitions. Earlier, you used `tar` to replicate entire file systems by copying the files from one computer and then pasted them as is on top of a fresh Linux install of another computer. But because those file system archives weren’t complete images, they required a running host OS to serve as a base.

Using `dd`, on the other hand, can make perfect byte-for-byte images of, well, just about anything digital. But before you start flinging partitions from one end of the earth to the other, I should mention that there’s some truth to that old UNIX admin joke: _dd_ stands for _Disk Destroyer_. If you type even one wrong character in a `dd` command, you can instantly and permanently wipe out an entire drive worth of valuable data. And yes, spelling counts.

As always with `dd`, pause and think very carefully before pressing that Enter key!

---

Now that you’ve been suitably warned, we’ll start with something straightforward. Suppose you want to create an exact image of an entire disk of data that’s been designated as /dev/sda. You’ve plugged in an empty drive (ideally having the same capacity as your `/dev/sdb` system). The syntax is simple: `if=` defines the source drive, and `of=` defines the file or location where you want your data saved:

`sudo dd if=/dev/sda of=/dev/sdb`

---

This command will spend some time writing millions and millions of zeros over every nook and cranny of the /dev/sda1 partition:

`sudo dd if=/dev/zero of=/dev/sda1`

But it gets better. Using the /dev/urandom file as your source, you can write over a disk with random characters:

`sudo dd if=/dev/urandom of=/dev/sda1`

–-

`dd` can also be used to write linux `ISO` image to USB drive.

## Synchronizing archives with `rsync`

One thing you already know about proper backups is that, to be effective, they absolutely have to happen regularly. One problem with that is that daily transfers of huge archives can place a lot of strain on your network resources. Wouldn’t it be nice if you only had to transfer the small handful of files that had been created or updated since the last time, rather than the whole file system? Done. Say hello to `rsync`.

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

nvim, yazi cài trong `C:\Users\anhao\.local\bin\`

## MacOS File system

[macOS Standard Directories: Where Files Reside](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW6)

In the macOS environment you will find the HFS+ file system. **HFS Plus** or **HFS +** uses something known as a B-Tree to index the files on the hard discs. B-Trees (which are different than a binary tree) allow for fast access time, much like the binary tree.  As of June 2016, Apple has implemented their new file system “APFS,” which also uses the B-Tree to index files.  This has become the replacement for HFS+ on macOS High Sierra and onward, as well as iOS 10.3+.

The `local domain` contains resources such as apps that are local to the current computer and shared among all users of that computer.

- `/Applications` chứa app tải về trên mạng

The `system domain` contains the system software installed by Apple. The resources in the system domain are required by the system to run. Users cannot add, remove, or alter items in this domain.
