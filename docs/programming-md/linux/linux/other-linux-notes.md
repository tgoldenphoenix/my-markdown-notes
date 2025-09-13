# Other linux notes

## History

1969, Bell Labs laboratories, crete UNIX. The C programming language was especially developed for creating the UNIX system.

By the end of the 80's, many people had home computers. By that time, there were several versions of UNIX available for the PC architecture, but none of them were truly free and more important: they were all terribly slow, so most people ran MS DOS or Windows 3.1 on their home PCs. UNIX at the time can only be found on very large environments with mainframes. And it is very hard to use for a normal person.

By the beginning of the 90s home PCs were finally powerful enough to run a full blown UNIX. Linus Torvalds, a young man studying computer science at the university of Helsinki, thought it would be a good idea to have some sort of freely available academic version of UNIX, and promptly started to code.

Maybe even more successful than the SAMBA project is the **Apache HTTP server project**. The server runs on UNIX, Windows NT and many other operating systems. Originally known as "A PAtCHy server", based on existing code and a series of "patch files", the name for the matured code deserves to be connoted with the native American tribe of the Apache, well-known for their superior skills in warfare strategy and inexhaustible endurance.

Linux is an implementation of UNIX.

[Unix time](https://en.wikipedia.org/wiki/Unix_time) is a date and time representation widely used in computing. It measures time by the number of non-leap seconds that have elapsed since **00:00:00 UTC on 1 January 1970**, the Unix epoch. For example, at midnight on 1 January 2010, Unix time was 1262304000.

## Why use Linux?

**Linux is portable to any hardware platform:**
A vendor who wants to sell a new type of computer and who doesn't know what kind of OS his new machine will run (say the CPU in your car or washing machine), can take a Linux kernel and make it work on his hardware, because documentation related to this activity is freely available.

## Symbolic links & hard links

Symbolic links use a path to point to the file. Hard links use the inode number to point to the file.

Symbolic links can link to file on different file system (different storage device that you are mounting) which hard link cannot. Becaues inodes are specific to file system.

It is very hard to tell if a hard link is actually a hard link using `ls -l` other than when you are the person who created that link.

## The OS Kernell

The kernel is also the defining piece of the OS: if two OS’s have the same kernel, they can run the same programs and use the same drivers.

## Process management

A process (sometimes called a task) is an instance of a running program.

## windowing system

In computing, a [windowing system](https://en.wikipedia.org/wiki/Windowing_system) (or window system) is a software suite that manages separately different parts of display screens.[1] It is a type of graphical user interface (GUI) which implements the WIMP (windows, icons, menus, pointer) paradigm for a user interface.

The [X Window System](https://en.wikipedia.org/wiki/X_Window_System) (X11, or simply X; stylized 𝕏) is a windowing system for bitmap displays, common on Unix-like operating systems.

Some people have attempted writing alternatives to and replacements for X. macOS (and its mobile counterpart, iOS) implements its windowing system, which is known as Quartz.

[Wayland](https://en.wikipedia.org/wiki/Wayland_(protocol)) is being developed by several X.Org developers as a prospective replacement for X. It works directly with the GPU hardware, via DRI. Wayland can run an X server as a Wayland compositor, which can be rootless.[16] The project reached version 1.0 in 2012. Like Android, Wayland is EGL-based.

## Systemd notes

`systemd` trên MacOS là `launchd`

[Processes, Daemons, and Services](https://www.baeldung.com/linux/process-daemon-service-differences)

[unit vs service](https://unix.stackexchange.com/questions/714674/what-is-the-difference-between-systemds-service-and-service-and-daemon-and-syst)

## Driver

The kernel depends on individual pieces of software to control each individual piece of hardware, called device drivers. Device drivers contain instructions, like a manual for the kernel, on how to make the hardware perform a requested function. The OS calls the driver, and the driver “drives” the device. These software pieces exist for all hardware, and are often specialized for things like video cards, network adapters, input devices and sound cards. OSs typically use basic drivers that will simply make devices work, but not operate at their full potential. To fully use a device, the user should locate the latest available device driver (either from an included disc, or from the vendor’s website).

<!-- ![driver-layers.png](./assets/driver-layers.png) -->

Linux comes with built-in Drivers for many types of Hardware. Yet, with newer Hardware, the installation procedure does not always manage to configure a proper Driver...
...Or the Driver is not found in our distribution. In these cases, we want to be able to find the right Driver, install it and configure it to work properly.

## Linux in Embedded systems

[What is the difference between embedded system and robots?](https://www.quora.com/What-is-the-difference-between-embedded-system-and-robots) It also compare embedded system vs computer pretty well.

## Terminologies

Text-based User Interface/Text Line Interface (TUI/TLI) displays using text only (usually with different colors) but allows for _mouse input_ rather than relying on a command language (như CLI chỉ cùng keyboard, no mouse, no graphic whatsoever).

A **GUI** presents the user with a full graphical display (images, buttons, scrollbars, etc.). These systems allow the use of a mouse or some other pointing device to click, select, drag, and manipulate objects.

**xterm** is the standard terminal emulator for the X Window System. It allows users to run programs which require a command-line interface.

## Linux installation

balenaEtcher: cài được trên Mac, flash iso cho linux

rufus: flash iso cho window 11

## References

[phoenixnap blog](https://phoenixnap.com/kb/) cũng có nhiều bài viết hay

