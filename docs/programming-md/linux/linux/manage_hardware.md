# Hardware Management

```
$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 63
model name      : Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz
stepping        : 2
microcode       : 0x46
cpu MHz         : 2399.915
cache size      : 30720 KB
[...]
$ free -m
         total    used    free    shared    buff/cache    available
Mem:       965      93     379         0           492          739
Swap:        0       0       0
```

Your virtual machine provides a single CPU core and 965 MB of memory.

```
$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 85
model name      : Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
stepping        : 7
microcode       : 0x500320a
cpu MHz         : 3117.531
cache size      : 36608 KB
[...]
  
processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 85
model name      : Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
stepping        : 7
microcode       : 0x500320a
cpu MHz         : 3100.884
cache size      : 36608 KB
[...]
 
$ free -m
         total    used    free    shared    buff/cache    available
Mem:      7737     108    7427         0           202         7406
Swap:        0       0       0
```

Your virtual machine can use two CPU cores and offers 7,737 MB of memory, compared to a single CPU core and 965 MB of memory before you increased the VM’s size.

## Check Hardware

Use `dmesg` to see what hardware was detected and which drivers were loaded by the kernel at boot time. 

`dmesg [options]`

Examples:

`sudo dmesg | less`

`sudo dmesg --follow` enable real-time kernel ring buffer monitoring.

`sudo dmesg | grep -i memory`

Search for multiple terms at once by appending the `-E` option to grep and providing the search terms encased in quotations, separated by pipe delimiters.

`sudo dmesg | grep -E "memory|tty"`

---

The `lspci` command lists PCI buses on your computer and devices connected to them.

If you are specifically interested in USB devices, try the `lsusb` command.

To see details about your processor, run the `lscpu` command.

## Storage Devices

IDE (Integrated Drive Electronics) was the standard from the late 1980s until the mid-2000s.

SATA (Serial Advanced Technology Attachment) was introduced around 2003 to solve the physical and speed limitations of IDE.

IDE is an older, parallel technology, while SATA is the modern, serial standard that replaced it.

when you create a new Virtual Machine, the virtual hard disk is usually attached to a SATA controller, but the virtual optical drive (CD/DVD) is often attached to an IDE controller by default.
