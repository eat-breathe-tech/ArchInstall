# Arch Installation Guide
Command List for the Arch Install Guide video located at: https://youtu.be/Zt0t04qZahk


Ensure that we're using EFI

``ls /sys/firmware/efi``


Check Network Connection

``ping 1.1.1.1``


Connect using wifi

``ip addr``

``wifi-menu <wireless adapter>``


Check the installed disks

``lsblk``

Partition Disks

``cfdisk /dev/sda``

Format disks

``mkfs.fat -F32 /dev/sda1``

``mkswap /dev/sda2``

``mkfs.ext4 /dev/sda3``

Enable the swap partition

``swapon /dev/sda2``

Mount the root disk

``mount /dev/sda3 /mnt``

Generate a decent mirrorlist
``pacman -Sy``

``pacman -S reflector``

``reflector –country United\ States –latest 5 –sort rate –save /etc/pacman.d/mirrorlist``

Download initial arch install

``pacstrap /mnt base base-devel``


Generate an fstab

``genfstab -U -p /mnt /mnt/etc/fstab``


Copy over new mirrorlist

``rm /mnt/etc/pacman.d/mirrorlist``

``cp /etc/pacman.d/mirrorlist /mnt/etc/pacman.d/mirrorlist``


Chroot to install directory

``arch-chroot /mnt``


Set the machine hostname

``echo "<hostname>" > /etc/hostname``


Edit locale

``nano /etc/locale.gen``

``locale-gen``

``echo LANG=en_US.UTF-8 > /etc/locale.conf``

``export LANG=en_US.UTF-8``

Set timezone

``rm /etc/localtime``

``ln -s /usr/share/zoneinfo/America/Chicago /etc/localtime``

``hwclock --systohc --utc``

Update system

``pacman -Syu``


Set root password

``passwd``


Create non-privileged user

``useradd -mg users -G wheel,storage,power -s /bin/bash <username>``

``passwd <username>``


Install sudo and update sudoers so new user has sudo privileges

``pacman -S sudo``

``EDITOR=nano visudo``

``# uncomment this line``

``# %wheel ALL=(ALL) ALL``
  

Install and configure bootloader

``pacman -S grub efibootmgr dosfstools mtools``

``mkdir /boot/EFI``

``mount /dev/sda1 /boot/EFI``

``grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --recheck``

``grub-mkconfig -o /boot/grub/grub.cfg``
