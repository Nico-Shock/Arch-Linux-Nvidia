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

## 4. Partition the Disk

1. 
   ```
   lsblk
   ```
2. 
   ```
   cfdisk /dev/sda
   ```

3. - Choose: `dos` or `gpt`
   - New -> 512M -> Primary -> Press: B -> Bootable
   - New -> Everything -> Primary
   - Write -> yes
   - Quit

## 5. Format Partitions

1. 
   ```
   mkfs.ext4 /dev/sda1
   ```
2. 
   ```
   mkfs.ext4 /dev/sda2
   ```

## 6. Install Linux Base System

1. 
   ```
   mount /dev/sda2 /mnt
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
   pacstrap /mnt base base-devel linux linux-firmware nano networkmanager grub
   ```

## 7. Configure Fstab

1. 
   ```
   genfstab -U /mnt >> /mnt/etc/fstab
   ```

## 8. Install Grub Bootloader

1. 
   ```
   arch-chroot /mnt /bin/bash
   ```
2. 
   ```
   grub-install /dev/sda
   ```
3. 
   ```
   grub-mkconfig -o /boot/grub/grub.cfg

## 9. Configure Arch

if not done already: 
   ```
   arch-chroot /mnt /bin/bash
   ```
1. 
   ```
   systemctl enable NetworkManager
   ```
2. 
   ```
   passwd
   ```
3. 
   ```
   nano /etc/locale.gen
   ```
   - Search for the language -> remove "#" from UTF and ISO.

4. 
   ```
   locale-gen
   ```
5. 
   ```
   nano /etc/locale.conf
   ```
   - `LANG=language` (name of the previously configured language).

6. 
    ```
    nano /etc/hostname
    ```
    - `type your pc name here`.

7. 
    ```
    ln -sf /usr/share/zoneinfo/ (TAB 1)
    ```
8. 
    ```
    ln -sf /usr/share/zoneinfo/region (TAB 2)
    ```
9. 
    ```
    ln -sf /usr/share/zoneinfo/region/city /etc/localtime
    ```
10. 
    ```
    exit
    ```

## 10. Finish Installation 

1. 
   ```
   umount -R /mnt
   ```
2. 
   ```
   reboot
   ```

## 11. Set Up a New User

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

## 12. Install GUI

### Install Xorg

1. 
   ```
   sudo pacman -S xorg xorg-server
   ```

### Graphics Drivers

- NVIDIA:
   ```
   nvidia nvidia-utils
   ```
- Intel:
   ```
   xf86-video-intel
   ```
- AMD:
   ```
   xf86-video-amdgpu
   ```

### Desktop Environments

- GNOME:
   ```
   gnome
   ```
- For KDE Plasma:
   ```
   plasma
   ```
- For XFCE:
   ```
  xfce4
   ```
- For Cinnamon:
   ```
   cinnamon
   ```
- For Deepin:
   ```
   deepin 
   ```
- For Budgie:
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

2. Enable the display manager:
   ```
   sudo systemctl enable gdm
   ```

3. Reboot the system:
   ```
   sudo reboot
   ```

## 13. Change Keyboard Layout

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

## 14. Finalize Build

1. Install Terminal via Discover/Software.
2. 
   ```
   sudo nano /etc/pacman.conf
   ```
   - Remove "#" from `Parallel downloads = 5/10/20`.
  
3.  
   ```
   sudo pacman -S flatpak dolphin mpv git fastfetch wget gedit fzf htop thermald zram-generator timeshift
4. 
   ```
   git clone https://aur.archlinux.org/yay.git
   ```
    
   ```
   cd yay
   ```
    
   ```
   makepkg -si
   ```
