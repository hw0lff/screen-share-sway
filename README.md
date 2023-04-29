# Update 2023-04-29
The setup described in this guide was intended as a workaround.
Since then more and more software started supporting Wayland.
Screen sharing in sway works (for me) now with the following packages out-of-the-box:
- pipewire
- wireplumber
- xdg-desktop-portal
- xdg-desktop-portal-wlr

Firefox and Chromium-based browser can use xdg-desktop-portal for screen-sharing.
Setting `QT_QPA_PLATFORM=xcb` for OBS is not needed anymore since it runs natively on wayland.
OBS supports screen capturing from pipewire too.

All that makes this guide kinda obsolete.

I leave this guide here if anyone still wants to use this hacky workaround.
But I strongly discourage it.


Screen Sharing with OBS Studio on Arch Linux and Sway
===

**Please read the instructions of the listed self-built projects.**
**They explain more than I do and may have changed.**

1. [Used Software](#Used-Software)
2. [Install OBS Studio](#Install-OBS-Studio)
3. [Install the v4l2loopback kernel module](#Install-the-v4l2loopback-kernel-module)
4. [Install the wlrobs OBS-plugin](#Install-the-wlrobs-OBS-plugin)
5. [Usage of wlrobs](#Usage-of-wlrobs)
6. [Versions of the used projects at the time of writing (2021-02-04)](#Versions-of-the-used-projects-at-the-time-of-writing-(2021-02-04))


## Used Software
- [OBS Studio](https://obsproject.com/)
- [v4l2loopback](https://github.com/umlaeute/v4l2loopback)
- [wlrobs](https://hg.sr.ht/~scoopta/wlrobs)


## Install OBS Studio
OBS Studio can be downloaded from the Arch Community Repository.
```sh
pacman -S obs-studio
```


## Install the v4l2loopback kernel module
'This module allows you to create "virtual video devices".' - from the 
[v4l2loopback github page](https://github.com/umlaeute/v4l2loopback).
Visit it for more information about it.

This is needed to feed an video output from OBS and 
be used as a screen or a camera by applications.

The DKMS Version is available in the Arch Community Repository.
More information about DKMS can be found in the [Arch Wiki](https://wiki.archlinux.org/index.php/Dynamic_Kernel_Module_Support).

```sh
pacman -S v4l2loopback-dkms linux-headers

sudo dkms add -m v4l2loopback -v 0.12.5
sudo dkms build -m v4l2loopback -v 0.12.5
sudo dkms install -m v4l2loopback -v 0.12.5

dkms status v4l2loopback/0.12.5

# load the module and
# create one virtual video device
modprobe v4l2loopback 
```

> Further helpful commands:
>```sh
># the ones with the highest numbers are created by v4l2loopback
>ls /dev/video*
>
># unloads the kernel module
>modprobe -r v4l2loopback
>
># creates 4 virtual video devices
>modprobe v4l2loopback devices=4
>```


## Install the wlrobs OBS-plugin
This plugin is responsible for actually recording screens from wlroots-based compositors.
It can be installed from the [AUR](https://aur.archlinux.org/packages/wlrobs/)


## Usage of wlrobs
Now choose `Wayland output(scpy)` as new input source.

![wlrobs Source.png](wlrobs-source-add.png)

I had to flip the blue and red values from the source, 
in order to display colors correctly. 
This can be done in the source preferences.

![wlrobs Source Preferences.png](wlrobs-source-preferences.png)

Now you should be able to share your screen i.e. in Firefox.
You can test it [here](https://mozilla.github.io/webrtc-landing/gum_test.html).

Thank you for reading.


## Versions of the used projects at the time of writing (2022-01-06)
> Linux 5.15.12.arch1-1
>
> SwayWM 1:1.6.1-2
>
> OBS 27.1.3-3
>
> v4l2loopback-dkms 0.12.5-1
>
> wlrobs 1.0-1 (AUR)
