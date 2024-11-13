Title: Format USB drive with EFI partition in Windows
Date: 2024-11-13 13:00
Tags: technical
Category: Technical Solutions
Slug: format-efi-usb-windows
Summary: Windows protects EFI partitions from being deleted. This article is a quick walkthrough on how to format a USB drive with an EFI partition within Windows.

[TOC]

## Introduction

I seem to collect USB drives. They are handed out at conferences. They are used as recovery media for new machines. They just randomly appear on my desk. I use them to build live Linux CDs, transferring media from one device to another, building recovery media, and generally the things you'd expect a USB drive to be used for. Generally, when I use a USB drive the first thing I do is format it. My USB drives are not for long term storage and I consider anything on them used one time then the drive goes back to the bin for reuse another time. 

An annoyance with this, though, is that when I use a drive to build a bootable backup media, often times an EFI partition will be created. As soon as this is created, Windows prevents that partition from being deleted in the future without taking additional steps. I have to go look those up every time because I do it infrequently enough to remember. This is a simple walkthrough of my process to solve this problem

A USB drive with an EFI partition looks like this within the Disk Management tool in Windows

![A USB drive with an EFI partition and an unallocated partition][efipartition]

## Format an EFI Partition

To remove this partition, utilize the built in `diskpart` tool. `Win` + `R` and type in `diskpart` and allow the application to run. 

First you need to find which drive you want to adjust partitions on. 

!!! attention
    Ensure you select the correct disk. Failure to do so could result in an inoperable system

Plug the USB drive into the machine and run `list disk` to see all available disks.

    DISKPART> list disk

    Disk ###  Status         Size     Free     Dyn  Gpt
    --------  -------------  -------  -------  ---  ---
    Disk 0    Online         3726 GB  1024 KB        *
    Disk 1    Online          931 GB  1024 KB        *
    Disk 2    Online           14 GB    14 GB

You can see that I have 3 drives detected. `Disk 2` is my USB drive.

Select the disk.

    DISKPART> sel disk 2

    Disk 2 is now the selected disk.

Then show the partitions on the disk.

    DISKPART> list partition

    Partition ###  Type              Size     Offset
    -------------  ----------------  -------  -------
    Partition 1    System            4000 KB   850 KB

There is a single partition - the EFI partition. The rest of the drive is unallocated, as the screenshot above shows. Select this partition. IF you have multiple partitions listed, you'll need to determine which is the EFI partition. In my case, since I'm going to be formatting the entire drive anyway, if there were multiple listed I'd select and delete each individual partition. 

    DISKPART> sel partition 1

    Partition 1 is now the selected partition.

!!! attention
    These commands will delete a partition. If you haven't selected the right drive or partition things will break.

Finally, delete the EFI partition

    DISKPART> delete partition override
    DiskPart successfully deleted the selected partition.

From the disk management tool, you can verify it's gone and format as normal.

![All EFI partitions removed][noefipartition]


 [efipartition]: {attach}images/efi-partition.png
 [noefipartition]: {attach}images/efi-partition-removed.png