# Networking & System Administration commands

## The OSI Model

All the different types of communication protocols are classified in 7 layers, which are known as the Open Systems Interconnection Reference Model, the OSI Model for short. For easy understanding, this model is reduced to a 4-layer protocol description, as described in the table below

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

## Remote Management with OpenSSH

