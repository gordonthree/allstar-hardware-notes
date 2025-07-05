# allstar-hardware-notes
My notes on preparing old hardware to run Allstar (and other things) reliably.

A few days ago I worked on an Allstar node on a Dell Optiplex 3050 USFF (Dell Micro) a friend had dropped off that would become unresponsive after being left on for a while. I didn't find anything wrong with the hardware directly, but did make a few updates that seemed to improve reliability.

Firstly, I edited grub adding these arguments to the kernel command line:
``pci=noaer pcie_aspm=off``

The first disables a form of error handling for the pci bus, something that is "broken" on a lot of older hardware, and the second disables pci express power management, where the kernel tries to turn off unused pci express devices. 

To make this fix, edit the default kernel command line and then update grub. 
``sudo nano /etc/default/grub``

edit the command line, look for this line:
``GRUB_CMDLINE_LINUX_DEFAULT="quiet"``
and change it to
``GRUB_CMDLINE_LINUX_DEFAULT="quiet pci=noaer pcie_aspm=off"``

Now, update the grub bootloader configuration and reboot

```
sudo update-grub
sudo reboot
```

Secondly, I installed a bios update on the machine.

```
sudo apt install fwupd

# update hardware inventory
sudo fwupdmgr get-devices

# update inventory of updates
sudo fwupdmgr refresh

# download updates for the hardware
sudo fwupdmgr get-updates

# install updates
sudo fwupdmgr update

# reboot computer (most firmware updates get installed before the OS boots, don't walk away yet)
sudo reboot
```
