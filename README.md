# Arch-Installation-Guide-btw
I'm going to make the easiest guide that I can write for users who want to switch to arch (btw) without using the archinstall script and without reading the archwiki since it's hard for some users to understand.


**DISCLAIMER**: if your terminal screen is full, click **CTRL + L** to clear everything.


#    The Guide

# Step 1 (Optional): Changing Keyboard Layout

If you have a keyboard layout that isn't us, change it with the `loadkeys` command.

For example if you want to change it to the german keymap:


`loadkeys de`


# Step 2 (Optional): Connecting to Wi-Fi
Although I strongly consider using ethernet instead of wifi, you can still connect to wifi with `iwd`

So you have to execute

`iwctl`

to get into iwd and to connect to wifi you have to type this:

`station wlan0 connect WifiName`

Enter your password and test your connection by:

`ping 1.1.1.1`

(you can type any website instead of 1.1.1.1)

if it returns 64 Bytes over and over terminate it by clicking `CTRL + C`

# Step 3: Partitioning The Disk

You have to enter `lsblk` first to see what disk you're using. so for example mine is nvme0n1 but yours could be sda, sdb etc.
So you have to remember your given name for the disk to use cfdisk like this:

`cfdisk /dev/diskname`

Delete all partitions until you only see Free Space at the top. You have to create new partitions for the arch installations.
**1. (Boot)** Make the size 1G since it's preferred.
**2 (Swap)** Make the size 4G or more.
**3 (Root)** Just press enter if it asks you for the size.

Afterwards if you created those partitions, select "Commit" in cfdisk then exit it by selecting "Quit".

# Step 4: Formatting the partitions

First execute `lsblk` to recognize all partitions.

The partitions have numbers at the end so under your disk there should be the partitions you just created.

Firstly you're going to format the root partition first:

`mkfs.ext4 /dev/yourrootpartition`

Then you're going to format your boot partition:

`mkfs.fat -F 32 /dev/yourbootpartition`

Lastly the swap partition

`mkswap /dev/yourswappartition`

# Step 5: Mounting the partitions

**The Root Partition**

`mount /dev/yourrootpartition /mnt`

**The Boot Partition**

Now remember, there are 2 steps to do this: BIOS and UEFI

If you got BIOS on your computer:

`mkdir -p /mnt/boot/`

`mount /dev/yourbootpartition /mnt/boot`

If you got UEFI on your computer:

`mkdir -p /mnt/boot/efi`

`mount /dev/yourbootpartition /mnt/boot/efi`

**The Swap Partition**

`swapon /dev/yourswappartition /mnt`

