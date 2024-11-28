## 1. Set Up Keyboard Layout

1. 
   ```
   localectl list-keymaps
   ```
2. 
   ```
   loadkeys de
   ```

## 2. Check Internet Connection

1. 
   ```
   ping -c 2 archlinux.org
   ```

## 3. Connect to the Internet

1. 
   ```
   iwctl
   ```
2. 
   ```
   device list
   ```
3. 
   ```
   exit
   ```

## Optional: Connect to SSH

1. 
   ```
   systemctl start sshd
   ```
2. 
   ```
   passwd
   ```
3. 
   ```
   ip addr
   ``` 
4.
   ```
   ssh root@ipadress
   ```

## 4. Partition the Disk

1. 
   ```
   lsblk
   ```
2. 
   ```
   cfdisk /dev/sda
   ```
    
### CSM/Lagacy:
3. - Choose: `dos`
   - New -> 512M -> Primary -> Press: B -> Bootable
   - New -> Everything -> Primary
   - Write -> yes
   - Quit

Optional Swap Partition: New -> Half of Ram or 8GB if you have 16GB or more Ram

  ### UEFI: 
   - Choose `gpt`
   - New -> 512m -> Type -> EFI System
   - New -> Everything -> Linux Filesystem
   - Write -> yes
   - Quit

Optional Swap Partition: New -> Half of Ram or 8GB if you have 16GB or more Ram -> Linux Swap 

## 5. Format Partitions

### CSM/Lagacy:
1. 
   ```
   mkfs.ext4 /dev/sda1
   ```
2. 
   ```
   mkfs.ext4 /dev/sda3 (sda2 without Swap Partition)
   ```

### UEFI:
1. 
   ```
   mkfs.fat -F32 /dev/sda1
   ```
2. 
   ```
   mkfs.ext4 /dev/sda3 (sda2 without Swap Partition)
   ```

### Optional Swap Parition Lagacy/UEFI:

Otional Swap:

1. 
   ```
   mkswap /dev/sda2
   ```
2. 
   ```
   swapon /dev/sda2
   ```

## 6. Install Linux Base System

### CSM/Lagacy:

1. 
   ```
   mount /dev/sda3 /mnt
   ```
2. 
   ```
   mkdir /mnt/boot
   ```
3. 
   ```
   mount /dev/sda1 /mnt/boot
   ```
4. 
   ```
   lsblk
   ```
5. 
   ```
   pacstrap /mnt base base-devel linux linux-firmware
   ```
   
### UEFI:

1. 
   ```
   mount /dev/sda3 /mnt
   ```
2. 
   ```
   mkdir /mnt/boot
   ```
3. 
   ```
   mkdir /mnt/boot/efi
   ```  
4. 
   ```
   mount /dev/sda1 /mnt/boot/efi
   ```
5. 
   ```
   lsblk
   ```
6. 
   ```
   pacstrap /mnt base base-devel linux linux-firmware
   ```

## 7. Install Required Packages

1. sudo pacman -S networkmanager dhcpcd nano

## 8. Configure Fstab

1. 
   ```
   genfstab -U /mnt >> /mnt/etc/fstab
   ```

## 9. Using the installed root system

1. 
   ```
   arch-chroot /mnt /bin/bash
   ```

## 10. Install Bootloader

1. 
   ```
   bootctl install
   ```
2. 
   ```
   nano /boot/loader/entries/arch.conf (name it what ever you want)
   ```
- paste this into the file:

```
title Arch
Linux /vmlinux-linux
initrd /initramfs-linux.img
```

3. 
   ```
   echo "Options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/sda3) rw" >> /boot/loader/entries
   ```

## 10. Configure Arch

1. 
   ```
   sudo pacman -S networkmanager dhcpcd
   ```
 2. 
   ```
   sudo systemctl enable dhcpcd@enp56s0
   ```
3. 
   ```
   sudo systemctl enable NetworkManager
   ```
4. 
   ```
   sudo nano /etc/pacman.conf
   ```
   
- search "multilib" -> remove "#" -> normal multilib not like multilib-testing + remove "#" -> include

5. 
   ```
   sudo pacman -Sy
   ```
6. 
   ```
   systemctl enable NetworkManager
   ```
7. 
   ```
   passwd
   ```
8. 
   ```
   nano /etc/locale.gen
   ```
   - Search for the language -> remove "#" from UTF and ISO.

9. 
   ```
   locale-gen
   ```
10. 
   ```
   nano /etc/locale.conf
   ```
   - `LANG=language` (name of the previously configured language).

11. 
    ```
    nano /etc/hostname
    ```
    - `type your pc name here`.

12. 
    ```
    ln -sf /usr/share/zoneinfo/ (TAB 1)
    ```
13. 
    ```
    ln -sf /usr/share/zoneinfo/region (TAB 2)
    ```
14. 
    ```
    ln -sf /usr/share/zoneinfo/region/city /etc/localtime
    ```
15. 
    ```
    exit
    ```

## 11. Finish Installation 

1. 
   ```
   umount -R /mnt
   ```
2. 
   ```
   reboot
   ```

## 12. Set Up a New User

1. 
   ```
   loadkeys de
   ```
2. 
   ```
   useradd -m -G wheel -s /bin/bash username
   ```
3. 
   ```
   passwd username
   ```
4. 
   ```
   nano /etc/sudoers
   ```
   - Remove "#" from "# %wheel ALL=(ALL) ALL".
   - Add "Defaults rootpw" to use the root password for the sudoers

5. 
   ```
   logout
   ```

## Optional: Set Up SSH

1. 
   ```
   sudo pacman -S openssh
   ```
2. 
   ```
   sudo systemctl start sshd
   ```
3. 
   ```
   ip addr
   ```
4.
   ```
   ssh root@ipadress
   ```

## 13. Install GUI

### Install Xorg & Nvida Packages & Desktop Enviroment & Display Manager

1. 
   ```
   sudo pacman -S xorg xorg-server nvidia-dkms libglvnd nvidia-utils opencl-nvidia lib32-libglvnd lib32-nvidia-utils lib32-opencl-nvidia nvidia-settings
   ```
- example for a GUI: 
   ```
   sudo pacman -S plasma sddm
   ```
   
- example for other recommended packages:
   ```
   sudo pacman -S flatpak dolphin mpv git fastfetch wget gedit fzf thermald zram-generator konsole
   ```
- example for all packages:
   ```
   sudo pacman -S flatpak dolphin mpv git fastfetch wget gedit fzf thermald zram-generator konsole plasma sddm xorg xorg-server nvidia-dkms libglvnd nvidia-utils opencl-nvidia lib32-libglvnd lib32-nvidia-utils lib32-opencl-nvidia nvidia-settings
   ```
2. 
   ```
   sudo pacman -S linux-headers
   ```
3. 
   ```
   sudo nano /etc/mkinitcpio.conf
   ```
add this to "MODULES=()" -> `nvidia nvidia-modeset nvidia_uvm nvidia_drm`

### Desktop Environments

- GNOME:
   ```
   gnome
   ```
- KDE Plasma:
   ```
   plasma
   ```
- XFCE:
   ```
  xfce4
   ```
- Cinnamon:
   ```
   cinnamon
   ```
- Deepin:
   ```
   deepin 
   ```
- Budgie:
   ```
   budgie-desktop
   ```

### Display Managers

- GNOME: 
   ```
   gdm
   ```
- KDE: 
   ```
   sddm
   ```
- XFCE, Cinnamon, Mate: 
   ```
   lightdm lightdm-gtk-greeter
   ```

1. Enable the display manager:
   ```
   sudo systemctl enable gdm
   ```

### Configure Nvidia
   ```
   sudo nano /boot/loader/entries/arch.conf
   ```

- add this beside rw -> nvidia-drm.modeset=1

```
sudo mkdir /etc/pacman.d/hooks
```

```
sudo nano /etc/pacman.d/hooks/nvidia.hook
```

- add this to the file:
```
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia

[Action]
Depends=mkinitcpio
When=PostTransaction
Exec=/usr/bin/mkinitcpio -P
```

2. Reboot system:
   ```
   sudo reboot
   ```

## 14. Change Keyboard Layout

1. 
   ```
   localectl list-keymaps
   ```
2. 
   ```
   sudo localectl set-keymap de
   ```
3. 
   ```
   sudo reboot
   ```

## 15. Finalize Build

1. Install Terminal via Discover/Software. (only if you doent installed "konsole" or "console" in the insterllation process)

2. 
   ```
   sudo nano /etc/pacman.conf
   ```
   - Remove "#" from `Parallel downloads = 5/10/20`.
3. 
   ```
   git clone https://aur.archlinux.org/yay.git
   ```
    
   ```
   cd yay
   ```
    
   ```
   makepkg -si
   ```

4. 
   ```
   git clone https://aur.archlinux.org/paru.git
   ```
    
   ```
   cd paru
   ```
    
   ```
   makepkg -si
   ```
