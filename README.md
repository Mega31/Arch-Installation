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
#####pacstrap######
pacstaro /mnt base-devel #and all other things you need
###after pacstrap######
genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt
####In chroot#######
ln -sf /usr/share/zoneinfo/India/Kolkata /etc/localtime
hwclock --systohc
nano /etc/locale.gen
uncomment en_US.UTF-8
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
echo arch >> /etc/hostname
nano /etc/hosts
127.0.0.1   localhost.localdomain   localhost 
::1         localhost.localdomain   localhost
127.0.1.1   arch.localdomain        arch 
#####Time for root pass and creating user #######
passwd #for root
useradd -m -G wheel -s /bin/bash (user name)
passwd (username you created)
###after creating user and setting pass###
nano /etc/pacman.conf
->uncomment 
[multilib]
include = /etc/pacman.d/mirrorlis
####time to install grub and all other things you want and installing grub and making dir#####
pacman -S dosfstools grub efibootmgr intel-ucode 
mkdir /boot/efi
mount /dev/sda1 /boot/efi (sda1 is here your efi/mbr partion which stores your bootloder)

grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub --recheck
grub-mkconfig -o boot/grub/grub.cfg
###Finishing up####
exit      # If still on arch-chroot mode
umount -R /mnt
reboot
