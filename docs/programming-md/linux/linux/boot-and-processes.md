# Booting

## Terminologies

- `HDD`: hard disk drive, have a spinning disk
- `SSD`: solid state drive, no moving part essentially

`GRUB` is the `GNU GRand Unified Bootloader` and is an attempt to get rid of the many different bootloaders we know today.

A `bootloader` is the code an OS uses to bring itself to life when it’s powered on.

[LILO](https://en.wikipedia.org/wiki/LILO_(bootloader)) (Linux Loader) is a bootloader for Linux and was the default boot loader for most Linux distributions. Unlike `loadlin`, it allowed booting Linux without having DOS on the computer. As of 2009, most distributions have switched to GRUB as the default boot loader. Further development of LILO was discontinued in December 2015 along with a request by Joachim Wiedorn for potential developers.

electromechanical teleprinters/teletypewriters (TeleTYpewriter, **TTY**)

[getty](https://en.wikipedia.org/wiki/Getty_(software)), short for "get tty", is a Unix program running on a host computer that manages physical or virtual terminals (TTYs). When it detects a connection, it prompts for a username and runs the 'login' program to authenticate the user.

`BIOS` (Basic Input/Output System)

## Linux Boot Process & Installation

- The Linux boot sequence:
  1. System power up
  2. BIOS or UEFI identifies hardware environment
  3. Mounts `MBR` master boot record partition
  4. GRUB display menu and executes an image kernel
  5. Kernel mounts root partition
  6. Hand-off to `init` or `systemd` (process id 1).

When a computer powers up, firmware instructions embedded in the basic system hardware identify the network, storage, and memory resources that are available. This was done through the BIOS system on older computers and, more recently, using UEFI.

Once the system finds a hard-drive partition containing a Master Boot Record (MBR), it loads the contents into active memory. On Linux systems, the MBR partition contains a number of files that, when run, present one or more loadable kernel image boot configurations. You can choose to load any of those configurations from the GRUB bootloader menu.

### System startup

During system startup stage, the BIOS firmware is called.

BIOS will respectively perform power-on self test (POST), which is to check the system hardware, then enumerate local device and finally initialize the system.  
For system initialization, BIOS will start by searching for the **bootable device** on the system which stores the OS. A bootable device can be storage devices like floppy disk, CD-ROM, USB flash drive, a partition on a hard disk (where a hard disk stores multiple OS, e.g Windows and Fedora), a storage device on local network, etc. A hard disk to boot Linux stores the **Master Boot Record (MBR)**, which contains the **first-stage/primary bootloader** in order to be loaded into RAM.

The BIOS normally lets you select which devices you want the system to try to boot from. Once the BIOS has figured out what device to boot from, it tries to read the first block of the device. This 512-byte segment is known as the **master boot record or MBR**. The MBR contains a program that tells the computer from which partition to load a secondary boot program, the “boot loader.”

IBM PC compatible replaces BIOS by UEFI. In UEFI systems, the Linux kernel can be executed directly by UEFI firmware via EFISTUB, but usually uses GRUB 2 or systemd-boot as a bootloader.

BIOS and UEFI are **firmware** and **NOT** hardware or software.

**POST (Power-On Self-Test)** is conducted by the system's firmware, whether it be BIOS or UEFI

After the POST is successfully completed, the BIOS will then proceed to search for the **boot device**, using a predetermined boot order that was previously set in the BIOS settings. This boot order typically includes popular devices such as hard drives, solid-state drives, optical drives (CD/DVD), USB drives, and network interfaces.

Once the boot device has been identified, the BIOS proceeds to search for either the **Master Boot Record (MBR)** or the **GUID Partition Table (GPT)** on the storage device. These contain the crucial initial boot loader code. The BIOS then dutifully passes the reins to the designated boot loader, such as GRUB for Linux operating systems.

UEFI includes a boot manager, which is more sophisticated than the boot loaders used in BIOS systems. It understands different file systems, allowing the system to boot from drives formatted with newer file systems like GPT. It uses EFI boot partitions to store bootloaders and related information.

UEFI introduced Secure Boot, a security feature that verifies the digital signatures of boot loaders and operating system kernels during the boot process. This helps prevent the loading of unauthorized or malicious code during boot time.

BIOS uses the Master Boot Record (MBR) method, while UEFI uses the GUID Partition Table (GPT) method.

## Download ISO & Verify Checksum

Large files can sometimes become corrupted during the download process. If even a single byte within your .ISO has been changed, there’s a chance the installation won’t work. 
Because you don’t want to invest time and energy only to discover that there was a problem with the download, it’s always a good idea to immediately calculate the checksum (or hash) for the .ISO you’ve downloaded to confirm that everything is as it was. To do that, you’ll need to get the appropriate SHA or MD5 checksum, which is a long string looking something like this: 

`4375b73e3a1aa305a36320ffd7484682922262b3`

You should compare the appropriate string from that page with the results of a command run from the same directory as your downloaded .ISO, which might look like this: 

```
❯ sha256sum -b linuxmint-22.2-xfce-64bit.iso
dea13e523dca28e3aa48d90167a6368c63e1b3251492115417fdbf648551558f *linuxmint-22.2-xfce-64bit.iso
```

If they match, you’re in business. If they don’t (and you’ve double-checked to make sure you’re looking at the right version), then you might have to download the .ISO a second time. 

Download both `sha256sum.txt` and `sha256sum.txt.gpg`.  
Do not copy their content, use “right-click->Save Link As…” to download the files themselves and do not modify them in any way.

The -b (binary mode) option forces the command to read the input file(s) in binary mode (as opposed to text mode). It is technically redundant on most modern Unix-like systems for the `sha256sum` utility, because they typically operate on input files in binary mode by default.

---

To verify the authenticity of `sha256sum.txt`, check the signature of `sha256sum.txt.gpg` by following the steps below.

Import the Linux Mint signing key:

```
❯ gpg --keyserver hkp://keys.openpgp.org:80 --recv-key 27DEB15644C6B3CF3BD7D291300F846BA25BAE09
gpg: directory '/home/anhao/.gnupg' created
gpg: keybox '/home/anhao/.gnupg/pubring.kbx' created
gpg: /home/anhao/.gnupg/trustdb.gpg: trustdb created
gpg: key 300F846BA25BAE09: public key "Linux Mint ISO Signing Key <root@linuxmint.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

Check that the key was properly imported:

```
❯ gpg --list-key --with-fingerprint A25BAE09
pub   rsa4096 2016-06-07 [SC]
      27DE B156 44C6 B3CF 3BD7  D291 300F 846B A25B AE09
uid           [ unknown] Linux Mint ISO Signing Key <root@linuxmint.com>
```

Verify the authenticity of sha256sum.txt:

```
❯ gpg --verify sha256sum.txt.gpg sha256sum.txt
gpg: Signature made Tue Sep  2 09:40:21 2025 UTC
gpg:                using RSA key 27DEB15644C6B3CF3BD7D291300F846BA25BAE09
gpg: Good signature from "Linux Mint ISO Signing Key <root@linuxmint.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 27DE B156 44C6 B3CF 3BD7  D291 300F 846B A25B AE09
```

You need to download both file (using "save link as").

A file with the extension .txt.gpg is a plain text file (.txt) that has been encrypted using GPG (GNU Privacy Guard), a popular cryptographic software tool.

It is a file in a secure, unreadable format that requires a decryption key or passphrase to be opened and viewed.

### live-boot drive

Those .ISO OS images you employed for your VirtualBox VMs back in chapter 2 can also be written to a CD or USB drive and used to boot a live session of the OS. Such live-boot devices let you load fully functioning Linux sessions without having to install anything to a hard drive. Many people use such drives to confirm that a particular Linux distribution will run happily on their hardware before trying to install it. Others will run live sessions as a secure way to maintain their privacy while engaged in sensitive activities like online banking. 

It turns out that those live boot drives are also a fantastic tool for system rescue and recovery.

---

If you want a CD or DVD-based live bootable, `dd` running on any Linux host is up to the job. But if the image will be written to a USB drive and if you happen to be working on an Ubuntu host, you’ll first need to modify the image by adding an MBR to the .ISO archive so that BIOS and UEFI firmware will know what to do with it. 

1. Lên Internet > download `.iso` image
2. Confirm `sha` hash is correct
3. Add MBR to ISO image (all distro)
4. write image to USB drive using `dd`



## Bootloader stage

After a menu entry is chosen and optional parameters are given, GRUB loads the linux kernel into memory and passes control to it.

GRUB (the GRand Unified Boot loader), developed by the GNU project, is the **default boot loader** for most UNIX and Linux systems with Intel processors. GRUB ships with most Linux distributions.  
GRUB’s job is to choose a kernel from a previously assembled list and to load that kernel with op-tions specified by the administrator.

## `init` & Startup Scripts

`init` executes the system startup scripts. These scripts are really just garden-variety shell scripts that are interpreted by sh or bash. The exact location, content, and organization of the scripts vary enormously among vendors.

- Some tasks that are often performed in the startup scripts are:
  * Setting the name of the computer 
  * Setting the time zone 
  * Checking the disks with fsck
  * Mounting the system’s disks
  * Removing old files from the /tmp directory
  * Configuring network interfaces
  * Starting up daemons and network services

`init` is one of the most important daemon. It always has a PID of 1 and is an ancestor of all user processes and all but a few system processes.

- init defines at least seven run levels, each of which represents a particular comple-ment of services that the system should be running:
  - At level 0, the system is completely shut down.
  - Levels 1 and S represent single-user mode.
  - Levels 2 through 5 include support for networking.
  - Level 6 is a “reboot” level.

Levels 0 and 6 are special in that the system can’t actually remain in them; it shuts down or reboots as a side effect of entering them. On most systems, the general default run level is 2 or 3. Under Linux, run level 5 is often used for X Windows login processes. Run level 4 is rarely used.

The `/etc/inittab` file tells init what to do at each run level. Its format varies from system to system, but the basic idea is that inittab defines commands that are to be run (or kept running) when the system enters each level.

As the machine boots, init ratchets its way up from run level 0 to the default run level, which is also set in `/etc/inittab`. To accomplish the transition between each pair of adjacent run levels, init runs the actions spelled out for that transition in /etc/inittab. The same progression is made in reverse order when the machine is shut down.

Starting with Feisty Fawn in early 2007, Ubuntu replaced the traditional init with `Upstart`, an event-driven **service management system** that is also used by some other Linux distributions. Upstart handles transitions in system state—such as hardware changes—more elegantly than does init. It also significantly reduces boot times

## Kernel stage (Init)

The kernel, once it is loaded, finds **init** in `sbin` and executes it.

When **init** starts, it becomes the parent or grandparent of all of the processes that start up automatically on your Linux system. The first thing **init** does, is reading its initialization file, `/etc/inittab`. This instructs **init** to read an initial configuration script for the environment, which sets the path, starts swapping, checks the file systems, and so on. Basically, this step takes care of everything that your system needs to have done at system initialization: setting the clock, initializing serial ports and so forth.

Then **init** continues to read the `/etc/inittab` file, which describes how the system should be set up in each run level and sets the default **run level**. A run level is a configuration of processes. All UNIX-like systems can be run in different process configurations, such as the single user mode, which is referred to as run level 1 or run level S (or s). In this mode, only the system administrator can connect to the system. It is used to perform maintenance tasks without risks of damaging the system or user data. Naturally, in this configuration we don't need to offer user services, so they will all be disabled. Another run level is the reboot run level, or run level 6, which shuts down all running services according to the appropriate procedures and then restarts the system.

Use the `who` to check what your current run level is

## Shutdown

UNIX was not made to be shut down, but if you really must, use the **shutdown** command. After completing the shutdown procedure, the `-h` option will halt the system, while `-r` will reboot it.

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

