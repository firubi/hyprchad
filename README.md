# Hyprchad
This serves to give steps to reproduce my Hyprland setup. For my old Hyprland setup (that barely utilizes the AUR), refer to this https://github.com/firubi/hyprland-gigachad.
*This is the current setup*
<img width="2560" height="1440" alt="Screenshot_2025-08-02_17 57 52" src="https://github.com/user-attachments/assets/5ba2bf7f-91df-414a-9f9e-e148042617a1" />

## Installing Arch
For a simple Arch install, refer to https://github.com/firubi/gigachad-arch-installation, or just the wiki. Lately I have been adding the CachyOS repositories, which is useful for prebuilt packages. This can be done by following their [wiki](https://wiki.cachyos.org/features/optimized_repos/). Under explicit-pkgs.txt, you will find packages I usually install on Arch (and also KDE-specific packages if you want a fully functioning KDE-desktop).

## Backup
Before continuing, I recommend useful directories and files, such as .config and .local. That way, you can always go back to a default state. Something like `mkdir ~/.backup` and `cp -r ~/.config ~/.backup` and `cp -r ~/.local ~/.backup`

## Install Hyprland dotfiles
Previously I used waybar, swaync and other packages when using Hyprland. Dotfiles from End4 takes use of Quickshell, and doesn't need separate packages for the bar, notifications or app-launcher. You could run the script, but I like doing it manually. It takes longer, but that way it makes it easier to uninstall. 
- Firstly, you will need all the dependencies, which can be found [here](https://github.com/end-4/dots-hyprland/tree/main/arch-packages). You technically don't need to install all of them, if you don't plan on using them, like tesseract for OCR. You decide for yourself :) Under end4-pkgs.txt you can find all the packages that I have installed personally (which should yield most functionality).
- You will have to enable a few services, one for virtual keyboard and one for bluetooth: `systemctl --user enable --now ydotool` and `sudo systemctl enable --now bluetooth`. The script also adds the user to the video and input group, which you can do like this `sudo usermod -aG video,input "$(whoami)"`.
- All you have to do now is place the dotfiles in .config and .local. I have edited some of the files to fit me, which you can download through this repository.
- Before rebooting, you should enable sddm: `sudo systemctl enable sddm`.

## Further customizations
The rest of this section will have customizations and tips specific to me but can also be useful to everyone else!

#### SDDM-Wayland
https://wiki.archlinux.org/title/SDDM#Wayland, and apply Wayland-settings on SDDM in System-settings. This requires the package `sddm-kcm`. If you want a clean looking theme, check out [Silent SDDM](https://github.com/uiriansan/SilentSDDM)!

#### Fcitx5
Add in /etc/environment
```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

#### Hyprland & Dolphin
In order to fix the [weird MIME behaviour](https://bbs.archlinux.org/viewtopic.php?pid=2167579#p2167579) of dolphin in Hyprland, make a link in `/etc/xdg/menus` like this: `sudo ln -s /etc/xdg/menus/plasma-applications.menu /etc/xdg/menus/applications.menu`. Either that, or you can put `env = XDG_MENU_PREFIX, plasma-` in env.conf. 

#### ZRAM-generator in CachyOS-settings
CachyOS-settings comes with zram-generator. If you followed the Arch Install Guide, you most likely already have a swap partition. Make a file `/etc/systemd/zram-generator.conf` and leave it empty (https://wiki.archlinux.org/title/Zram#Using_zram-generator). The service will be enabled automatically when the CachyOS-settings package is updated, so this is my preferred way. 

#### Virt-manager shared folder
Install the packages `virt-manager` and `qemu-base`. Add to the fstab in the virtual machine:
```
shared /home/firubi/Shared/ 9p defaults 0 0
```
The driver on the filesystem needs to be virtio-9p with the target path `shared` and a source path of your choice, for example `/home/firubi/Public/`

#### Drives
To automount drives, you need to edit /etc/fstab:
```
UUID=(UUID OF EXTERNAL SSD) /run/media/firubi/T7/ exfat nosuid,nodev,nofail,x-gvfs-show,uid=1000,gid=1000 0 0
UUID=(UUID OF NVME) /run/media/firubi/NVME2/ ext4 defaults 0 0
```

#### Hooks
Useful pacman hooks I use are [paccache-hook](https://aur.archlinux.org/packages/paccache-hook) and [systemd-boot-autoupdate](https://wiki.archlinux.org/title/Systemd-boot#pacman_hook).

#### Yomitan/-chan local audio server
Set up a local audio server with Anki with this tutorial: https://github.com/themoeway/local-audio-yomichan.

#### Adding Japanese locale in flatpak
For when you want to run Japanese games/applications through Bottles or such. No need to change the system locales. 
```
flatpak config --set extra-languages ja_JP.UTF-8
flatpak update
```
Set the environment variables.
```
LC_ALL = ja_JP.UTF-8
TZ = Asia/Tokyo
```

## Basic maintenance
- `sudo pacman -Syu` - to update
- `sudo pacman -Rns` - to remove package
- `pacman -Qq | wc -l` - to show package count
- `sudo pacman -Rns $(pacman -Qdtq)` - to remove orphans (does also remove some make dependencies for AUR and tkg)
- `paccache -r` - to remove previous versions of packages, but keep the latest 3
- `paccache -ruk0` - to remove previous versions of uninstalled packages
