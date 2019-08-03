# Hackintosh HP Elitebook 840 G1
This guide was made to help you install **macOS Mojave 10.14.5** on your Elitebook 840 G1.
If you want to install *10.14.1*, I highly suggest you go over [blint01's guide](https://github.com/blint01/hackintosh-mojave-HP-840-G1). In fact, this guide is highly inspired by the one from *blint01* but unfortunatly his doesn't work properly with Mojave 10.14.5 (*At the time of this writing, it's the only version of Mojave that you can get from the App Store*)


## Prerequisites
- USB flash drive (8Gb+) formatted to _MacOS Extended (Journaled)_ and _GUID Partition Map_
- macOS Mojave 10.14.5 (Downloaded from the official App Store)
- [Clover EFI (Latest version)](https://github.com/Dids/clover-builder)
- [Clover Configurator](http://mackie100projects.altervista.org/download-clover-configurator/)
- A clone of this repository somewhere safe
- 2 hours, at least


## USB Installer
To install Mojave you will need to create a bootable USB with the image that you have downloaded from the App Store. Here is the steps you want to follow:
- [Follow this guide](https://www.imore.com/how-create-bootable-installer-mac-operating-system)
- Install [Clover EFI](https://github.com/Dids/clover-builder).
- When you reached *Installation Type*, click on _(Change Install Location)_ and select the USB.
- In *Installation Type*, click on _(Customize)_ and select:
  - Install for UEFI booting only
  - Install Clover in the ESP
  - **Drivers** :
    - Leave those that are already selected
    - VBoxHFS
    - ApfsDriverLoader
