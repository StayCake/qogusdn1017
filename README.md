```shell
#!/bin/bash
# Hyeon's own Arch Linux Automatic Installation script
# THIS SCRIPT IS NOT FINISHED YET!!!!!!!!!!!!
#
# Download this script...
#
# With wget:
#
# wget -O install.sh https://raw.githubusercontent.com/qogusdn1017/qogusdn1017/master/README.md ; tail -n +2 install.sh > temp.sh ; head -n -1 temp.sh > install.sh ; rm -rf temp.sh
#
# With curl:
#
# curl https://raw.githubusercontent.com/qogusdn1017/qogusdn1017/master/READEME.md" > install.sh ; tail -n +2 install.sh > temp.sh ; head -n -1 temp.sh > install.sh ; rm -rf temp.sh
#
# This script is licensed under Do What The Fuck You Want To Public License v2.0.

############# WARNING #############
#
# THIS SCRIPT DOES NOT PARTITION YOUR DRIVE. (INCLUDING FORMATTING.)
# THERE ARE MANY EXCEPTIONAL CIRCUMSTANCES WHILE PARTITIONING, SO THIS SCRIPT WILL DIRECTLY MOUNT THE PARTITION.
# ALTERNATIVELY, YOU CAN EDIT SCRIPT_CONFIG AT HOME DRIVE (~) TO PARTITION WITH MY CONFIGURATIONS.
# I AM NOT RESPONSIBLE FOR ANY DAMAGE WITH THIS CONFIG.
# 
# MY CONFIGURATIONS (~~ means that there can be other characters inside, usually sda, sdb.):
# /dev/~~1 - EFI Partition, 512 MIB. - FAT32
# /dev/~~2 - Linux Swap Partition, 4 GIB. - SWAP
# /dev/~~3 - Linux Filesystem Partition, Allocates all remaining disk space. - ext4
#
# This script only supports EFI Platforms.
# If you're running on BIOS Platform, please stop trying to use this script. IT WILL BROKE.
# I'll consider to add BIOS version of this script.
#
# This script only uses Default KDE Plasma.
# If you want to use other DEs, for now stop using this script and manually install other DEs.
#
# Mirrorlist is coming from Arch Linux Mirrorlist Generator (South Korea), but along with Kaist's mirror.
# If you want to use another mirror, simply edit the mirrorlist, re-run the script and check "I have my own Mirrorlist."
#
# Script was based on Jeon W. H.'s Arch Linux Install Guide: https://jeonwh.com/arch-install/ (Korean)
# For starters, I highly recommend to check his guide.
###################################


# TODO: Check efivars

# Configuring Mirrorlist / TODO: Own Mirrorlist Check

rm -rf /etc/pacman.d/mirrorlist
echo "http://ftp.kaist.ac.kr/ArchLinux/\$repo/os/\$arch" >> mirrorlist
echo "https://ftp.kaist.ac.kr/ArchLinux/\$repo/os/\$arch" >> mirrorlist
curl "https://archlinux.org/mirrorlist/?country=KR&protocol=http&protocol=https&ip_version=4" >> /etc/pacman.d/temp
tail -n +7 /etc/pacman.d/temp >> mirrorlist
sed -i '3,8s/.//' mirrorlist # To remove comment signification

# Partitioning
# TODO: MUST FIND SCRIPT_CONFIG AT HOME DRIVE (~) TO CHECK IF THE PARTITIONING IS SET TO TRUE.

DISKNAME=sda

parted /dev/$DISKNAME mklabel gpt -s
parted /dev/$DISKNAME mkpart primary 2048s 1050623s -s
parted /dev/$DISKNAME mkpart primary 1050624s 9439231s -s
parted /dev/$DISKNAME mkpart primary 9439232s 100% -s

# Formatting / SWAP
mkfs.vfat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.ext4 -J /dev/sda3

# Mount
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot

# Time Congfiguration, using ntp
pacman -Syu ; pacman -S ntp --noconfirm ; ntpd -q -g

# Base Package Installation / Desktop Environment Installation
pacstrap /mnt base linux linux-firmware nano vim dhcpcd networkmanager base-devel man-db man-pages texinfo dosfstools e2fsprogs git go plasma plasma-wayland-session sddm kde-applications

# genfstab
genfstab -U /mnt >> /mnt/etc/fstab

# TODO: CHROOT AND CONFIGURE USER SETTINGS, BOOTLOADER AS GRUB.
```
