---
title: "Make a new hard drive using ext4 file system"
excerpt: "Partition/Format external and internal hard disk using ext4 file system"
author_profile: false
categories:
  - Linux
tags:
  - Partition
  - File System
  - Format
  - Disk
Layout: posts
toc: false
toc_sticky: false
---

This article is applicable to external (mainly USB) storage device as well as internal storage device. Please note that all disk operation in Linux requires super user privileges.
{: style="text-align: justify;"}

### Find the new hard drive
The command `lsblk` will list all detected hard drives. 
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 223.6G  0 disk 
├─sda1   8:1    0   100M  0 part /boot/efi
├─sda2   8:2    0  1000M  0 part [SWAP]
├─sda3   8:3    0 178.6G  0 part /home
└─sda4   8:4    0    44G  0 part /
sdb      8:16   0 931.5G  0 disk
```
A device name refers to the entire hard disk. In the above output, `sda` refers to the first hard disk and `sdb` is the second one. The numbers following to device name refers to the partitions on that hard disk. In this example, `sdb` is the new hard disk. We also see `sdb` is not yet partitioned since there is no partition number suffixing to the device name.
{: style="text-align: justify;"}

### Partition the new hard drive
Both of the following two commands perfectly partition hard disk. They have user friendly indications and are very easy to use.
{: style="text-align: justify;"}
```
sudo fdisk /dev/sdb
```
```
sudo cfdisk /dev/sdb
```
If you are not familiar with command line, the famous GUI partition application named "KDE Partition Manager" would be better. In this example, we suppose having created only one partition on the new hard disk - ```sdb1```. We have the output of `lsblk`:
{: style="text-align: justify;"}
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 223.6G  0 disk 
├─sda1   8:1    0   100M  0 part /boot/efi
├─sda2   8:2    0  1000M  0 part [SWAP]
├─sda3   8:3    0 178.6G  0 part /home
└─sda4   8:4    0    44G  0 part /
sdb      8:16   0 931.5G  0 disk 
└─sdb1   8:17   0 931.5G  0 part
```
### Format the partitioned hard drive
The following command formats the new hard drive using ext4 file system:
```
mkfs.ext4 /dev/sdb1
```
**Note:** We format the first partition of the hard disk ```sdb``` which is ```sdb1``` (not ```sdb```, the hard disk) using ext4 file system.
{: .notice--warning}
{: style="text-align: justify;"}

### Mount the new hard disk
For the most GUI Linux distros, if you have partitioned and formatted an USB drive, you could pull it out, then plug it in again. It would automatically mount the USB drive under the directory ```/media/[username]/```. Then, you need to set its directory (mount point) accessible to every user, by the following command:
{: style="text-align: justify;"}
```
chmod 0777 /media/[username]/[mount directory]
```
If not, you would not be able to copy any files on your USB drive. Because that your hard disk mount point only grants the access to `root` user who created it. 
{: style="text-align: justify;"}

If your Linux distro does not automatically mount any drives, you need to do it manually by using the following command:
{: style="text-align: justify;"}
```
sudo mount /dev/sdb1 /[mount directory]
```
**Note:** Normally, the hard disk is mounted under the directory ```/mnt```, you could create your own directory under ```/mnt``` before mounting the hard disk. For example, ```sudo mkdir /mnt/driveb```
{: .notice--warning}

### Update /etc/fstab file
If you install the new hard disk as an internal hard disk (inside computer case), and want it to be mounted every time you reboot Linux, You must add your hard disk in your `fstab` file. Here, we use `vim` to edit `fstab` file.
```
vim /etc/fstab
```
append as follows:
```
UUID=[new disk UUID]   /dev/sdb1   /mnt/driveb   ext4   defaults   1   2
```
Disk's UUID value can be obtained by the command:
```
blkid
```
### Label the partition
We can label the partition using `l2label` command. For example, if you want to label the new partition `/driveb`, enter:
```
e2label /dev/sdb1 /driveb
```
We can use disk label instead of its devie name + partition number in the previous step updating `fstab` file to mount the new partition.
```
UUID=[new disk UUID]   LABEL=/driveb   /mnt/driveb   ext4   defaults   1   2
```
Disk label is very practical for USB external drives, if you give them significant names.