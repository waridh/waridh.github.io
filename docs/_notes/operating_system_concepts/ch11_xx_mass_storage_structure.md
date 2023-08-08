---
title: "OSE: 11.XX Mass-Storage Structure"
topic: ose
categories: notes
tags: osc storage
excerpt: "The fundamental components of storage systems."
---

## Overview

This chapter covers the basics of how HDD and SSDs work, along with how the operating system views and interface on a low level with these devices.

## Definitions

- **HDD**: Hard Disk Drives
- **SSD**: Solid State Drives
- **NVM**: Non-volatile memory

## 11.1 Overview of Mass-Storage Structures

### Anatomy of Hard Disks

To understand why some queuing algorithms exist, you must first understand the anatomy of a hard disk. Here are some definitions:

- **Spindle**: The center axle that the magnetic disks are resting on.
- **Arm**: like a record player arm. It moves the Read-Write Head to the sector that the data it needs to read is stored.
- **Read-Write Head**: the tip of the arm that is used to read the magnetic disks. Think of the head of a record player.
- **Tracks**: Singular circumference on a single magnetic disk. These are what the data are stored on.
- **Cylinder**: Since there are many magnetic disks stacked on a singular spindle, a cylinder that the tracks that are vertically overlapping.
- **Sector**: A subdivision of a track. This is the smallest unit of transferable data on an HDD, and it usually ranges from 512B to 4KB.

### Problems with HDD

HDD are used because they have some of the best data to dollar ratio, while giving relatively good performance. One of the main issues with HDDs is that the head must find the sector that it is doing work on. This is mechanical movement, and it takes up the bulk of the latency of IO to an HDD.

To reduce latency, data are stored sequentially as it will also reduce the amount of times that the head must seek the right sector to do IO on.

The two delays caused by the mechanical nature of HDD are called:

- **Seek time**: The time it takes for the head to get to the right track.
- **Rotational latency**: The time it takes for the right sector to rotate to the head.

Some HDDs will have flash memory that will act as a cache for better performance.

Usually, the head will glide just above the magnetic disk, with a clearance in the microns. By design, if the head makes physical contact with the magnetic disk, it would cause the HDD to fail. This event is called a **head crash**, and it normally cannot be repaired.

### Non-Volatile Memory

Solid state devices. NVM devices do not have mechanical parts and instead rely on NAND flash and a controller to store data. The advantage with these devices is that they do not have mechanical seek time, or rotational latency. There are still drawbacks to using NVM in that they are more expensive per MB, and that a single page on the NVM have a limited read and write cycles before it deteriorates.

#### NAND Flash Characteristics

NAND flash can be read and written on page increments, but data cannot be directly overwritten. Instead, data must be erased before they can be written to again. Due to this extra constraint, the following system has been designed:

- There is now an extra grouping layer that has been implemented called blocks, which is a collection of pages.
- When a page is getting updated, that updated page is written to a new page, and the old page is marked as invalid. This means that it will not be read.
- When a block of invalid page has formed, the NAND controller will erase this block.
- The flash translation layer is used by the controller to keep track of which pages are good, and which are invalid data.

There are some extra cases that are designed for when the NAND flash still has some storage remaining, but there is no invalid block to erase. This case handling is called **Garbage Collection**, and it requires that the manufacturer allocate around 20% extra storage to the NAND flash device, which is called **over-provisioning**. To reiterate the problem, NAND flash devices cannot write new data to a page that already has data, so the pre-existing data must be erased before it can be written to. In this scenario where there isn't a fully invalid block that can be erased, garbage collection will copy the block into the **Over-provisioned** area, so that the controller will know what was in the block prior. The controller then erases the original block, and write the updated version with both the new data and the old valid data into that block.

Another major problem with NAND flash devices is that the pages have a limited lifespan, and can only survive limited erase cycles before they start to fail. The two systems put in place to deal with this interaction is over-provisioning and controller write algorithm.

- What happens is that the controller will try to write to all the pages evenly, and not put too much focus on a single page, or block, as that would cause blocks to fail faster.
- Once blocks do fail, blocks from the over-provisioned space will be used to replace the failed blocks.

Here are some notes about operation speed:

- **Read**: The fastest operation on a NAND flash device.
- **Write**: Slower than read, but still relatively fast.
- **Erase**: The slowest operation.
- **Garbage Collection**: Garbage Collection writes are very slow, as it requires the controller to read the blocks, and write it in the over-provisioned space, then read those blocks again, and write it back down with the updated pages. This might have to be done multiple times for a single write, as the available space might be spread out.

Some space when storing data is used for error-correcting code, which helps the controller detect if there are corrupted data, and be able to attempt to self correct.

### Volatile Memory

Sometimes, volatile memory is used as a temporary file system that is useful for programs to store temporary data, or communicate with another program. These **RAM drives** can be carved out by a device driver from the DRAM, and it will allow the system to use this section exactly like a persistent drive with a file system (standard file operations).

On linux, there is `/dev/ram`, on macOS we have `diskutil` to create this, and on windows, you need to use a third party tool to make them. `/tmp` is a RAM drive on Linux.

RAM drives are excellent high-speed temporary storage, thus they are also the best way to create, read, write and delete files.

### Secondary Storage connection method

There are protocols that allow connection between the host and drives. The following is a list of some common protocols:

- **SATA**: Also known as serial advanced technology attachment. This is the most widely used consumer protocol. It allows for 6G bit transfer rate using 8b10b encoding.
- **SAS**: This protocol is mostly used in enterprise, and it is compatible with SATA drives. The latest products of these types support 22.5G bit transfer rate, and uses 128b150b encoding, which provides better efficiency in the data transfer while providing ECC and CRC.
- **USB**: This protocol allows universal communication with many devices. The new USB-C protocols like thunderbolt are able to transfer data at up to 40Gbps.
- **NVMe**: Non-volatile memory express uses PCIe lanes to transfer data. This method of interfacing provides much better bandwidth, and is the cutting edge solution for consumer level storage communications.

Data transfer is done through a controller, (also known as a host bus adapter) which is a device that is connected to the host, and allows communication with the drives. The drives themselves have a device controller, which manages the data on the drives, and allows the host to read and write to the drives.

The host sends a command to the HBA usually using memory mapped IO ports, and the HBA will send a command to the device controller. The device controller then reads the data from the storage media, and keep it in its cache. The data from the cache is then sent to the HBA, where the HBA cache and host DRAM is interchanged using DMA.

### Address Mapping

The addressing of the storage devices are done as a large one-dimensional array or logical blocks.

- A logical block is the smallest unit of transfer.

This one dimensional array of logical block is translated into pages or sectors on a physical drive. By design, the logical block address will attempt to map sequentially to the physical address such the adjacent logical block address would likely entail adjacent physical address. This layer of translation allows a compatibility layer for storage algorithms which makes them compatible with both HDDs and SSDs, while also providing a little more abstractions.

The translation isn't perfect however, and it is because of the following three reasons:

1. Sectors and pages may fail, but LBA will hide this from the system by remapping the logical address to a working page/sector. This process will create small sections of non-sequential mapping to appear.
2. Some HDDs do not have a constant number of sectors per track. This prevents accurate mapping without knowing the exact amount of sectors per track for every track.
3. The physical manufacturer does the LBA mapping internally, and thus sometimes, this information is not disclosed to other developers.

#### CLV and CAV

The front end of a hard-drive has to remain constant, in that they must be able to transfer data at a constant rate. Since there are differing design decisions when it comes to how many sectors are in a track, the mechanics must be adjusted.

1. **CLV**: In one design decision, the bit density per track remain constant. This causes the outer tracks to hold more data than the inner tracks. The benefit of doing this, is that you would be able to maximize the amount of storage on a given HDD. The design issue that arises here though is that to keep data transfer constant throughout the drive, the rotation speed has to change depending on the track that you are reading. When using this design, outer tracks will hold up to 40% more data than the inner tracks.
2. **CAV**: To eliminate variable rotational speed, some drives will have the same amount of bits on every track. This means that the outer tracks are less dense than the inner track.

As with all technology, the capacity of drives are ever-increasing.

## 11.2 HDD scheduling

The goal of HDD scheduling is to reduce the seek time and rotational delay. This can be accomplished by changing up the data transfer queue. Let's get into some of the algorithms that are used.

When IO is happening to a drive, there are some information that the host system must send to the HBA.

- If the operation is an input or an output.
- The open file handle for the file that is being operated on.
- What the memory address for the transfer is.
- Amount of data to transfer.

Here is a list of goals that are trying to be achived by HDD scheduling:

- **Fairness**
- **Timeliness**
- **Optimization**

### First Come First Serve

This algorithm does not work well on HDDs. The reasoning is that you will have cases where the head will have to jump from the inner zone to the very outer zone, and jump back.

In an example where the initial head location is on track `53`, and the queue is `98, 183, 37, 122, 13, 124, 65, 67`. In a FCFS algorithm, you'd have to traverse a total of 642 cylinders to service all the IO requests.

### SCAN

Also known as the elevator algorithm, this queue scheduling does service to the requests that is in its current path, with the closer one being given a higher priority. This is like how an elevator algorithm works, so when a new request is added to the queue that is right in front of the head path, it will be serviced almost immediately, but if the new request is right behind the head, it will have to wait for the head to get to the end, and reverse. Using the previous example, where the starting point is `53` moving towards `0`, this is how the queue would be reordered:

```
37, 13, 65, 67, 122, 124, 183
```

Using this algorithm, the head only traverses 236 cylinders.

There is an issue that arises when using this algorithm, and that is when there is uniform address density in a set of request, as there will be an increasing pileup at the other end of the cylinder index.

### C-SCAN

This scheduling algorithm is similar to SCAN, but in an abstracted view, it's now more like a scanning circle. After the head makes its way to the end, instead of reversing direction and beginning to continue servicing, the head makes its way back to the beginning first, and then start servicing in the same direction. This algorithm is designed to provide a more uniform wait time.

Using the same example of starting at `53` and heading to `0` on list `98, 183, 37, 122, 13, 124, 65, 67`, we get the following output:

`37, 13, 183, 124, 122, 98, 67, 65`

Using this algorithm, the head would have traversed 386 cylinders, but with the added benefit of uniform wait time.

### Choosing algorithms

Here are the jot notes:

- Sometimes, there are overheads when it comes to fining the most optimal service algorithm, that it would be better to just use SCAN.
- When there is only one item in the queue, it does not matter which algorithm is use, since FCFS would be the fastest algorithm.
- When there is heavy queue load, SCAN and C-SCAN becomes more useful.
- SCAN and C-SCAN tries to reduce starvation problems, but it does not eliminate it.

### Deadline Scheduling

Since SCAN and C-SCAN still does not fully eliminate the starvation problem, the Linux kernel came up with Deadline, which is a combination scheduler that removes starvation.

Deadline separates reads and writes, and then further split those into two queues, with a total of four queues.

- The read queue has priority because read processes are more likely to block.
- The real queue that the reading is based on is the C-SCAN queue. This means that the real queue is sorted by LBA.
- The secondary queue run FCFS. After a batch of LBA is done, the system will check the FCFS queue to see if there is a request that is older than the configured age. If there is, a new LBA batch will be queued up with the aged item.

### Other scheduling algorithms

- NOOP
- Completely Fair Queuing scheduler.

## NVM Scheduling

No seek time, and no rotational latency. Linux uses NOOP, which is a modified FCFS algorithm. This is first come, first served, but adjacent requests are merged.

What was analyzed was that read service time is uniform, but write service times are not, due to cases where a garbage collection must occur.

With this property in mind, some SSDs will only merge adjacent write requests, but keeping read requests in pure FCFS.

Still, NVM generally beat HDD in basically all metrics, with some leads being larger than others (random-access IO). NVM might have degraded performance near the end of life, while HDDs generally stay consistent until a mechanical part breaks.

## Error Detection and Correction

There are many error detection and error correction codes. Error detection lets the system know that the value that is being sent has some sort of issues. Some example includes parity bits in transmission, which lets you know there was a corrupted bit in a packet if there isn't an even number of 1s. There is also CRC (cyclic redundancy check) which uses a hash function to detect multi-bit errors.

Error correcting code will not only detect errors, but also restore the original value as long as there isn't too many errors happening on that single packet. **Soft error** are errors that are able to be corrected, while **hard errors** are uncorrectable.

## 11.5 Storage Device Management

- Drive initialization
- Booting from a drive
- Bad-block recovery

### Drive formatting, partitions and volumes

#### Physical Formatting

New storage devices are blank, so there must be some low level initialization:

- HDDs need sectors to be initialized
- SSDs need the pages to be initialized and the flash translation layer (FTL) to be created.

These processes are called **low-level formatting**, or **physical formatting**. The initialization is done by adding data structure for each storage location (for a sector or a page). It will include the header, and trailer surrounding the data section.

The headers and trailers will include information about the sector/page number, and the error detecting/correcting code.

On a lot of drives, the low-level formatting happens at the factory, but some software allows you to format the drive again. The sector sizes that you can choose are commonly 512B to 4KB, and there are trade offs for different sizes.

- When your sector size is smaller, you are able to more efficiently and quickly access files.
- When your sector size is large, you will have less headers and trailers in your drive, and thus better space utilization.

#### Partitions

Before the drive system could hold a file, the operating system will need to write its own data structure into the drive. This is done in three steps.

1. Partition the device into one or more groups of blocks. Each partition will act like its own device. The partition information is written to a fixed location with a fixed format.
    - In Linux, `fdisk` is used to manage partitions on storage devices.
    - Partitions that are recognized by the operating system will have a device entry made by the operating system (`/dev` in Linux). From here, a configuration file (i.e., `/etc/fstab`) will tell the operating system to mount each partition containing a file system at a specified location, and maybe give a mounting permission (`read-only or write-only, or both`).
    - **Mounting** a system allows the user and programs to access the content of a partition.
2. Volume creation and management is the next step. Sometimes this is done implicitly if the partition and file system being put on there is rather simple. Other instances require explicit declaration (i.e., RAID).
    - Linux `lvm2` provides these features.
    - `ZFS` provides both the volume management tools and file systems in a single program.
    - Note that volume also means any mountable file systems.
3. **Logical formatting** is next, and it is the creation of the file system.
    - The operating system will store the base file-system data structure into the partition, which could contain a mapping of free and allocated space, along with an initial empty directory.

The partition information also indicates if the partition contains a bootable file system (contains the operating system).

- This partition that is labeled for boot will establish the root of the file system.
- After the boot partition is mounted, the rest of the devices will have their partition created, and get mounted as well.
- The operating system file-system is the collection of all these mounted partitions.

File-systems will group blocks together into **clusters**. Device IO may be done through blocks, but file-system IO are done through clusters. This is done so that there are more sequential access, and less random access characteristics.

There are also some operating systems that will allow programs to bypass the file-system on a partition, and use the partition as a large sequential array of logical blocks. This is mostly done with databases that can self optimize. When partitions are accessed like this, it is called a **raw disk**. Linux normally does not support raw IO, but a similar access can be achieved by using the DIRECT flag on the `open()` system call.

#### Boot Block

When a computer is turned on, it needs to have an initial program to run. This program is called the **bootstrap loader** and it is a simple program that is stored on the motherboard flash memory. This program will initialize everything on the system, followed by the **bootstrap program**.

The bootstrap program is the full boot-up program that is stored on the boot block of the boot partition. On execution of the code stored in the boot block, the bootstrap program will load the components that are needed to run the operating system, along with device drivers.

Here is a bootstrap program example on Windows. There will be a partition on Windows called the boot partition, and it contains the OS and device drivers. The boot code is placed in the first logical block address on a device, and this is called the **Master Boot Record** (MBR). The booting process starts with running the bootstrap loader, which directs the system to read the code in the boot block in the MBR.

- The MBR also contains information about the partitions for the drive, and flag the boot partition.

Once the system found the boot partition, it will read the first block from that, which directs it to the OS kernel.
