```shell
#!/bin/bash
# Hyeon's own Arch Linux Automatic Installation script
# THIS SCRIPT IS NOT FINISHED YET!!!!!!!!!!!!
#
# Download this script...
#
# With wget:
#
# wget -O https://baehyeonwoo.com/hyeonalis/install.sh
#
# With curl:
#
# curl "https://baehyeonwoo.com/hyeonalis/install.sh" > install.sh
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
# Script was based on Jeon W. H.'s Arch Linux Install Guide: https://jeonwh.com/arch-install/ (Korean)
# For starters, I highly recommend to check his guide.
###################################

# EFI Check

if [ -d "/sys/firmware/efi/efivars" ]
then
    return
else
    echo "This computer is not in EFI Platform. Please check the EFI state and try again."
    exit
fi

# Configuring Mirrorlist / TODO: Own Mirrorlist Check

rm -rf /etc/pacman.d/mirrorlist
echo "Server = http://ftp.kaist.ac.kr/ArchLinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist
echo "Server = https://ftp.kaist.ac.kr/ArchLinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist
curl "https://archlinux.org/mirrorlist/?country=KR&protocol=http&protocol=https&ip_version=4" >> /etc/pacman.d/temp
tail -n +7 /etc/pacman.d/temp >> /etc/pacman.d/mirrorlist
sed -i '3,10s/.//' /etc/pacman.d/mirrorlist # To remove comment signification
rm -rf /etc/pacman.d/temp

# Partitioning

parted /dev/sda mklabel gpt -s
parted /dev/sda mkpart primary 2048s 1050623s -s # FAT32
parted /dev/sda mkpart primary 1050624s 9439231s -s
parted /dev/sda mkpart primary 9439232s 100% -s # EXT4

# Formatting / SWAP
mkfs.vfat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.ext4 -j /dev/sda3

# Mount
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot

# Time Congfiguration, using ntp
pacman -Syu ; pacman -S ntp --noconfirm ; ntpd -q -g

# Base Package Installation / Desktop Environment Installation
pacstrap /mnt base linux linux-firmware nano vim dhcpcd base-devel man-db man-pages texinfo dosfstools e2fsprogs git go

# genfstab
genfstab -U /mnt >> /mnt/etc/fstab

# TODO: CHROOT AND CONFIGURE USER SETTINGS, BOOTLOADER AS GRUB. ENABLING SOME SERVICES AND EXIT THE SCRIPT.

arch-chroot /mnt bash -c ""
```
