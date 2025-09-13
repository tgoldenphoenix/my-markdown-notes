# Storage Devices

Computers put fast but expensive and small storage options close to the CPU and slower but less expensive and larger options further away. Generally, the fast technologies are referred to as "**memory**" (used for processing), while slower persistent technologies are referred to as "**storage**".

The disk (secondary storage) stores the information about the partitions' locations and sizes in an area known as the **partition table** that the operating system reads before any other part of the disk.

Swap space là 1 vùng trên hard drive được dùng như RAM để hỗ trợ memory for the computer (it is slower than the actual RAM)

`/media/

## Installing linux

What is boot from ram???

Khi dùng bootable USB flash drive thì phải disable secure boot.

Disable fast boot if there is no boot priority option found.

While CSM provides compatibility, it also has some drawbacks. Enabling CSM may impact system performance, as it adds an additional layer of emulation. Additionally, it can limit the benefits and features provided by UEFI, such as secure boot and faster startup times. Nên muốn disable secure boot thì trước đó phải disable CSM (may need to reboot for the USB to be found).

### Partitioning

1 GB = 1.000 MB
1 MB = 1,000,000 bytes or 8,000,000 bits
1 MiB (Mebibyte) = 1,048,576 bytes or 8,388,608 bits. Cũng không khác MB là mấy nhưng chính xác hơn chút.

The **EFI** (Extensible Firmware Interface) system partition or ESP is a partition on a data storage device (usually a hard disk drive or solid-state drive) that is used by computers that have the Unified Extensible Firmware Interface (UEFI). When a computer is booted, UEFI firmware loads files stored on the ESP to start operating systems and various utilities. 

A partition table is required before partitions can be added.

kingston NVMe
usb 57 GB ??? sandisk
ATA ST1000DM010-2EP1 /dev/sda

Nếu có UEFI supported motherboard thì chọn GPT partition table (GUID Partition Table). Không phải "GPT" as in ChatGPT (Chat Generative Pre-trained Transformer.

On Window: Disk management (GUI), diskpart (terminal)
On Linux: Gparted (GUI) or cfdisk(terminal)

## MBR vs GPT

[MBR vs GPT](https://www.freecodecamp.org/news/mbr-vs-gpt-whats-the-difference-between-an-mbr-partition-and-a-gpt-partition-solved/)

Before a drive can be divided into individual partitions, it needs to be configured to use a specific partition scheme or table.

A partition table tells the operating system how the partitions and data on the drive are organized. For example, the screenshots above show the partition tables on the drive, and each individual partition is shown as a rectangular block.

There are two main types of partition tables: MBR and GPT

### MBR (Master Boot Record)

## Mounting

[explain](https://unix.stackexchange.com/questions/3192/what-is-meant-by-mounting-a-device-in-linux)
[learn Linux TV](https://www.youtube.com/watch?v=2Z6ouBYfZr8&t=514s)

`lsblk` list block devices

## Termilogy

**sda, sdb, sdc** storage device a, b
**NVMe** (Non-Volatile Memory Express)

LBA: logical block addressing

[block device vs character device](http://haifux.org/lectures/86-sil/kernel-modules-drivers/node10.html)