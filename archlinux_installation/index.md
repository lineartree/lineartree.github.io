# Arch Linux Installation


Arch Linux Installation Steps.

<!--more-->

## Preparation

download archlinx iso from official site, and create a live USB using etcher

https://www.balena.io/etcher

## Config Before Installation

make terminal font bigger

```bash
cd /usr/share/kbd/consolefonts/
setfont ter-d20b.psf.gz
```

in `/etc/makepkg.conf`

```pkgconfig
MAKEFLAGS="-j$(expr $(nproc) \+ 1)"
```

in `/etc/pacman.conf`

```ini
Color
ParallelDownloads = 5
```

```pacmanconf
[core]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist

[extra]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist

#[community-testing]
#Include = /etc/pacman.d/mirrorlist

[community]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist
```

```bash
pacman -Syy
```

## Partition

### UEFI System

```bash
fdisk -l
```

```bash
device     Size  ID Type
/dev/sda1  512M  ef EFI (FAT-12/16/32)
/dev/sda2    4G  82 Linux Swap /Solaris
/dev/sda3 35.5G  83 Linux
```

```bash
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.ext4 /dev/sda3
```

### Non-UEFI System
Creating filesystem for non-UEFI system
For non-UEFI system, you only have one single root partition. So just make it ext4:

```bash
mkfs.ext4 /dev/sda1
```

## Mount to /mnt

```bash
mount /dev/sda3 /mnt
```

```bash
pacstrap /mnt base linux linux-firmware vim
```

```bash
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

## Set Time and Locale
```bash
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```

```bash
vim /etc/locale.gen
```

```bash
# uncomment this line
en_US.UTF-8 UTF-8
```

```bash
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

the name is the computer's name

```bash
echo myarch > /etc/hostname
```

```bash
vim /etc/hosts
```

```ini
127.0.0.1 localhost
::1 localhost
127.0.1.1 myarch
```

```bash
passwd
```


## Install Grub Boot Loader

### UEFI System
```bash
pacman -S grub efibootmgr
mkdir /boot/efi
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

### Non-UEFI System
Install grub on Non-UEFI systems
Install grub package first:

```
pacman -S grub
```

And then install grub like this (don’t put the disk number sda1, just the disk name sda):

```
grub-install /dev/sda
```

Last step:

```
grub-mkconfig -o /boot/grub/grub.cfg
```

## Add User

the name is the user's name

```bash
pacman -S sudo
useradd -m lpy
passwd lpy
```

```bash
usermod -aG wheel,audio,video,storage lpy
```

```bash
EDITOR=vim visudo
```

uncomment this line

```bash
## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL:ALL) ALL
```

## Install NetworkManager
```bash
pacman -S networkmanager
```

```bash
systemctl enable NetworkManager.service
```

```bash
exit
```

```bash
umount /mnt
```

or

```bash
umount -l /mnt
```

```bash
shutdown now
```

{{< admonition note "Set config after installation">}}
config terminal fonts
```bash
sudo pacman -S terminus-font
cd /usr/share/kbd/consolefonts/
setfont ter-d20b.psf.gz
```

in `/etc/vconsole.conf`
```
FONT=ter-d20b.psf.gz
```
  
in `/etc/makepkg.conf`

```ini
MAKEFLAGS="-j$(expr $(nproc) \+ 1)"
```

in `/etc/pacman.conf`

```pacmanconf
Color
ParallelDownloads = 5
```

```pacmanconf
[core]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist

[extra]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist

#[community-testing]
#Include = /etc/pacman.d/mirrorlist

[community]
Server = http://archlinux.cs.nctu.edu.tw/$repo/os/$arch
Include = /etc/pacman.d/mirrorlist
```
{{< /admonition >}}

## Install Yay AUR Helper

```bash
pacman -Syy
```

copy github ip to `/etc/hosts`:

```bash
140.82.114.3 github.com
```

```bash
sudo pacman -S base-devel git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## Install i3 and Display Manager

### Lightdm

```bash
yay -S lightdm lightdm-webkit2-greeter
sudo systemctl enable lightdm.service
```

theme

```bash
yay -S lightdm-webkit-theme-aether
yay -R lightdm-webkit-theme-aether
yay -S lightdm-webkit-theme-aether-git
```

### SDDM
```bash
yay -S sddm
yay -S sddm-sugar-dark
sudo systemctl enable sddm.service
```

view themes in `/usr/share/sddm/themes`

in `/etc/sddm.conf`

```ini
[Theme]
Current=sugar-dark
```

disable showing password when typing

in `/usr/share/sddm/themes/sugar-dark/theme.conf`

set `ForceHideCompletePassword` to `true`

```ini
ForceHideCompletePassword=true
## If you don't like to see any character at all not even while being entered set this to true.
```


### i3

```bash
sudo pacman -S i3 xorg xorg-server alacritty kitty rofi
```

```bash
sudo pacman -S xdg-user-dirs
xdg-user-dirs-update
```


