
I use  a minimal install of Debian as my distro.
Add disk using ventoy
Run the command line installer as I prefer it
Use expert mode to get more options to set up a minimal install
Use guided partitioning but then edit the partition table to set up a 16G swap space
- Image for partition table

Fairly standard: create users
Use phone hotspot as I can't connect to the institute WIFI with the inbuilt networkmanager

Then deselect EVERYTHING except for Debian - I'll manually install components later
Let it download and install

It installed Gnome for some reason, lol
add user to sudoers file
Modify sources.list to comment out the CDROM image as a source
Use apt to install openssh-server, nano
Use apt to uninstall gnome-* and libreoffice
Autoboot: Change autoboot in bios settings so it auto reboots on software package

Now I set up networking WPA enterprise using networkmanager
Reboot

Now i install official Docker package and then set up ngrok. I run ngrok port 22 through a screen
This lets me ssh from anywhere!

Disable internal keyboard its useless: GRUB_CMDLINE_LINUX_DEFAULT="quiet i8042.nokbd"

Installed nvidia-driver to remove error message
```
[28640.935630] nouveau 0000:01:00.0: bus: MMIO read of 00000000 FAULT at 6013d4 [ IBUS ]
```

Weirdly I had to add con