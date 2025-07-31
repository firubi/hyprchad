# Hyprchad
This serves to give steps to reproduce my Hyprland setup. For my old Hyprland setup (that barely utilizes the AUR), refer to this https://github.com/firubi/hyprland-gigachad.

## Installing Arch
For a simple Arch install, refer to https://github.com/firubi/gigachad-arch-installation, or just the wiki. Lately I have been adding the CachyOS repositories, which is useful for prebuilt packages. This can be done by following their [wiki](https://wiki.cachyos.org/features/optimized_repos/). Under explicit-pkgs.txt, you will find packages I usually install on Arch (and also KDE-specific packages if you want a fully functioning KDE-desktop).

## Backup
Before continuing, I recommend useful directories and files, such as .config and .local. That way, you can always go back to a default state. Something like `mkdir ~/.backup` and `cp -r ~/.config ~/.backup` and `cp -r ~/.local ~/.backup`

## Install Hyprland dotfiles
Previously I used waybar, swaync and other packages when using Hyprland. Dotfiles from End4 takes use of Quickshell, and doesn't need separate packages for the bar, notifications or app-launcher. You could run the script, but I like doing it manually. It takes longer, but that way it makes it easier to uninstall. 
- Firstly, you will need all the dependencies, which can be found [here](https://github.com/end-4/dots-hyprland/tree/main/arch-packages). You technically don't need to install all of them, if you don't plan on using them, like tesseract for OCR. You decide for yourself :) Under end4-pkgs.txt you can find all the packages that I have installed personally.
- You will have to enable a few services, one for virtual keyboard and one for bluetooth: `systemctl --user enable --now ydotool` and `sudo systemctl enable --now bluetooth`. The script also adds the user to the video and input group, which you can do like this `sudo usermod -aG video,input "$(whoami)"`.
- All you have to do now is place the dotfiles in .config and .local. I have edited some of the files to fit me, which you can download through this repository.
- Before rebooting, you should enable sddm: `sudo systemctl enable sddm`.

## Further customizations
The rest of this section will have customizations and tips specific to me but can also be useful to everyone else!
