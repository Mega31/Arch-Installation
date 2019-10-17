# Dual booting arch with win10
# start

```bash
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
```

## Installation


```bash
pacstrap /mnt base base-devel linux-firmware  #and all other things you need

genfstab -U /mnt >> /mnt/etc/fstab
```



```bash
arch-chroot /mnt
```

## In chroot

```bash
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
```


## Time for root pass and creating user
```bash
passwd #for root
useradd -m -G wheel -s /bin/bash (user name)
passwd (username you created)
```
## After creating user and setting pass 
```bash
nano /etc/pacman.conf
#->uncomment 
[multilib]
include = /etc/pacman.d/mirrorlis
````
## Time to install grub and all other things you want
```bash
pacman -S dosfstools grub efibootmgr intel-ucode 
mkdir /boot/efi
mount /dev/sda1 /boot/efi (sda1 is here your efi/mbr partion which stores your bootloder)

grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub --recheck
grub-mkconfig -o boot/grub/grub.cfg
```
## End of Installation

```bash
#Enable required services
exit      # If still on arch-chroot mode
umount -R /mnt
reboot
```

## Things required for my system

This will be in pacstrap
```bash
#####at arch installation(pacstrap)#######
pacstrap /mnt base-devel gnome i3 nvidia bbswitch codeblocks intellij-idea-community-edition eclipse-jee rofi thunar networkmanager ntfs-3g lightdm lightdm-webkit2-greeter firefox chromium telegram-desktop git sudo network-manager-applet cpupower lib32-mesa xf86-video-intel acpi wpa_supplicant  dialog  xorg xorg-server-utils  xorg-xinit gnome-tweaks  	jre11-openjdk jdk11-openjdk 	openjdk11-doc gparted lxappearance feh neofetch polkit-gnome


##########Post Install########
install yay

```
