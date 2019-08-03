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
- When you reached *Destination Select*, select your **USB drive**.
- In *Installation Type*, click on _(Customize)_ and select:
  - Clover for UEFI booting only
  - Install Clover in the ESP
  - **UEFI Drivers** (Leave those that are already selected but make sure you have) :
    - ApfsDriverLoader
    - AptioMemoryFix
    - AudioDxe
    - DataHubDxe
    - FSInject
    - HFSPlus
    - SMCHelper
    - VBoxHfs
- Install and open [Clover Configurator](http://mackie100projects.altervista.org/download-clover-configurator/). Go to *Mount EFI* and find your USB EFI Partition and click *Mount Partition*.
- Go to the EFI disk (using Finder or Explorer) and navigate to **EFI -> CLOVER -> kexts -> Other**. Here copy all the *Kexts* from the clone of this repository
- Go back to **EFI -> CLOVER** and paste the *config.plist* from the clone of this repository. Make sur to rename the existing one in *config.plist.old*!

And there you go, you are done with this step! Let's move on to the BIOS setup.


## BIOS Setup
Access the BIOS of your Elitebook using the *F10* key and go to the **Advanced** tab.
Change those settings:
- **Device Configuration**:
  - Uncheck the *Fn key switch*
  - Change *Video Memory* to **128MB**
  - Disable *Wake On USB*
  - Disable *Virtualization Technology (VT-x)*
  - Disable *VT-d*, if you have it
  - Set *SATA mode* to **AHCI**
- **Built-in Device Option**:
  - Disable *Wake On LAN*


Save and reboot on your USB Installer
