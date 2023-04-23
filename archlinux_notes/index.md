# Archlinux notes


Some notes about Arch Linux.
<!--more-->

## Archlinux Installation

arch installation
https://itsfoss.com/install-arch-linux/

## Change Console Fonts

```bash
cd /usr/share/kbd/consolefonts/
```

```bash
sudo pacman -Syu terminus-font
```

```bash
setfont ter-d20b.psf.gz
```

set it permanent, in `/etc/vconsole.conf`

```ini
FONT=ter-d20b.psf.gz
```

## Chrome Fonts

install Microsoft fonts

```bash
yay -S ttf-ms-win10-auto
```

noto-cjk

```bash
sudo pacman -S noto-fonts-cjk
```

AR PL UMing TW
```
yay -S adobe-source-han-sans-tw-font
```

Source Han Sans TW

go to chrome settings -> Appearance -> Customize fonts and select Arial

![set chrome font](./chrome_setfont.png)


## ibus-chewing

```bash
sudo pacman -S ibus ibus-chewing
```

settings

```bash
ibus-setup
```

in `/etc/locale.conf`

```ini
LANG=en_US.UTF-8
```

in `/etc/locale.gen`
uncomment this line

```ini
zh_TW.UTF-8 UTF-8
```

and do

```bash
sudo locale-gen
```

in `~./bashrc`

```ini
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
```

log out and log in

## WhiteSur Theme

https://wiki.gnome.org/action/show/Projects/GnomeShellIntegration/Installation?action=show&redirect=Projects%2FGnomeShellIntegrationForChrome%2FInstallation

shell theme
https://extensions.gnome.org/extension/19/user-themes/


```bash
git clone https://aur.archlinux.org/gnome-browser-connector.git
cd gnome-browser-connector
makepkg -si
```
gtk theme

```bash
yay -S whitesur-gtk-theme
```

icon theme

```bash
yay -S whitesur-icon-theme
```

and then go to gnome tweaks


## Adjust Brightness

install xorg

```bash
sudo pacman -S xorg
sudo pacman -S xorg-xinit xorg-twm xorg-xclock xterm
```

```shell
startx
```

use xrandr
```shell
xrandr --current
```

```shell
xrandr --output HDMI-2 --brightness 0.5
```

go to settings -> keyboard -> keyboard shortcuts to config

in keyboard section, change the `Alternative Characters Key` from `Alt` to `Right Super` to use right alt key


## Click to Minimize Menu

install dash-to-dock
https://extensions.gnome.org/extension/307/dash-to-dock/

```bash
git clone https://aur.archlinux.org/gnome-browser-connector.git
cd gnome-browser-connector/
makepkg -si
```

go to dash-to-dock setting

Behavior -> Click Action -> Minimize


## Neovim

```bash
ping github.com
```

if it stucked then 
check this site to get github ip:
https://www.ipaddress.com/site/github.com

copy the ip to /etc/hosts:

```bash
140.82.114.3 github.com
```

## Play Videos

```bash
sudo pacman -S ffmpeg
sudo pacman -S gst-plugins-good
sudo pacman -S gst-libav
```

install mpv player

```bash
yay -S mpv
````


## Vim

```bash
yay -S vim-plug
```

in vim:

```vim
:PlugInstall
```

install ycm

```bash
yay -S cmake
cd ~/.vim/plugged/youcompleteme
./install.py --clang-completer  
```

## OpenGL

```bash
sudo pacman -S glfw-x11
```

## chmod

only directories

```bash
find * -type d -exec chmod 755 {} +
```

only files

```bash
find * -type f -exec chmod 644 {} +
```


use fd to batch process, use `-X` instead of `-x`

```bash
fd -t d -X chmod 755 {}
```

```bash
fd -t f -X chmod 644 {}
```

## Alacritty + fish + tmux

### Fish Shell

```bash
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish
omf theme
omf install eclm
```

in `~/.config/fish`

```ini
if status is-interactive
    # Commands to run in interactive sessions can go here
    set -U fish_greeting ""


    fish_vi_key_bindings
    bind -M insert -m default jk backward-char force-repaint
    bind -M insert -m insert \cf accept-autosuggestion force-repaint

    function fish_mode_prompt
      # NOOP - Disable vim mode indicator
    end
    set fish_cursor_default     block      
    set fish_cursor_insert      line       
    set fish_cursor_replace_one underscore 
    set fish_cursor_visual      block

    set fish_cursor_unknown 
end
```

### Alacritty

in `~/.config/alacritty/alacritty.yml`

```yaml
import:
 - ~/.config/alacritty/themes/themes/one_dark.yaml

shell:
  program: /usr/bin/fish
  args:
    - -l
    - -c
    - "tmux"

window:
  dimensions:
    columns: 136
    lines: 36

  padding:
    x: 6
    y: 6

  opacity: 0.8
    
scrolling:
  history: 100000
  multiplier: 3
  autoscroll: true


font:
  normal:
    family: BlexMono Nerd Font Mono
    style: Regular
  
  bold:
    family: BlexMono Nerd Font Mono
    style: Bold
      
  italic:
    family: BlexMono Nerd Font Mono
    style: Italic
      
  bold_italic:
    family: BlexMono Nerd Font Mono
    style: Bold Italic

  size: 12.0

  # offset:
  #   x: 1
  #   y: 1

  draw_bold_text_with_bright_colors: true

key_bindings:
  - { key: F11, action: ToggleFullscreen }
```

### tmux

install tpm

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

in `~/.tmux.conf`

```ini
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'wfxr/tmux-power'

set-option -g default-shell /usr/bin/fish

# kill sessions automatically
set-option -g destroy-unattached

set -g @tmux_power_theme 'moon'

# send prefix
set-option -g prefix C-a
unbind-key C-a
bind-key C-a send-prefix
 
# use Alt-arrow keys to switch panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
 
# Shift arrow to switch windows
bind -n S-Left previous-window
bind -n S-Right next-window
 
# mouse mode
setw -g mouse on
 
# set easier window split keys
bind-key v split-window -h
bind-key h split-window -v
 
# easy config reload
bind-key r source-file ~/.tmux.conf \; display-message "~/.tmux.conf reloaded."

run '~/.tmux/plugins/tpm/tpm'

```

reload `.tmux.conf`
and press `prefix` + `I`(Capital i) to install plugins

kill all tmux windows

```bash
tmux kill-server
```

enter vi mode

`prefix` + `[`

quit vi mode

`q`

## Enable gtk Theme

```bash
yay -S lxappearance
```

## i3 config

### Map Caps Lock to Ctrl

```ini
# map caps lock to ctrl
exec --no-startup-id setxkbmap -option ctrl:nocaps
```

### ibus

```ini
# ibus-chewing
exec --no-startup-id ibus-daemon -drxR
```

### i3 Adjust Brightness

```ini
# adjust brightness
bindsym Mod1+backslash exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 1
bindcode Mod1+35 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.9
bindsym Ctrl+Shift+8 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.8
bindsym Ctrl+Shift+7 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.7
bindsym Ctrl+Shift+6 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.6
bindsym Ctrl+Shift+5 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.5
bindsym Ctrl+Shift+4 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.4
bindsym Ctrl+Shift+3 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.3
bindsym Ctrl+Shift+2 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.2
bindsym Ctrl+Shift+1 exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.1
bindsym Mod1+comma exec xrandr --output $(xrandr --query | grep " connected" | cut -d" " -f1) --brightness 0.0
```


## Polybar Requisites

```bash
yay -S rxvt-unicode
yay -S alsa-utils
yay -S mate-power-manager
sudo pacman -S git nitrogen rofi python-pip ttf-font-awesome adobe-source-code-pro-fonts binutils gcc make pkg-config fakeroot
yay -S ttf-font-awesome-5
```

## Polybar wlan and eth

### wlan

```bash
ip link
```

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 9c:5c:8e:01:b2:d0 brd ff:ff:ff:ff:ff:ff
3: wlp2s0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 1e:58:da:19:71:42 brd ff:ff:ff:ff:ff:ff permaddr 7c:b0:c2:63:08:12
```

choose enp3s0

### eth

install iwd for iwctl
```bash
yay -S iwd
```

start iwd
```bash
sudo systemctl start iwd
```

```bash
iwctl
```

```bash
device list
```

you will see:
```bash
[iwd]# device list
                                    Devices                                   *
--------------------------------------------------------------------------------
  Name                  Address               Powered     Adapter     Mode
--------------------------------------------------------------------------------
  wlan0                 3a:68:8f:72:6d:df     off          phy0       station
```

if it is powered off

```bash
[iwd]# adapter phy0 set-property Powered on
```

and run device list again

```bash
[iwd]# device list
                                    Devices                                   *
--------------------------------------------------------------------------------
  Name                  Address               Powered     Adapter     Mode
--------------------------------------------------------------------------------
  wlan0                 3a:68:8f:72:6d:df     on          phy0        station
```

```bash
[iwd]# station wlan0 scan
```

```bash
[iwd]# station wlan0 get-networks
```

you will see network lists

```bash
[iwd]# station wlan0 get-networks
                               Available networks
--------------------------------------------------------------------------------
      Network name                      Security            Signal
--------------------------------------------------------------------------------
  >   IAN-2                             psk                 ****
      js-3F                             psk                 ****
      Media Control System              psk                 ****
      4FWDC20                           psk                 ****
      HP-Print-85-LaserJet 1102         open                ****
```

choose the network

```bash
[iwd]# station wlan0 connect IAN-2
```
that's done

next, install wireless_tools for iwconfig

```bash
yay -S wireless_tools
```

```bash
iwconfig
```

```bash
lo        no wireless extensions.

enp3s0    no wireless extensions.

wlan0     IEEE 802.11  ESSID:"IAN-2"
          Mode:Managed  Frequency:2.412 GHz  Access Point: 6C:19:8F:E1:65:44
          Bit Rate=1 Mb/s   Tx-Power=22 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          Link Quality=56/70  Signal level=-54 dBm
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:1   Missed beacon:0
```

choose wlan0

if you want to close the wifi:

```bash
[iwd]# adapter phy0 set-property Powered off
```

## Disk Usage Analyzer

```bash
yay -S gdu
```

## Virt-manager

```bash
yay -S acpid
```

## Thunar

generate user directories

```
sudo pacman -S xdg-user-dirs
xdg-user-dirs-update
```

generate trash directory

```
yay -S gvfs
```

thumbnails

```
sudo pacman -S tumbler
```

## Vmware Ubuntu 

### Shared Folders

```bash
sudo vmhgfs-fuse .host:/ /mnt/hgfs/ -o allow_other -o uid=1000
```

### Opengl + glfw

```bash
sudo apt install cmake meson libglfw3-dev pkg-config

sudo dpkg-reconfigure bash
```

### 3D Acceleration

in `~/.vmware/preference`, add

```ini
mks.gl.allowBlacklistedDrivers = "TRUE"
```

## qt5ct

in `/etc/environment`

```ini
QT_QPA_PLATFORMTHEME=qt5ct
```

## Convert Video Format

```bash
ffmpeg -i test.webm -c copy test.mp4
```

## yt-dlp

download only audio

```bash
yt-dlp --extract-audio 'https://www.youtube.com/watch?v='
```

it will select opus format (default)

if you want to select differnt format, use `--audio-format`

download only mp3

```
yt-dlp --extract-audio --audio-format mp3 'https://www.youtube.com/watch?v='
```

## Repeat Key Speed

in `~/.xinitrc`

xset r rate [delay] [rate]

```ini
xset r rate 200 20
```

## Authentication Agent

run applications that need root permission, install lxsession

https://wiki.archlinux.org/title/Polkit

```bash
sudo pacman -S lxsession
```

