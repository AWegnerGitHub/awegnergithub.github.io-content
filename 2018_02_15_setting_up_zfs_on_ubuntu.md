Title: Setting up a ZFS pool on Ubuntu 16.04
Date: 2018-02-15 22:30
Tags: technical
Category: Technical Solutions
Slug: zfs-pool-on-ubuntu
Summary: With the backup server assembled, it's time to start configuring it. This post covers setting up the ZFS pool for all the data
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

[Previously][1] in this series, the new NAS was assembled. Ubuntu 16.04 has been installed and updated. It's time to do something with all those hard drives! 

I'll be setting the seven 4TB drives in a single [ZFS][2] pool. I'm using ZFS for protection against data corruption. It offers several other [features][3] too. I'll
be using dual parity, which means I could lose two drives and be able to recover. The goal is never to test this, but I'd rather not go through a [data loss scare][4] again.

Before we begin, it's a good idea to ensure Ubuntu has been updated.

    sudo apt-get update && sudo apt-get upgrade

With the update complete, let's get started. 

## Installing ZFS

Installing the ZFS file system is simple on Ubuntu. 

    sudo apt-get install zfs parted
	
Ta-da! Your system is now capable of setting up ZFS pools. The `parted` package will be used to set up a ZFS pool shortly.

## Setting up our pool

Pools are the basic building block of ZFS. A pool is made up of the underlying devices that will store the data. Setting up our ZFS pool requires a little bit of prep work
for our new drives. First, ensure that the `zfs` package installed correctly by running:

    sudo zpool status
	
At this point in the process, you should get the message `no pools available`. 

### Adding the GPT label

I'm setting up this pool with brand new drives. We need to add a `GPT` label to each disk so that ZFS doesn't complain about disks having an `invalid vdev specification` 
when we create the pool. To do this, we'll find the names of our drives first

    ls -l /dev/sd*
	
On my system, I get a result similar to this:

	brw-rw---- 1 root disk 8,   0 Feb 13 09:23 /dev/sda
	brw-rw---- 1 root disk 8,   1 Feb 13 09:23 /dev/sda1
	brw-rw---- 1 root disk 8,   2 Feb 13 09:23 /dev/sda2
	brw-rw---- 1 root disk 8,   5 Feb 13 09:23 /dev/sda5
	brw-rw---- 1 root disk 8,  16 Feb 13 09:23 /dev/sdb
	brw-rw---- 1 root disk 8,  32 Feb 13 09:23 /dev/sdc
	brw-rw---- 1 root disk 8,  48 Feb 13 09:23 /dev/sdd
	brw-rw---- 1 root disk 8,  64 Feb 13 09:23 /dev/sde
	brw-rw---- 1 root disk 8,  80 Feb 13 09:23 /dev/sdf
	brw-rw---- 1 root disk 8,  96 Feb 13 09:23 /dev/sdg
	brw-rw---- 1 root disk 8, 112 Feb 13 09:23 /dev/sdh
	
We'll be adding the `GPT` labels to each of the unformatted drives. The unformatted ones are the listed drives that don't have a numeral as well. For me, that means we'll
be working with `sdb`, `sdc`, `sdd`, `sde`, `sdf`, `sdg` and `sdh`. The `sda` drive has been formatted and contains partitions already. Those are `sda1`, `sda2` and `sda5`. 

For each drive, except `sda` in my case, we need to run the `parted` command.

    sudo parted /dev/sdb
	
This will give you a short dialog. All you will need to do is issue the `mklabel GPT` command and then quit (using `q`)

    GNU Parted 3.2
    Using /dev/sdb
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) mklabel GPT
    (parted) q
    Information: You may need to update /etc/fstab.

### Getting device IDs

Once the `GPT` labels are added, we can create our pool. However, we're not going to use the device paths returned above. Theoretically, those can change (especially if you 
replace a drive). That would be bad and mess with the entire ZFS pool. Instead we're going to create the pool by using the device id. 

    ls -l /dev/disk/by-id/*
	
This returns output similar to this:

    ...
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee20f1d3114 -> ../../sdc
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee20f3ba2b9 -> ../../sdg
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee2647227b7 -> ../../sdb
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee26490a21e -> ../../sdd
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee2b9c81501 -> ../../sdh
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee2b9e6ab61 -> ../../sdf
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x50014ee2b9e6b857 -> ../../sde
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/wwn-0x5001b444a9525c87 -> ../../sda
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/wwn-0x5001b444a9525c87-part1 -> ../../sda1
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/wwn-0x5001b444a9525c87-part2 -> ../../sda2
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/wwn-0x5001b444a9525c87-part5 -> ../../sda5
    
    ...
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKUXT -> ../../sdb
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKXS0 -> ../../sdc
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K4YJ6T0U -> ../../sdg
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K5LCEYN4 -> ../../sdf
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNN6TA -> ../../sde
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNNTXY -> ../../sdd
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K7TSA4VS -> ../../sdh
    lrwxrwxrwx 1 root root  9 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WDS100T1B0A-00H9H0_174256421671 -> ../../sda
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WDS100T1B0A-00H9H0_174256421671-part1 -> ../../sda1
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WDS100T1B0A-00H9H0_174256421671-part2 -> ../../sda2
    lrwxrwxrwx 1 root root 10 Feb 13 09:23 /dev/disk/by-id/ata-WDC_WDS100T1B0A-00H9H0_174256421671-part5 -> ../../sda5
	
Notice that both formats symlink to the same location. This means you can pick which ever format you like better. However, I recommend the second one that contains the 
device serial number. It'll make it easier to determine problem disks in the future. 

### Create the pool

Now that we've determined the device ides for each of our hard drives, it's time to actually create the pool. As I mentioned above, we'll be creating using dual parity
(`raidz2`). We'll be naming our pool `data`. Once this command is complete, `/data` will be where this pool resides.

    sudo zpool create data raidz2 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKUXT /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKXS0 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNNTXY /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNN6TA /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K5LCEYN4 /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K4YJ6T0U /dev/disk/by-id/ata-WDC_WD40EFRX-68N32N0_WD-WCC7K7TSA4VS
	
This will take a little while, but a surprisingly smaller amount of time than I initially expected. 

Once the creation is complete, take a look at the status of your new pool:

    sudo zpool status
	
      pool: data
     state: ONLINE
      scan: none requested
    config:
    
            NAME                                          STATE     READ WRITE CKSUM
            data                                          ONLINE       0     0     0
              raidz2-0                                    ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKUXT  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K0HLKXS0  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNNTXY  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K6VNN6TA  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K5LCEYN4  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K4YJ6T0U  ONLINE       0     0     0
                ata-WDC_WD40EFRX-68N32N0_WD-WCC7K7TSA4VS  ONLINE       0     0     0
    
    errors: No known data errors
	
That last line is important. No known data errors is good. 

## Create datasets

Where pools are the basic building blocks of ZFS, datasets is a term for a ZFS file system, volume, snapshot or clone. Each dataset can be managed and configured differently.
This means that you can compress one dataset, but leave the others alone. You can put a quota on one, but leave the others without a quota. Creating a dataset is pretty easy:

	cd /
    sudo zfs create data/storage
	
This will create a dataset that exists at `/data` named `storage`. You can have child datasets that inherit attributes from parents (or even grandparents) by doing something
like:

    sudo zfs create data/storage/music
	
This will create the new dataset at `/data/storage` named `music`.

Once you've set up your datasets, you can see they were all created and how much space they have available by issuing `sudo zfs list`.

## Set up complete

With that, we've finished setting up ZFS on Ubuntu 16.04. I set up a few datasets for my purposes. I'm one step closer to getting this running and handling all of the 
digital data in the house. 
	
	
 [1]: {filename}2018_02_12_a_new_server_for_the_house.md
 [2]: https://en.wikipedia.org/wiki/ZFS
 [3]: https://wiki.ubuntu.com/ZFS
 [4]: {filename}2018_01_27_backup_your_data.md