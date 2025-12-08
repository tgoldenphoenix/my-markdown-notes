# Process Management

A **process** is the abstraction used by UNIX and Linux to represent a running pro-gram. It’s the object through which a program’s use of memory, processor time, and I/O resources can be managed and monitored.

System and user processes all follow the same rules, so you can use a single set of tools to control them both.

## Terminologies

A **core dump** (or more formally, a memory dump) in Linux is a file containing a snapshot of the memory (RAM) used by a process at the moment it crashed. It also includes other information like the processor register values and process status.

Software, as I’m sure you already know, is programming code containing instructions to control computer hardware on behalf of human users. A **process** is an instance of a running software program. An **operating system** is a tool for organizing and managing those instances/processes to effectively use a computer’s hardware resources. 

## Component of a Process

A process consists of an **address space** and a set of data structures within the ker-nel.

- PID: process ID number
- The kernel assigns a unique ID number to every process. Most commands and system calls that manipulate processes require you to specify a PID to identify the target of the operation. PIDs are assigned in order as processes are created.

---

`PPID`: parent PID

Neither UNIX nor Linux has a system call that initiates a new process running a particular program. Instead, an existing process must clone itself to create a new process. The clone can then exchange the program it’s running for a different one.

---

UID and EUID: real and effective user ID

A process’s UID is the user identification number of the person who created it, or more accurately, it is a copy of the UID value of the parent process.

The EUID is the “effective” user ID, an extra UID used to determine what re-sources and files a process has permission to access at any given moment. For most processes, the UID and EUID are the same, the usual exception being pro-grams that are setuid.

Why have both a UID and an EUID? Simply because it’s useful to maintain a distinction between identity and permission.

---

GID and EGID: real and effective group ID

The GID is the group identification number of a process. The EGID is related to the GID in the same way that the EUID is related to the UID in that it can be “upgraded” by the execution of a setgid program.

## Life Cycle of a Process

To create a new process, a process copies itself with the `fork` system call. `fork` creates a copy of the original process; that copy is largely identical to the parent. The new process has a distinct PID and has its own accounting information

After a fork, the child process will often use one of the `exec` family of system calls to begin the execution of a new program.4 These calls change the program that the process is executing and reset the memory segments to a predefined initial state

All processes other than the ones the kernel creates are descendants of `init`.

init also plays another important role in process management. When a process completes, it calls a routine named `_exit` to notify the kernel that it is ready to die. It supplies an exit code (an integer) that tells why it’s exiting. By convention, `0` is used to indicate a normal or “successful” termination.

## Signals

- Signals are process-level interrupt requests. About thirty different kinds are de-fined, and they’re used in a variety of ways:
  * They can be sent among processes as a means of communication.
  * They can be sent by the terminal driver to kill, interrupt, or suspend 
processes when keys such as `<Control-C>` and `<Control-Z>` are typed.
  * They can be sent by an administrator (with `kill`) to achieve various ends.
  * They can be sent by the kernel when a process commits an infraction 
such as division by zero.
  * They can be sent by the kernel to notify a process of an “interesting” 
condition such as the death of a child process or the availability of data 
on an I/O channel.

## `systemctl`

- systemd is the Service Manager (the core program or daemon).
- systemctl is the Command-Line Interface (CLI) used to interact with the running systemd daemon.

`systemctl` is the primary command-line tool for managing the `systemd` system and service manager in modern Linux, used to control system services (start, stop, restart, enable, disable), check their status, manage system targets, and monitor overall system state, replacing older init systems like SysV. It offers comprehensive control over daemons and system behavior, making it essential for Linux administration

`systemd` is the very first user-space process launched by the Linux kernel, receiving the special Process ID of 1 (PID 1).

You never interact directly with the systemd PID 1 process; you always send commands to it using the `systemctl` tool.

`start`, `stop`, `restart`

`enable`, `disable`

Because you’ve edited the configuration files, you’ll need to restart SSH on both machines to make sure that your changes are live: 

`sudo systemctl restart ssh`

The availability and responsiveness of many system services are managed by systemd’s systemctl process manager. 

## `systemd` and `init`

Ngày xưa `systemd` có tên là `init` và `init` có PID là 1. On many systemd-based systems, if you look at the file path for init (`/sbin/init`), you will find it is actually a symbolic link that points directly to the systemd executable.

There’s something interesting about that /sbin/init file you just saw: `file` is a venerable UNIX program that gives you insider information about a file. If you run `file` with `/sbin/init` as its argument, you’ll see that the init file is not actually a program, but a **symbolic link** to a program called `systemd`.

```bash
$ file /sbin/init
/sbin/init: symbolic link to /lib/systemd/systemd
```

It took years of fragmentation and some vigorous political infighting, but nearly all Linux distributions now use the same **process manager**: `systemd`. It’s a drop-in replacement for a process called init, which has long been the very first process started during the boot process of all UNIX-based operating systems. By drop-in replacement, I mean that, even if the way it gets things done can be quite different, to the casual observer, systemd functions like init always did. That’s why the /sbin/init file is now nothing more than a link to the systemd program. 

Technically, systemd’s primary job is to control the ways individual processes are born, live their lives, and then die. The `systemctl` command you used previously is the tool of choice for those tasks. But, somewhat controversially, the systemd developers expanded the functionality far beyond the traditional role of process management to take control over various system services. Included under the new systemd umbrella are tools like a logging manager (`journald`), network manager (`networkd`), and device manager (you guessed it: udevd). Curious? The _d_ stands for _daemon_, a background system process.

## `kill`: Send Signals

As its name implies, the `kill` command is most often used to terminate a process. kill can send any signal, but by default it sends a `TERM`. kill can be used by nor-mal users on their own processes or by root on any process. The syntax is:

`kill [-signal] pid`

where signal is the number or symbolic name of the signal to be sent and pid is the process identification number of the target process.

Under Linux, `killall` kills processes by name. For example, the following com-mand kills all Apache web server processes: `ubuntu$ sudo killall httpd`

## Process State

A process is not automatically eligible to receive CPU time just because it exists. You need to be  aware of the four execution states listed in Table:

| State    | Meaning                                            |
|----------|----------------------------------------------------|
| Runnable | The process can be executed.                       |
| Sleeping | The process is waiting for some resource.          |
| Zombie   | The process is trying to die.                      |
| Stopped  | The process is suspended (not allowed to execute). |

A runnable process is ready to execute whenever CPU time is available. It has acquired all the resources it needs and is just waiting for CPU time to process its data. As soon as the process makes a system call that cannot be immediately com-pleted (such as a request to read part of a file), the kernel puts it to sleep.

Zombies are processes that have finished execution but have not yet had their status collected. If you see zombies hanging around, check their PPIDs with ps to find out where they’re coming from.

## `ps`: Monitor Processes

`ps` is the system administrator’s main tool for monitoring processes.

`ps` can show the PID, UID, priority, and control terminal of processes. It also gives information about how much memory a process is using, how much CPU time it has consumed, and its current status (running, stopped, sleeping, etc.). Zombies show up in a ps listing as `<exiting>` or `<defunct>`.

Adding the `-e` argument to ps as you did previously returns not only the processes running in your current child shell, but all the processes from all parent shells right back up to init. 

A **parent shell** is a shell environment from within which new (child) shells can subsequently be launched and through which programs run. You can think of your GUI desktop session as a shell, and the terminal you open to get a command line as its child. The top-level shell (the grandparent?) is the one that’s run first when Linux boots.

---

`-f` is the option for "full", hiện đầy đủ thông tin về process.

If you want to visualize parent and child shells/processes, you can use the `pstree` command (adding the `-p` argument to display the PIDs for each process). Note how the first process (assigned PID 1) is `systemd`. On older versions of Linux (Ubuntu 14.04 and earlier, for instance), this would have been called `init` instead

```bash
$ pstree -p
systemd(1)agetty(264)
            agetty(266)
            agetty(267)
            agetty(268)
            agetty(269)
            apache2(320)apache2(351)
                            apache2(352)
                            apache2(353)
                            apache2(354)
                            apache2(355)
            cron(118)
            dbus-daemon(109)
            dhclient(204)
            dockerd(236)docker-containe(390){docker-containe}(392)
                                                    {docker-containe}(404)
                            {dockerd}(306)
                            {dockerd}(409)
            mysqld(280){mysqld}(325)
                           {mysqld}(326)
                           {mysqld}(399)
            nmbd(294)
            rsyslogd(116){in:imklog}(166)
                             {in:imuxsock}(165)
                             {rs:main Q:Reg}(167)
            smbd(174)smbd(203)
                         smbd(313)
            sshd(239)sshd(840)sshd(849)bash(850)pstree(15328)
            systemd-journal(42)
            systemd-logind(108)
```

## Running in the background

The ampersand symbol (`&`) at the end of a shell command is a control operator that instructs the shell to run the command in the background.

This means the shell executes the command asynchronously and immediately returns control to the command prompt, allowing you to run other commands simultaneously.

Normally, when you execute a command (e.g., `sleep 100`), the shell process waits for that command to finish before accepting new input. This is called running in the **foreground**.

```
sleep 60    #The terminal freezes for 60 seconds.
sleep 60 &  #The shell prints a Job ID and PID, and the prompt returns immediately. The sleep command runs invisibly for 60 seconds.
```

A process started with just `&` will typically be terminated when you close the terminal session. If you need a command to keep running even after you log out (especially over SSH), you should use the nohup command or a terminal multiplexer like `tmux` or screen.

The column `TTY` ?

## job vs process

- A Process is the low-level, technical unit of execution.
- A Job is a high-level unit used for user organization and shell control.

- Process:
  * An instance of a program being executed. The basic unit of work for the kernel.
  * System-wide. Managed by the operating system kernel.
  * Identified by a unique PID (Process ID).
  * A process belongs to at least one job (or is an independent daemon).
  * State Control: `kill`, `nice`, `ps` (kernel tools)
- Job:
  * A set of processes grouped by a shell or batch system, managed as a single entity by the user.
  * Shell-specific. Managed by the command-line shell (like Bash).
  * Identified by a Job ID (e.g.,`[1]`, `[2]`), unique within a single terminal session.
  * A job can contain one or more processes.
  * State Control: `jobs`, `fg` (foreground), `bg` (background) (shell tools)

- When you execute any program (e.g., Firefox, a Python script, or a simple ls command), the kernel creates a process.
- The kernel assigns each process a unique PID (Process ID) and allocates system resources (CPU time, memory, file descriptors).
- A single application can involve multiple cooperating processes (parent processes creating child processes).

A job is a concept specific to the shell environment (like Bash or Zsh). It is a collection of one or more related processes that the shell treats as a single unit, primarily for job control.

- A simple command like `sleep 60` is one job containing one process.
- A piped command like `ls | grep .txt` is one job containing two processes (one for ls and one for grep).
- The key feature is foreground/background control. By adding & to the end of a command (sleep 60 &), you tell the shell to put that entire job into the background, returning control of the terminal to you.

You then use shell commands like jobs, fg (foreground), and bg (background) to manage the execution state of that job.

## Dynamic Monitoring with `top`

Since commands like ps offer only a one-time snapshot of your system, it is often difficult to grasp the big picture of what’s really happening. `top` is a free utility that runs on many systems and provides a regularly updated summary of active pro-cesses and their use of resources.

By default, the display updates every 10 seconds. The most CPU-consumptive processes appear at the top. `top` also accepts input from the keyboard and allows you to send signals and to `renice` processes, so you can observe how your actions affect the overall condition of the machine.

## The `/proc` filesystem

The Linux versions of ps and top read their process status information from the `/proc` directory, a pseudo-filesystem in which the kernel exposes a variety of in-teresting information about the system’s state.

Despite the name `/proc`, the information is not limited to process information—a variety of status information and statistics generated by the kernel are represented here.