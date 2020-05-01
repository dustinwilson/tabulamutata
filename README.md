# Tabula Mutata #

Tabula Mutata is a command line tool written in bash for managing configurations for a Wacom tablet in Linux using the xf86-input-wacom drivers.

## Why? ##

Since switching to Linux as my primary operating system I have been trying to duplicate the tablet configuration I have been used to using the official Wacom drivers on macOS. I have tried many desktop environments in Linux, and I have settled upon KDE currently. Cinnamon's tablet configurator could replicate my configuration. KDE's tablet configurator software is widely praised, but I find it lacking and its interface unnecessarily confusing.

The main feature I use concerning the buttons on the tablet is the button in the center of the touch wheel which in Wacom's official driver is strictly used to toggle between four different configurations for the touch wheel itself, denoted by a small LED. This isn't the default on KDE, and there isn't a way to configure it. So, if I wanted to have this feature I needed to make it happen myself. Tabula Mutata is the result of my efforts.

This is strictly designed to scratch my own itch, so right now it is configured for my own Wacom Intuos 4 XL tablet with my preferred configuration. I am open to expanding it in the future to be more generic and configurable without editing the executable if others are interested.

## Installation ##

Tabula Mutata has the following dependencies:

* docopts
* systemd
* xbindkeys
* X11

Tabula Mutata is made up of 4 parts:

1. Executable: `tabulamutata`  
   `tabulamutata` should be installed at `/usr/local/bin/tabulamutata`
2. Udev rule: `tabulamutata.rules`  
   `tabulamutata.rules` should be installed at `/etc/udev/rules.d/tabulamutata.rules`
3. Systemd unit file: `tabulamutata.service`  
   `tabulamutata.service` should be installed at `/etc/systemd/system/.xbindkeysrc`
4. Xbindkeys configuration file: `xbindkeysrc`  
   `xbindkeysrc` should be installed at `~/.xbindkeysrc`

Following installation of the files enable and run the systemd unit file to get rolling:

```bash
systemctl enable tabulamutata.service
systemctl start tabulamutata.service
```

## Documentation ##

The executable (`tabulamutata`) has three subcommands:

1. `init`  
   This is run by the systemd unit file and sets up the tablet.
2. `set-permissions`  
   This is a command meant to be run as root that sets the permissions of the LED folders. This usually shouldn't need to be run unless the tablet wasn't initialized properly.
3. `toggle-touch-wheel`  
   This command is run by xbindkeys where it is bound to the button in the middle of the touch wheel, causing the configuration for the touch wheel to change

The udev rule `tabulamutata.rules` is used to run the systemd unit file (`tabulamutata.service`) to configure the tablet when the tablet is plugged in or when the computer is booted up.