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
# curl "https://raw.githubusercontent.com/qogusdn1017/qogusdn1017/master/README.md" > install.sh ; tail -n +2 install.sh > temp.sh ; head -n -1 temp.sh > install.sh ; rm -rf temp.sh
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
# If you're running on BIOS Platform, it will not work. (Checks "/sys/firmware/efi/efivars".)
# I'll consider to add BIOS version of this script.
#
# This script only uses Default DE as KDE Plasma. (DE stans for Desktop Environment in this case.)
# If you want to use other DEs, for now stop using this script and manually install other DEs.
#
# Mirrorlist is coming from Arch Linux Mirrorlist Generator (South Korea), along with Kaist's mirror by adding manually.
# If you want to use another mirror, simply edit the mirrorlist, re-run the script and check "I have my own Mirrorlist."
#
# Script was based on Jeon W. H.'s Arch Linux Install Guide: https://jeonwh.com/arch-install/ (Korean)
# For starters, I highly recommend to check his guide.
###################################

############# SCRIPT VARIABLE LOCATION #############

# TODO: READS FROM CONFIG FIRST, THEN QUESTIONS THE USER ABOUT VARIABLES IF SPECIFIC VARIABLE CONFIG IS EMPTY.

# DISKNAME / Inistal Disk Name is set to sda, will be changed during script progress.
DISKNAME=sda

# PARTNUM / Initial Partition Number is set to 0, will be changed during script progress.
PARTNUM=0

####################################################

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
# TODO: MUST FIND SCRIPT_CONFIG AT HOME DRIVE (~) TO CHECK IF THE PARTITIONING IS SET TO TRUE.
# TODO: But question the user first which disk name should be used. Or, Read available disks and prints them to check which disk should be used.
# TODO: SETTINGS PARTITION TYPE (FAT32, EXT4)

parted /dev/$DISKNAME mklabel gpt -s
parted /dev/$DISKNAME mkpart primary 2048s 1050623s -s # FAT32
parted /dev/$DISKNAME mkpart primary 1050624s 9439231s -s
parted /dev/$DISKNAME mkpart primary 9439232s 100% -s # EXT4

# Formatting / SWAP
mkfs.vfat -F32 /dev/$DISKNAME$PARTNUM
mkswap /dev/$DISKNAME$PARTNUM
swapon /dev/$DISKNAME$PARTNUM
mkfs.ext4 -j /dev/$DISKNAME$PARTNUM

# Mount / TODO: If partitning is off, directly mount the partition. But question the user first which disk name and partition name should be used.
mount /dev/$DISKNAME$PARTNUM /mnt
mkdir /mnt/boot
mount /dev/$DISKNAME$PARTNUM /mnt/boot

# Time Congfiguration, using ntp
pacman -Syu ; pacman -S ntp --noconfirm ; ntpd -q -g

# Base Package Installation / Desktop Environment Installation
pacstrap /mnt base linux linux-firmware nano vim dhcpcd networkmanager base-devel man-db man-pages texinfo dosfstools e2fsprogs git go plasma plasma-wayland-session sddm kde-applications

# genfstab
genfstab -U /mnt >> /mnt/etc/fstab

# TODO: CHROOT AND CONFIGURE USER SETTINGS, BOOTLOADER AS GRUB. ENABLING SOME SERVICES AND EXIT THE SCRIPT.
```
