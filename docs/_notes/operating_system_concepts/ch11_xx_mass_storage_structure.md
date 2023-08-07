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

In an example where the initial head location is on track `53`, and the queue is `98, 183, 37, 122, 13, 124, 65, 67`. In a FCFS algorithm, you'd have 
