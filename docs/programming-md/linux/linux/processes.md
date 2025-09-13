# Processes

Next to files, processes are the most important things on a UNIX/Linux system.

Not every command starts a single process. Some commands initiate a series of processes, such as **mozilla**; others, like **ls**, are executed as a single command.

## Process types

### Interactive processes

Interactive processes are initialized and controlled through a terminal session. In other words, there has to be someone connected to the system to start these processes; they are not started automatically as part of the system functions.

The shell offers a feature called _job control_ which allows easy handling of multiple processes. This mechanism switches processes between the foreground and the background. Using this system, programs can also be started in the background immediately.

### Automatic processes

Automatic or batch processes are not connected to a terminal. Rather, these are tasks that can be queued into a spooler area, where they wait to be executed on a FIFO (first-in, first-out) basis.

### daemons (server processes)

Daemons are server processes that run continuously. Most of the time, they are initialized at system startup and then wait in the background until their service is required. A typical example is the networking daemon, _xinetd_, which is started in almost every boot procedure. After the system is booted, the network daemon just sits and waits until a client program, such as an FTP client, needs to connect.

## Process attributes

A process has a series of characteristics, which can be viewed with the `ps` command.

Note that **ps** only gives a momentary state of the active processes, it is a one-time recording. The `top` program displays a more precise view by updating the results given by **ps** (with a bunch of options) once every five seconds, generating a new list of the processes causing the heaviest load periodically, meanwhile integrating more information about the swap space in use and the state of the CPU, from the `proc` file system

The first line of **top** contains the same information displayed by the `uptime` command

## Life and death of a process


## Commands

`[COMMAND] &` Run this command in the background (release the terminal). In order to free the issuing terminal after entering the command, a trailing ampersand is added.

`jobs` Show commands running in the background.

`Ctrl Z` Suspend (stop, but not quit) a process running in the foreground (suspend).\
`Ctrl C` Interrupt (terminate and quit) a process running in the foreground.

`bg` Reactivate a suspended program in the background.

`fg` Puts the job back in the foreground. Every process running in the background gets a number assigned to it. By using the `%` expression a job can be referred to using its number, for instance `fg %2`.

`ps` which processes are active and what numbers these processes have

`kill` get rid of processes. Foreground processes can often be killed by typing `Control-C`

# Linux Boot Process & Installation

4 steps: system startup (hardware initialization), bootloader stage, kernel stage, and init process.

A firmware/program like BIOS or UEFI load the bootloader into RAM.

After being loaded into RAM, bootloader (also called first-stage bootloader or primary bootloader) will execute to load the second-stage bootloader (also called secondary bootloader). 

The second-stage bootloader will load the kernel image into memory, decompress and initialize it then pass control to this kernel image. Second-stage bootloader also performs several operation on the system such as system hardware check, mounting the root device, loading the necessary kernel modules, etc. Finally, the very first user-space process (init process) starts, and other high-level system initializations are performed (which involve with startup scripts).

## System startup

During system startup stage, the BIOS firmware is called.

BIOS will respectively perform power-on self test (POST), which is to check the system hardware, then enumerate local device and finally initialize the system. For system initialization, BIOS will start by searching for the **bootable device** on the system which stores the OS. A bootable device can be storage devices like floppy disk, CD-ROM, USB flash drive, a partition on a hard disk (where a hard disk stores multiple OS, e.g Windows and Fedora), a storage device on local network, etc. A hard disk to boot Linux stores the **Master Boot Record (MBR)**, which contains the **first-stage/primary bootloader** in order to be loaded into RAM.

IBM PC compatible replaces BIOS by UEFI. In UEFI systems, the Linux kernel can be executed directly by UEFI firmware via EFISTUB, but usually uses GRUB 2 or systemd-boot as a bootloader.

BIOS and UEFI are **firmware** and **NOT** hardware or software.

**POST (Power-On Self-Test)** is conducted by the system's firmware, whether it be BIOS or UEFI

After the POST is successfully completed, the BIOS will then proceed to search for the **boot device**, using a predetermined boot order that was previously set in the BIOS settings. This boot order typically includes popular devices such as hard drives, solid-state drives, optical drives (CD/DVD), USB drives, and network interfaces.

Once the boot device has been identified, the BIOS proceeds to search for either the **Master Boot Record (MBR)** or the **GUID Partition Table (GPT)** on the storage device. These contain the crucial initial boot loader code. The BIOS then dutifully passes the reins to the designated boot loader, such as GRUB for Linux operating systems.

UEFI includes a boot manager, which is more sophisticated than the boot loaders used in BIOS systems. It understands different file systems, allowing the system to boot from drives formatted with newer file systems like GPT. It uses EFI boot partitions to store bootloaders and related information.

UEFI introduced Secure Boot, a security feature that verifies the digital signatures of boot loaders and operating system kernels during the boot process. This helps prevent the loading of unauthorized or malicious code during boot time.

BIOS uses the Master Boot Record (MBR) method, while UEFI uses the GUID Partition Table (GPT) method.

## Bootloader stage

After a menu entry is chosen and optional parameters are given, GRUB loads the linux kernel into memory and passes control to it.

## Kernel stage (Init)

The kernel, once it is loaded, finds **init** in `sbin` and executes it.

When **init** starts, it becomes the parent or grandparent of all of the processes that start up automatically on your Linux system. The first thing **init** does, is reading its initialization file, `/etc/inittab`. This instructs **init** to read an initial configuration script for the environment, which sets the path, starts swapping, checks the file systems, and so on. Basically, this step takes care of everything that your system needs to have done at system initialization: setting the clock, initializing serial ports and so forth.

Then **init** continues to read the `/etc/inittab` file, which describes how the system should be set up in each run level and sets the default **run level**. A run level is a configuration of processes. All UNIX-like systems can be run in different process configurations, such as the single user mode, which is referred to as run level 1 or run level S (or s). In this mode, only the system administrator can connect to the system. It is used to perform maintenance tasks without risks of damaging the system or user data. Naturally, in this configuration we don't need to offer user services, so they will all be disabled. Another run level is the reboot run level, or run level 6, which shuts down all running services according to the appropriate procedures and then restarts the system.

Use the `who` to check what your current run level is

## Shutdown

UNIX was not made to be shut down, but if you really must, use the **shutdown** command. After completing the shutdown procedure, the `-h` option will halt the system, while `-r` will reboot it.

## Terminologies

HDD: hard disk drive, have a spinning disk

SSD: solid state drive, no moving part essentially

Grub is the GRand Unified Boot loader and is an attempt to get rid of the many different boot-loaders we know today.

[LILO](https://en.wikipedia.org/wiki/LILO_(bootloader)) (Linux Loader) is a bootloader for Linux and was the default boot loader for most Linux distributions. Unlike loadlin, it allowed booting Linux without having DOS on the computer.[3] As of 2009, most distributions have switched to GRUB as the default boot loader.[4] Further development of LILO was discontinued in December 2015 along with a request by Joachim Wiedorn for potential developers.

electromechanical teleprinters/teletypewriters (TeleTYpewriter, **TTY**)

[getty](https://en.wikipedia.org/wiki/Getty_(software)), short for "get tty", is a Unix program running on a host computer that manages physical or virtual terminals (TTYs). When it detects a connection, it prompts for a username and runs the 'login' program to authenticate the user. 

## Windows & Linux: Dual Drive Dual Boot 

You need two hard drives for this method.

Sometimes when Windows shuts down it never shuts down properly, leaving certain devices in a specific state that may not work properly under Linux.  This is why on a dual-boot setup it is recommended to disable Fast Boot in Windows to properly close and reset everything.

[This video](https://www.youtube.com/watch?v=KWVte9WGxGE) demonstrates both single-drive dual boot using GNU GRUB and dual-drive dual boot methods. It also mention some disadvantages of a single-drive dual boot.

If you dual boot on one drive only watch [this video](https://www.youtube.com/watch?v=9gS5SoogltE)

## Install Ubuntu

Nếu có window bản quyền, mình có thể copy window activation key lưu lại. Sau này có thể dùng để revert back to window.

**References:**

[freeCodeCamp](https://www.freecodecamp.org/news/linux-boot-process-in-rhel/)

[Wiki boot process](https://en.wikipedia.org/wiki/Booting_process_of_Linux)