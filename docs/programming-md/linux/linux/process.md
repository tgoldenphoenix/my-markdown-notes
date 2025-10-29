# Controlling Processes

A process is the abstraction used by UNIX and Linux to represent a running pro-gram. It’s the object through which a program’s use of memory, processor time, and I/O resources can be managed and monitored.

System and user processes all follow the same rules, so you can use a single set of tools to control them both.

## Terminologies

A **core dump** (or more formally, a memory dump) in Linux is a file containing a snapshot of the memory (RAM) used by a process at the moment it crashed. It also includes other information like the processor register values and process status.

## Component of a Process

A process consists of an **address space** and a set of data structures within the ker-nel.

- PID: process ID number
- The kernel assigns a unique ID number to every process. Most commands and system calls that manipulate processes require you to specify a PID to identify the target of the operation. PIDs are assigned in order as processes are created.

---

PPID: parent PID

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

## Dynamic Monitoring with `top`

Since commands like ps offer only a one-time snapshot of your system, it is often difficult to grasp the big picture of what’s really happening. `top` is a free utility that runs on many systems and provides a regularly updated summary of active pro-cesses and their use of resources.

By default, the display updates every 10 seconds. The most CPU-consumptive processes appear at the top. `top` also accepts input from the keyboard and allows you to send signals and to `renice` processes, so you can observe how your actions affect the overall condition of the machine.

## The `/proc` filesystem

The Linux versions of ps and top read their process status information from the `/proc` directory, a pseudo-filesystem in which the kernel exposes a variety of in-teresting information about the system’s state.

Despite the name `/proc`, the information is not limited to process information—a variety of status information and statistics generated by the kernel are represented here.