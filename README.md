# hackintosh
Installing MacOS Sierra on an Intel based custom machine

### Hardware
* Motherboard - Asus Z170-A
* Processor (CPU) - Intel i7-6700K
* Memory - Corsair Vengeance 16GB 3000MHz
* Graphics - Asus nVidia GeForce GTX 1070
* Disk 1 - Western Digital Black 500GB
* Disk 2 - Samsung 960 EVO 500GB SSD
* 8 GB Lexar USB 2.0 Thumb Drive
* Cooling - Corsair H80i <— Do not use the usb cable for Corsair Link on the CPU block
* I have the following setup for the cooling:
  * Water pump to the W_PUMP
  * Fan 1 Connected to CPU_FAN
  * Fan 2 Connected to CHA_FAN1

### Software
* macOS Sierra Installation
* Disk Utility
* Unibeast
* Multibeast
* Clover Configurator
* Patch-nvme-master
* Kext Utility
* audio_CloverALC
* Vidia Web Driver 378.05.05.xx
* CodecCommander.kext
### SSD Note
As macOS Sierra doesn’t recognize the Samsung 960 EVO SSD during installation the only workaround I have found is to install it on a regular drive and then using cloning software such as Carbon Copy Cloner to clone the HDD to SSD. You will need to patch the OS first and once the SSD appears as available drive you can clone it.
### Get Copy of MacOS Sierra
For this step you will need an existing Mac machine. You can have your rich friend download a copy from App Store on their Mac device for you or you can search the internet for another source of the MacOS installation files. This is not part of this tutorial but you can “google it”.
### Installation Prep
#### USB Installer Prep
* Use Disk Utility and erase USB thumb drive using Mac OS Extended (Journaled), Scheme GUID
* Use Unibeast
* Destination select USB drive
* OS select Sierra
* Bootloader select UEFI
* Ignore other options for now and click Install
* Your bootable USB drive with Clover UEFI and Sierra Installation is ready
* As the system may not have internet on initial boot up it is a good idea to download Multibeast and copy it onto the USB drive
#### Machine Prep
* Reboot machine and press and hold DEL button on the keyboard
* For Z170-A
* Disable VT-d
* Disable Secure Boot
* OS Type select Other OS
* Press F10, save settings and reboot
### OS Installation
* You can either have the system boot using the USB in the BIOS by going to boot and setting the USB drive as the first boot device or just press F8 on reboot to bring up the boot device menu
* At Clover boot screen select Boot OS install from USB
* Choose language
* You will come to the install screen but you need to prepare the drive first.
* In the menu bar click Utilities and select Disk Utility
* You will notice the SSD drive is not available yet. This is because the system has not been patched yet so just use regular hard drive for now and erase with Mac OS Extended (Journaled), Scheme GUID, I name my drive as macOS
* Once erase is done exit Disk Utility
* Use the installer and install to the macOS drive
* Once complete the machine will restart
* Press F8 during reboot to boot into USB
* Clover select Boot into macOS
* The installer will continue its installation and ask what features you need enabled such and location services and Siri
* The OS is setup but not bootable on it’s own. This is where multibeast comes in.

### Almost There
FIRST THINGS FIRST. DISABLE AUTOUPDATE in App Store. You don’t want the OS updating in the middle of your setup, rendering your configurations incompatible.
* With USB still plugged in use the multibeast copied earlier
* Quick start UEFI boot
* Drivers
* Audio
* ALC892
* Optional 3 Port (5.1)
* 100 Series Audio
* Network
* IntelMausiEthernet
* USB
* Increase Max Port Limit
* Bootloader
* Clover + NVRAM
* Graphics
* nVidia Web Driver Flag
* Build
* Make sure your drive is selected and click install
* Install nVidia driver and it will reboot the machine.
* At this time you can unplug your USB drive as your machine should now have full graphics acceleration and access to internet
* When machine reboots press DEL on keyboard and go to boot menu and select UEFI your hard drive
* Press F10 and save settings
* Your machine will reboot and directly go to clover with macOS pre-selected with a 3 second countdown.

### Home Stretch
#### Audio
* Open the audio_cloverALC-130 and in Terminal run the audio_cloverALC-130.sh command
* It will ask a few questions and you press y and y and select Audio ID 2
* On next reboot you will have sound through the analog audio. If that is not the case open up Sound from System preferences and select Internal Speakers - Built-in Output device
#### Audio Sleep Fix
The audio kept getting disabled when the system was put to sleep. To fix this issue you will need to install CodecCommander.kext
* Launch Kext Utility
* Drag and drop CodecCommander.kext
* Once done quit the app

#### SSD Install
* Boot into your macOS
* In the patch-nvme-master folder
* Run ./path_nvme.sh
* This will create a HackrNVMeFamily kext file in the same folder
* Remove the existing kext that comes with macOS
* Mount your EFI system
* Open Terminal
* diskutil list
* Get the identifier for the EFI drive
* mkdir /Volumes/EFI
* mount -t msdos /dev/disk0s1 /Volumes/EFI
* sudo mv  /Volumes/EFI/System/Library/Extensions/IONVMeFamily.kext ~/Documents
* Open Kext Utility
* Drag and drop the HackrNVMeFamily kext file from the patch folder
* Once done reboot the machine
* On bootup you will now have the SSD available in Disk Utility
* Use Carbon Copy Cloner to clone your HDD to you SSD and you should be good to go

### YAY
* I now have a fully functional hackintosh running on my 960 EVO SSD with macOS Sierra and really good system stability and all functional software.

## Oh I forgot to tell you its running on a 4K monitor. It is sweeeet!
#### Issues
* Corsair H80i causes sleep issues if you connect the usb to the motherboard as per the instructions of the manufacturer. This is mainly to use Corsair Link but in this case we don’t need it so leave it out or unplug it.
* I have disabled all kinds of updates
  * nVidia Web Driver Update
  * App Store Updates
