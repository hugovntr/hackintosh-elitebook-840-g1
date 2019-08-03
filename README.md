# Hackintosh HP Elitebook 840 G1
This guide was made to help you install **macOS Mojave 10.14.5** on your Elitebook 840 G1.
If you want to install *10.14.1*, I highly suggest you go over [blint01's guide](https://github.com/blint01/hackintosh-mojave-HP-840-G1). In fact, this guide is highly inspired by the one from *blint01* but unfortunatly his doesn't work properly with Mojave 10.14.5 (*At the time of this writing, it's the only version of Mojave that you can get from the App Store*)


## Prerequisites
- USB flash drive (8Gb+) formatted to _MacOS Extended (Journaled)_ and _GUID Partition Map_
- macOS Mojave 10.14.5 (Downloaded from the official App Store)
- [Clover EFI (Latest version)](https://github.com/Dids/clover-builder)
- [Clover Configurator](http://mackie100projects.altervista.org/download-clover-configurator/)
- BCM943224HMS WiFi Half Mini PCI card. The standard WiFi card is not supported by Apple unfortunatly.
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


## Install
Spamming *F9* key at start-up and select *External Device*

You should normally see the **Clover** menu appear on your screen with *Boot macOS Install from Install macOS Mojave*, if that's the case then press *Enter* and prepare yourself because from now on you are going to spend a lot of time doing absolutely nothing but watching it run...

**Don't panic if it takes ages to boot, as long as it doesn't restart unexpectedly, wait!**

After around 10-15minutes you should see the *OS X Utilities/macOS Utilities* menu.
- Select the *Disk Utility*
- In the *View* menu select *(Show all Device)* then select your drive, not the partition but the actual drive you want to install macOS Mojave on!
- Click *Erase* and set a *Name*, format in **APFS** and Scheme to **GUID Partition Map**. When it's done, close the *Disk Utility*
- Click on *Install macOS*, accept everything and install it to your freshly wipped drive!
  - **During this step the computer should restart**(that's why you have to pay attention). After the first restart you should see 2 or more drives on the *Clover* menu. Select the drive with **the name that you set 2 steps before** and let it run!
  - If a second restart occur, do the exact same thing
- After a loooooong wait, macOS should finally greet you with the standard installation questions like your Country, Username, ...

**Make sure to do all this steps at once**

You have now a working Hackintosh!.. But it still can not boot without the USB :)


## Post Install
So, this is the final steps.
In this one, we are going to fix every problem that our current installation has. Like not starting without the USB for example.

First you need to be connected to your router with an *Ethernet cable* to downloads the tools that we are going to need.
Once that's done, we can move on:
- Install [Clover EFI (Latest version)](https://github.com/Dids/clover-builder) but this time on your *drive* and not the USB
- Install Xcode "toolkit"
```
xcode-select --install
```
- We are going to clone *RehabMan's Probook repo*
```
mkdir ~/Downloads/Projects
cd ~/Downloads/Projects
git clone https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch probook.git
```
- Next, download and install the *Kexts*
```cd ~/Downloads/Projects/probook.git
./download.sh
./install-downloads.sh
```
- Here we are going to disable the hibernation mode. (**Do it unless you want to redo that all over again in 15 minutes**)
```
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```
- Now let's build stuff shall we ?
```
cd  ~/Downloads/Projects/probook.git
./build.sh
```
- Mount the EFI
```
./mount_efi.sh
```
- Install the SSDT patches
```
./install_acpi.sh install_8x0g1_haswell
```
- And finaly, install the *config.plist*
```
cp ./config/config_8x0_G1_Haswell.plist /Volumes/EFI/EFI/Clover/config.plist
```

Make a copy of that *config.plist* and open it in [Clover Configurator](http://mackie100projects.altervista.org/download-clover-configurator/)*(on your brand-new mac)*. Go to **SMBIOS** and Generate a new *Serial Number*.
Save that *config.plist* and put it back to your **EFI** Volume

Last step, if like me and *blint01* your sound card wasn't recongnized. Go to **EFI -> EFI -> CLOVER -> ACPI -> PATCHED** and open the **SSDT-8x0G1h.aml** file with *MaciASL*. Find *layout* (using Cmd+F).You should find 2 matches. The default value is `0x0D`, that means layout 13. Switch both `0x0D` after layout to either of these:
- Layout 3 = 0x03
- Layout 12 = 0x0C
- Layout 33 = 0x21
- Layout 84 = 0x54

For me the layout 84 worked perfectly! After you made the changes to both value, save it and reboot your computer, to see if it worked or not.


And there you go! You now have a perfectly working Hackintosh on macOS 10.14.5!
Thanks again to *blint01*, *RehabMan*, and everyone else in the /r/Hackintosh subreddit!

If you have any question you can contact me here or on Twitter (Can be found in my GitHub profil page)
