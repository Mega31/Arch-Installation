# Arch-Installation
Installing arch on win10 pc

####start#####
wifi-menu #connect to your wifi

ping -c 3 www.google.com

pacman -Sy 

pacman -S reflector

reflector --verbose --latest 5 --sort rate --save /etc/pacman.d/mirrorlist

cfdisk #check where you have freespace and make partion

mkfs.ext4 /dev/sdax

mkswap /dev/sdax2

swapon /dev/sdX2

mount /dev/sdax /mnt #mount it to ext4 partition
