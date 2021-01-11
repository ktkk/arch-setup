# Arch Install
By KatKak: https://github.com/ktkk

**IMPORTANT**: This is purely designed for my personal use and is probably (definitely) flawed in many ways

## ISO
Get the ISO from https://www.archlinux.org/download/

Boot into live environment.
Let systemd do it's thing.

## PART 1, THE LIVE ENVIRONMENT

**If you're using a US keyboard, skip this step:**
List available keyboard with:
```console
# ls /usr/share/kbd/kemaps/**/*.map.gz
```
Set layout with:
```console
# loadkeys [insert layout here]
```

Verify the boot mode:
```console
# ls /sys/firmware/efi/efivars
```
If the directory does not exist, the system can be booted in Legacy BIOS mode:

Connect to the internet.
If using ethernet, this should just work automatically:
```console
# ping archlinux.org -c 4
```
If using wifi, check out the [Arch Wiki](https://wiki.archlinux.org/index.php/Network_configuration/Wireless).

update the system clock:
```console
# timedatectl status
# timedatectl set-ntp true
```

Partition the drive.
To list drives:
```console
# lsblk
```
To partition the drive:
```console
# cfdisk /dev/sdX
```
| Legacy BIOS mode | In UEFI mode: |
| :------------------------------ | :------------------------------ |
| `/dev/sdX1` = Linux | `/dev/sdX1` = EFI System partition |
| `/dev/sdX2` = Linux swap/Solaris | `/dev/sdX2` = Linux |
| | `/dev/sdX3` = Linux swap/Solaris |

Format the newly created partitions:
```console
# mkfs.ext4 /dev/sdX1
```
```console
# mkswap /dev/sdX2 #(or /dev/sdX3 in cade of UEFI)
# swapon /dev/sdX2 #(...)
```

Mount the file system to to /mnt:
```console
# mount /dev/sdX1 /mnt
```
If dual booting Windows, mount the Windows partition to /mnt/win:
```console
# mount /dev/[windows drive/partition] /mnt/win
```

Edit the pacman mirrorlist by putting the closest region at the top of the file for faster downloads:
```console
# vim /etc/pacman.d/mirrorlist
```

Install the system.
Don't forget to also install a text editor to edit config files while chrooted.
I'll also go ahead and install dhcpcd for networking and git for accessing dotfile repos.
```console
# pacstrap /mnt base linux linux-firmware base-devel vim git dhcpcd
```

Generate the file system table:
```console
# genfstab -U /mnt >> /mnt/etc/fstab
```
Check the fs table for errors:
```console
# cat /mnt/etc/fstab
```

Chroot in to the new system:
```console
# arch-chroot /mnt
```

## PART 2, THE NEW SYSTEM

Set the time zone by creating a symlink:
```console
# ln -sf /usr/share/zoneinfo/[Region]/[City] /etc/localtime
```
Sync the harware clock:
```console
# hwclock --systohc
```

Set the locales.
Edit /etc/locale.gen and uncomment the needed locales (mainly en_US.UTF-8 UTF-8).
```console
# vim /etc/locale.gen
# locale-gen
```
Create the locale config file:
```console
# echo "LANG=en_US.UTF-8" > /etc/locale.conf
# cat /etc/locale.conf
```
Set the keyboard layout if necessary:
```console
# echo "KEYMAP=[keyboard layout]" > /etc/vconsole.conf
# cat /etc/vconsole.conf
```

Set the hostname (`user@HOSTNAME $`)
```console
# echo "[hostname]" > /etc/hostname
# vim /etc/hosts
```
Add following lines to /etc/hosts:
```
127.0.0.1	localhost
::1		localhost
127.0.1.1	[hostname].localdomain [hostname]
```
Replace 127.0.1.1 with the permanent IP if using a permanent IP.

Create a root password:
```console
# passwd
```

Create a user account:
```console
# useradd -m [username]
```
Give the user account a password:
```console
# passwd [username]
```
Add user to important groups:
```console
# usermod -aG wheel,audio,video,optical,storage [username]
```
Install sudo and edit the sudoers file to allow the wheel group to use sudo:
```console
# pacman -S sudo
# visudo
```
To add the default user directories specified by the XDG Base Directory specifications:
```console
# pacman -S xdg-user-dirs
# xdg-user-dirs-update
```

Set up dhcpcd so that internet access is available on reboot:
```console
# systemctl enable dhcpcd
```

Install grub as a bootloader.
```console
# pacman -S grub
# grub-install /dev/sdX
# grub-mkconfig -o /boot/grub/grub.cfg
```

Shut down the system and remove the install media.
```console
# exit
# shutdown now
```

## PART 3, AFTER INSTALLATION

Log in to the new system using the previously created user account.

Install yay as an AUR helper:
```console
$ git clone https://aur.archlinux.org/yay.git
$ cd yay
$ makepkg -si
```

Install a graphical environment.
```console
$ sudo pacman -S xorg lightdm lightdm-gtk-greeter [desktop environment/window manager of choice + additional configuration programs]
$ sudo systemctl enable lightdm
```

Install additional programs of choice (eg. terminal emulator, file browser and web browser).
```console
$ sudo pacman -S rxvt-unicode pcmanfm firefox neofetch feh zathura scrot
```

Install display drivers if needed.
```console
$ sudo pacman -S nvidia
```

Reboot.
```console
$ reboot
```
Systemd should automatically start the xorg server.

## Enjoy the new glorious Arch Linux (btw) system!
