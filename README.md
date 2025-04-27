# Arch-Installation-Guide-btw
I'm going to make the easiest guide that I can write for users who want to switch to arch (btw) without using the archinstall script and without reading the archwiki since it's hard for some users to understand.



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

In the Archwiki it is considered using fdisk but there is another tool called `cfdisk` and it is easier since it has a text user interface.
