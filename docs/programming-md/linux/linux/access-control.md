# Access Control

## Filesystem Access control

Although the owner of a file is always a single person, many people can be group owners of the file, as long as they are all part of a single group. Groups are traditionally defined in the `/etc/group` file.

The ownerships of a file can be determined with `ls -l filename`.

```bash
$ ls -l /home/garth/todo 
-rw------- 1 garth staff 1258 Jun 4 18:15 /home/garth/todo
```

This file is owned by the user `garth` and the group `staff`.

Both the kernel and the filesystem track owners and groups as numbers rather than as text names.

In the most basic case, user identification numbers (`UIDs` for short) are mapped to usernames in the `/etc/passwd` file, and group identification numbers (`GIDs`) are mapped to group names in `/etc/group`.  
The text names that correspond to UIDs and GIDs are defined only for the convenience of the system’s human users. When commands such as `ls` want to display ownership information in a human-readable format, they must look up each name in the appropriate file or database.

## The `root` account

The defining characteristic of the root account is its UID of `0`.

- Examples of restricted operations are:
  * Changing the root directory of a process with `chroot`
  * Creating device files
  * Setting the system clock
  * Raising resource usage limits and process priorities
  * Setting the system’s hostname
  * Configuring network interfaces
  * Opening privileged network ports (those numbered below `1,024`)
  * Shutting down the system

## `su` substitute user identity

If invoked without arguments, `su` prompts for the root password and then starts up a **root shell**. Root privileges remain in effect until you terminate the shell by typing `<Control-D>` or the `exit` command.

`su` doesn’t record the commands executed as root, but it does create a log entry that states **who** became root and when.

---

The `su` command can also substitute **identities other than root**. Sometimes, the only way to reproduce or debug a user’s problem is to `su` to their account so that you reproduce the environment in which the problem occurs.

If you know someone’s password, you can access that person’s account directly by executing `su - username`.  
As with an su to root, you will be prompted for the password for username. The `-` (dash) option makes su spawn the shell in login mode. The exact implications of login mode vary by shell, but it normally changes the number or identity of the startup files that the shell reads. For example, bash reads ~/.bash_profile in login mode and ~/.bashrc in nonlogin mode. When di-agnosing other users’ problems, it helps to reproduce their login environments as closely as possible.

## `sudo`: limited `su`

`sudo` is a program (superuser do).

`sudo` takes as its argument a command line to be executed as root (or as another restricted user). 

`sudo` consults the file `/etc/sudoers`, which lists the people who are authorized to use sudo and the commands they are allowed to run on each host.  
If the proposed command is permitted, sudo prompts for the **user’s own password** and executes the command.

`sudo` keeps a log of the command lines that were executed, the hosts on which they were run, the people who requested them, the directory from which they were run, and the times at which they were invoked.

You can edit the `sudoer` file with the `visudo` command.

By default, the user created during the initial Linux installation will have sudo powers.

When illustrating command-line examples throughout this book, I use a command prompt of $ for commands that don’t require administrator privileges and, instead of `$ sudo`, I use `#` for those commands that do. Thus a sudo command will look like this: `# nano /etc/group`

## Managing users

The `cat /etc/passwd` file stores all the users information including system users. The actual passwords are stored in `sudo cat etc/shadow`
`cat etc/group` list of groups

`groups [USERNAME]` print groups in which user belong to

`sudo adduser USERNAME` add new user

`su - USERNAME` switch to another user (require password). `su -`switch to the `root` user (require password). Type `logout|exit` or `Ctrl d` to log out. You only use this method if the `sudo` package is not installed on your machine.

`sudo su - [USERNAME]` switch to another user (without password). `sudo su -` switch to root user

`passwd` change current user's password
`sudo passwd USERNAME` change user password as root

`sudo userdel -r USERNAME` remove user. The `-r` option also remove this user's home directory.

`sudo groupadd GROUPNAME` create new group

`sudo usermod -aG GROUPNAME USERNAME` add user to group. You have to log-out and log-in for it to take effect

`sudo gpasswd -d USERNAME GROUPNAME` remove user from group

`sudo groupdel GROUPNAME` remove group

## The sudo command

[su command in linux](https://linuxize.com/post/su-command-in-linux/)

The `su` (short for substitute or switch user) utility allows you to run commands with another user’s privileges, by default the root user. `sudo` should be read as "su do", that is, "switch user and do this command"

`which sudo` check if the sudo package is installed

`cat /etc/sudoers` member of sudo group can execute any command
`usermod -aG [wheel|sudo] USERNAME` add user to sudo group. After adding user to group, you log in again for it to take effect

`sudo -l` list the privileges for the invoking user

`!!` repeat the last command you ran. For example: `sudo !!`

`sudo visudo` dedicated command to edit the `/etc/sudoers` file

## User Management

Usernames aren't really identifications for users. The system uses a user ID (`UID`) to identify a user.

To find out what users are mapped to what ID, look at the `/etc/passwd` file (`cat /etc/passwd`). Other information in this file includes: user's shell, home directory  
The password is not really stored in this file, it's usually stored in the `/etc/shadow` file. If you see an `x` that means the password is stored in the `/etc/shadow` file, a `*` means the user doesn't have login access and if there is a blank field that means the user doesn't have a password.

`/etc/passwd` also contains other users which are not human (called `system users`).  
Remember that users are really only on the system to run processes with different permissions. Sometimes we want to run processes with pre-determined permissions. For example, the daemon user is used for daemon processes.

The `/etc/shadow` file is used to store information about user authentication. It requires superuser read permissions.\
In most distributions today, user authentication doesn't rely on just the `/etc/shadow` file, there are other mechanisms in place such as `PAM (Pluggable Authentication Modules)` that replace authentication.

`/etc/group` store group names and users inside that group. You can also use the command `groups`

You can use the `adduser` or the `useradd` command. The adduser command contains more helpful features such as making a home directory and more. There are configuration files for adding new users that can be customized depending on what you want to allocate to a default user. Example: `$ sudo useradd bob`

To remove a user, you can use the `userdel` command.

Change the password of yourself or another user (if you are root): `$ passwd bob`

## ACL

ACL (Access Control List) is used in both Linux and networking because it is a universal computer security concept—the method used to implement granular authorization.

The reason the name is the same is that the goal is identical: to have a list of rules that the system checks in order to decide whether to allow or deny access to a resource.

- Linux (Filesystem) ACL
  * Resource: A specific file or folder.
  * Purpose: Granular Permissions (Who can touch this file?).

- Networking (Firewall/Router) ACL
  * Resource: A network interface or an entire subnet.
  * Purpose: Traffic Filtering (What data is allowed on this wire?).