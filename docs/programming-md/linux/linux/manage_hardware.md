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