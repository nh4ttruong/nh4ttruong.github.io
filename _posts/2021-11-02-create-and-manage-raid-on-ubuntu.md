---
title: Create And Manage RAID Array On Ubuntu
tags: [Ubuntu, NT132.ANTT]
style: border
color: success
description: How to create or manage a RAID array on Linux system? Let's do it on Ubuntu.
---

{% include elements/video.html id="b0N7Af86m-w" %}

## RAID Array
RAID devices are virtual devices created from two or more real block devices. This allows multiple devices (typically disk drives or partitions thereof) to be combined into a single device to hold (for example) a single filesystem. Some RAID levels include redundancy and so can survive some degree of device failure.. It will support user to improve performance and backup when using disks as virtual storage.

Linux supports LINEAR md devices, RAID0 (striping), RAID1 (mirroring), RAID4, RAID5, RAID6, RAID10, MULTIPATH, FAULTY, and CONTAINER. In this post, I will help you create and manage RAID array with 0, 1 and 5 on Ubuntu 20.04 (others are same)

## Check RAID status and show the device infomation

### Check RAID status 
You can easily check with the following command:

`cat /proc/mdstat`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/check-raid.png)

### Show devices which were used to build or not build the RAID array
Type the following command:

`lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT`

## Install `mdadm`
mdadm - manage MD devices aka Linux Software RAID. *[More info about RAID and mdadm](https://linux.die.net/man/8/mdadm)*

`mdadm [mode] <raiddevice> [options] <component-devices>`

## Get the infomation of RAID on device
To get the infomation of RAID on your device, you can:

`sudo mdadm -E /dev/md0

## Create RAID array
You can create a RAID array and custom some demand such as RAID level, amount of devices,...

I will use `sdb` and `sdc` device to create RAID 0:

`sudo mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/create-raid0.png)

After create the RAID array, you need to create the filesystem and mount it into the folder

### Create file system
Type the command:

`sudo mkfs.ext4 -F /dev/md0`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/make-fs.png)


### Create a mount point and mount into the folder
Use `mkdir` to create a mountpoint:

`sudo mkdir -p /mnt/md0`

And mount `/dev/md0` into `/mnt/md0`:

`sudo mount /dev/md0 /mnt/md0`

## Save and config a automatic reassemble and mount RAID array
I was create a RAID array, and I wanna that the RAID array will be automatic reassembled at boot:

`sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/auto-reassemble.png)

Next, I need that it will be automatic mounted:

`echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/auto-mount.png)

Finally, we need to update init ram file system:

`sudo update-initramfs -u`

If you do right, you will see the output which is used when you type `lsblk` command:

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/result-raid0.png)

If you want to know how I create the RAID 1, 5, you can watch the video which I attached it on the top of page ^^

## Manage RAID array/auto-mount
Now, I will manage RAID array with add, remove and reset a RAID array

### Reset a RAID array
Before you reset a RAID, you had to umount it:

`sudo umount /dev/md0`

Next, let's use `mdadm` to stop and remove it:

```bash
sudo mdadm --stop /dev/md0
sudo mdadm --remove /dev/md0
```

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/reset-raid.png)

Now, the /dev/md0 is stopped and removed from RAID array. We try to check it:

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/check1.png)

As you can see that, it's not yet clear. So we can use `--zero-superblock` to full reset:

```
sudo mdadm --zero-superblock /dev/sdb
sudo mdadm --zero-superblock /dev/sdc
```

Done:

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/check2.png)

Next, you need to edit the mdadm config file and fstable file because you were config it to automatic reassemble and mount before:

`sudo nano /etc/fstab`

The nano will appear and you need to comment or earase the ARRAY info:

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/nano-fstab.png)

`sudo nano /etc/mdadm/mdadm.conf`

Comment or earase the FSTAB info:

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/nano-mdadm.png)

And let's update the init ram fs now to finish ^^

`sudo update-initramfs -u`

### Add a devide into RAID array
I will add a device on RAID 5. Note that the RAID 5 have at least 3 devices. I will add the sde device to the RAID 5:

`sudo mdadm /dev/md0 --add /dev/sde`

After that, we need to `comment` with `#` in *mdadd.conf* file as same as resetting step:

`sudo nano /etc/mdadm/mdadm.conf`

Append new RAID info into the file to mdadm reassemble array:

`sudo mdadm --detail --brief /dev/md0 | sudo tee -a /etc/mdadm/mdadm.conf`

Now the RAID have 4 active devices but only have 3 working devices. You need to grow up the RAID with:

`sudo mdadm --grow /dev/md0 --raid-devices=4 --add /dev/sde`

And resize the filesystem:

`sudo resize2fs /dev/md0`

![Output](https://raw.githubusercontent.com/nh4ttruong/nh4ttruong.github.io/main/_posts/create-manage-raid-array/result-add.png)

### Remove a devide from RAID array

Before you remove a devices from RAID array, you must to fail it first with:

`sudo mdadm /dev/md0 --fail /dev/sde`

Next, let's remove it:

`sudo mdadm /dev/md0 --remove /dev/sdc`

After you remove the device, you need to re-edit the fstab file and mdadm file as same as previous part. Now, you were remove the devices succesfully ^^

Good luck <3

## Note and reference
- This is a post which is wrote to share the way to create and manage RAID array.
- Special thank for linux.die.net and Digital Ocean Community.
- If you like my post, let you give me a star in Github and a like/follow in the youtube video 😄. Thanks a lot ❤️
