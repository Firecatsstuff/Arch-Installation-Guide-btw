# Arch-Installation-Guide-btw
I'm going to make the easiest guide that I can write for users who want to switch to arch (btw) without using the archinstall script and without reading the archwiki since it's hard for some users to understand.


**DISCLAIMER**: if your terminal screen gets full, click **CTRL + L** to clear everything.


#    The Guide

# Step 1 (Optional): Changing Keyboard Layout

If you have a keyboard layout that isn't us, change it with the `loadkeys` command.

For example if you want to change it to the german keymap:


`loadkeys de`


# Step 2 (Optional): Connecting To Wi-Fi
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

You have to enter `lsblk` first to see what disk you're using. So for example mine is nvme0n1 but yours could be sda, sdb etc.
So you have to remember your given name for the disk to use cfdisk like this:

`cfdisk /dev/diskname`

Delete all partitions until you only see Free Space at the top. You have to create new partitions for the arch installations.

**1. (Boot)** Make the size 1G since it's preferred.

**2. (Swap)** Make the size 4G or more.

**3. (Root)** Just press enter if it asks you for the size.

Afterwards if you created those partitions, select "Commit" in cfdisk then exit it by selecting "Quit".

# Step 4: Formatting The Partitions

First execute `lsblk` to recognize all partitions.

The partitions have numbers at the end so under your disk there should be the partitions you just created.

Firstly you're going to format the root partition first:

`mkfs.ext4 /dev/yourrootpartition`

Then you're going to format your boot partition:

`mkfs.fat -F 32 /dev/yourbootpartition`

Lastly the swap partition

`mkswap /dev/yourswappartition`

# Step 5: Mounting The Partitions

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

# Step 6: Installing Base Packages

Install the base packages like this (only include efibootmgr if you got a uefi system and include networkmanager if you need to connect to wifi):

`pacstrap /mnt base linux linux-firmware sof-firmware alsa-utils pipewire pipewire-alsa pipewire-pulse wireplumber base-devel grub networkmanager efibootmgr nano`

If you use an amd or intel cpu, make sure to add `adm-ucode` or `intel-ucode` too

# Step 7: FsTab (File System tab)

Firstly type:

`genfstab /mnt'

It will show you the file system tab. But now you have to copy the contents into a file likes this:

`genfstab /mnt > /mnt/etc/fstab`

Now check the contents of the file to make sure it worked:

`cat /mnt/etc/fstab`

If it shows the same contents as it did by running the `genfstab` command, that means it worked.

# Step 8: Setting Up The Local Timezone

Change your root to your system first:

`arch-chroot /mnt`

Now view available citiews like this:

`ls /usr/share/zoneinfo/yourcontinent/`

(Replace `yourcontinent` with your actual continent)

Afterwards you can setup your local time like this:

`ln -sf /usr/share/zoneinfo/yourcontinent/yourcity /etc/localtime`

Check your date to see if it worked:

`date`

Lastly synchronize the system clock like this:

`hwclocl -systohc`

# Step 9: Localization

Firstly edit the locale.gen file like this:

`nano /etc/locale.gen`

Remove the hashtag opn the locale you want so if you want english locales remove the hashtag of the line that consist of:

`en_US.UTF-8 UTF-8`

To exit nano do these keybinds: **CTRL+O**, **ENTER** and **CTRL+X**

Now enter this command to actually generate the locales:

`locale-gen`

You also have to specify your locale in the locale.conf file like this:

`nano /etc>/locale.conf`

Proceed by adding this in the file:

`LANG:en_US.UTF-8`

exit nano with the same keybinds I've mentioned before.

# Step 10: Keymap (Optional):

if you have a keyboard layout other than us, follow these steps:

Firstly edit the vconsole.conf file like this:

`nano /etc/vconsole.conf`

Add this in the file:

`KEYMAP=yourkeyboardlayout`

(Obviously replace "yourkeyboardlayout" with your actual layout)
Write the file then exit it.

# Step 11: Host Name

Edit /etc/hostname with `nano` and type in the host name you want.

# Step 12: Creating User And Changing Passwords

First change your root password by using this command:

`passwd`

It asks you to type in your new password twice then you're already done.

To add your personal user enter this:

`useradd -m -G wheel -s /bin/bash yourusername`

**(Obviously you have to replace `yourusername` with your username of choice)**

To add a password to your user enter this command:

`passwd yourusername`

Now, you can't execute commands with sudo on your user yet because "your user is not in the sudoers file"
To prevent that edit the file like this:

`EDITOR=nano visudo`

Find the line that consist of:

**%wheel ALL=(ALL) ALL**

remove the hashtag to properly execute commands with sudo.

# Step 13: Installing Packages Of Choice

So let`s say you want to install plasma on your machine, install the needed packages like this:

`sudo pacman -S sddm plasma konsole firefox`

# Step 14: Enabling needed services

So you have to enable sddm to login in plasma and NetworkManager if you want to connect to Wi-Fi.

Enable SDDM like this:

`systemctl enable sddm`

and NetworkManager (optional) like this:

`systemctl enable NetworkManager`

# Step 15: Installing Grub

First run `lsblk` command to identify the given disk name. Remember,it is over your created partitions.
If you know what the given name is,run the command to install grub on the disk:

`grub-install /dev/disksgivenname`

If it finished run this command to update grub:

`grub-mkconfig -o /boot/grub/grub.cfg`

# Step 16 (Last Step): Unmounting and rebooting into the new system

Enter `exit` to get into the archiso command line again and run this to unmount everything:

`umount -a`

(No this is not a typo btw)


# Ignore the errors saying target is busy and now you can finally boot into your new arch setup withoout archinstall!
