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

## 4. Optional: Connect to SSH

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

## 5. Partition the Disk

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

## 6. Create Partitions

1. 
   ```
   mkfs.ext4 /dev/sda1
   ```
2. 
   ```
   mkfs.ext4 /dev/sda2
   ```

## 7. Install Linux Base System

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
   pacstrap /mnt base base-devel linux linux-firmware nano
   ```

## 8. Configure Fstab

1. 
   ```
   genfstab -U /mnt >> /mnt/etc/fstab
   ```

## 9. Configure Arch

1. 
   ```
   arch-chroot /mnt /bin/bash
   ```
2. 
   ```
   pacman -S networkmanager grub
   ```
3. 
   ```
   systemctl enable NetworkManager
   ```
4. 
   ```
   grub-install /dev/sda
   ```
5. 
   ```
   grub-mkconfig -o /boot/grub/grub.cfg
   ```
6. 
   ```
   passwd
   ```
7. 
   ```
   nano /etc/locale.gen
   ```
   - Search for the language -> remove "#" from UTF and ISO.

8. 
   ```
   locale-gen
   ```
9. 
   ```
   nano /etc/locale.conf
   ```
   - `LANG=language` (name of the previously configured language).

10. 
    ```
    nano /etc/hostname
    ```
    - `type your pc name here`.

11. 
    ```
    ln -sf /usr/share/zoneinfo/ (TAB 1)
    ```
12. 
    ```
    ln -sf /usr/share/zoneinfo/region (TAB 2)
    ```
13. 
    ```
    ln -sf /usr/share/zoneinfo/region/city /etc/localtime
    ```
14. 
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
   useradd -m -G wheel -s /bin/bash username
   ```
2. 
   ```
   passwd username
   ```
3. 
   ```
   nano /etc/sudoers
   ```
   - Remove "#" from "# %wheel ALL=(ALL) ALL".

4. 
   ```
   logout
   ```

## 12. Optional: Set Up SSH

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

## 13. Install GUI

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

1. Install Terminal via Discover/Software.
2. 
   ```
   sudo nano /etc/pacman.conf
   ```
   - Remove "#" from `Parallel downloads = 5/10/20`.
  
3.  
   ```
   sudo pacman -S flatpak dolphin mpv git fastfetch wget gedit linux-zen fzf htop thermald zram-generator timeshift
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


5. Install Flatpak applications:
   ```
   flatpak install flathub org.winehq.Wine io.missioncenter.MissionCenter
   ```
