# ASUS Zenbook 13 Hackintosh
This is a pre-built OpenCore Hackintosh EFI for ASUS Zenbook 13 laptops. This has been tested on the BX325J (aka UX325J) model, but also should work with others, the closer your model is to mine, the better. My CPU is Intel i5 Ice Lake. If you want more info about my computer, just search it up, I don't think other info is that important.

Find out your audio driver codec before installing, it should be something like ALC*** with Realtek and CX**** with Conexant. Search up a tutorial on how for your OS online (you'll see why in step 7).
# What's not working
Sleep: You'll have to disable it by running `sudo pmset disablesleep 1` in the Terminal app. Keep in mind that the screen can still turn off after a few minutes or after you close the lid.

Touchpad Numpad: No fix avalible.

Managing keyboard backlight: You have to turn it on to your desired intensity in Linux or Windows and then reboot to macOS. At least it turns off when you close the lid, and then turns back on (probably because hardware manages that).

Bluetooth is a bit buggy: If you try to connect to a Bluetooth device and it immidiatly dissconnects, type `sudo purge` in the terminal, unpair the device, and pair it again.


If you are able to fix any of these or find a non-listed issue, please create a fork, edit the nessesarry stuff and file a pull request.
# What's working
Everything not listed in the previus section.
# Prerequisites
If you use Windows, make sure BitLocker is turned off.
Disable secure boot: Go into the UEFI setup by rebooting your computer and holding F2 (if it just boots into the OS, you can try other keys on the top of your keyboard). After you see the cool UEFI screen (it should look like mine if you have an ASUS), press F7 to enter advanced mode, go to the Security tab, then into "Secure boot" and change "Secure boot control" to Disabled. Now press F10 and then Ok to save your changes and reboot.
# How to install
1. Download the latest EFI from the releases page. Then, go to the [the installer website][installer], and download the same version you downloaded from releases. I recommend downloading the torrent as it is fast, but you will need a torrent client. You will now be redirected to a site called linkvertise, turn off your adblocker here, and click on free access with ads. Click on "I'm interested" then on "Explore web page" and on the webpage wait a few seconds, then close the tab and go back to linkvertise. Now click on the tiny "I already completed this step" button, and now you have access to the download.
2. After you've downloaded them, prepare a USB that is at least 16 GBs in size, and attach it to the computer. In this step everything on the USB will be erased, so back them up before proceeding. Now download and install BalenaEtcher and and inside it, select the .raw installer you just downloaded, then select the USB device, and click Flash. This will take some time.
3. Now prepare the partitions. Use something like GParted on a live Linux USB, or other partition software on Windows. If you want to dual-boot (which I recommend, I love both Linux and macOS), also give space to the future macOS (I think you'll need minimum 40 GBs), it should be a filesystem that macOS allows to erase (FAT32, NTFS or HFS+) because we will format it as APFS later. Also give some space (like 200 MBs) to a new EFI partition formatted as FAT32, and copy my EFI folder there. Make sure to unzip it, and that the EFI folder inside the partition contains the Boot and OC folders, not another EFI. If it does, rename the EFI/EFI/ folder to something else, and move it onto the root of the USB drive, delete the EFI folder you moved it out from, and name it back to EFI.
4. Now, reboot your computer, and press the UEFI key (for me, it's F2, but if it doesn't work, try other Fn keys, Escape or delete). Press F8 to select the boot device, and select your EFI partition (not the USB!), most likely called UEFI OS, and when you see the options, select "Install macOS XXX" with an orange Apple. It can take a minute to boot, don't worry.
5. After it loaded, select the language, click Continue, and double-click Disk Utility, click on this button: ![the button](https://github.com/Octopus1348/ASUS-Zenbook-13-Hackintosh/assets/105970916/47c4620a-775d-4049-8299-03e1b8703908), and select "Show all devices", now if you wanted to dual-boot, select the macOS partition (NOT THE EFI) it's mostly identifiable by the size, click Erase and name it something like macOS, then proceed with the deletion. If you didn't want to dual-boot, do the same, just with your previus OS partition. After it finished, click Done, and the red window button to go back.
6. Now double-click the Install macOS XXX button, continue, accept the agreement, in which they specifically tell you only to install on a Mac, and choose the partition you just named macOS (or any other name) and continue. This will take some time, so grab a cup of c̶o̶f̶f̶e̶e̶ water while you wait. The computer will turn off miltiple times while installing, but that is normal. If it turns off, you can pull out the USB drive (DON'T PULL IT OUT IF ONLY THE SCRREN TURNED OFF!), turn the computer back on, and the installation should automatically continue. These turn-offs can also happen during a macOS update if I remember correctly.
7. After it installed and booted, go trough the setup, and check if audio (and microphone) is working. If it is, skip step 8.
8. If not, go to the [AppleALC supported codecs][applealc] page, search with Cmd+F (Cmd is the Windows key) to find your audio driver codec, and save the possible layouts onto your desktop (a cool macOS feature, select and hold the text for a while and drag it onto your desktop to save it). Install OpenCore Configurator. Now in Finder, go into your EFI partition, if you didn't name it, it's probably called NO NAME, and go into EFI, OC, and open config.plist using OpenCore Configurator. Now go to the NVRAM tab, select `7C436110-AB2A-4BBB-A880-FE41995C9F82` and double-click the value of `boot-args`. Change the value of `alcid` to the number of one of the supported codecs you have in the file on your desktop, save, and reboot. Check if sound is working again, and if not, change the alcid to another number from the supported codecs, save and reboot, and repeat this until the audio is working.
9. Maybe even install a PC keyboard layout like [this Hungarian PC keyboard layout for Mac][hungarocell]. Search online to find more layouts. Tough I stopped using that layout, because I want to get used to where special characters are on Mac. (I will give this computer to my sister when I will have a Mac, so even then I can maintain this)
10. Enjoy!

# I get gray screen in recovery mode, what do I do?
If you get a gray screen in recovery, it also means that you may not see updates in settings, and for me, I couldn't even update from the full macOS installer because it said "A snapshot is currently set to boot that is not the currently booted snapshot.", so follow this easy way to fix that.

Reset your NVRAM from the boot entry "Reset NVRAM.efi". If it doesn't work, try resetting your NVRAM in your installer USB's "Reset NVRAM" boot entry.

# How to update to a new macOS version
If it's a small update, like from 14.0 to 14.1, it's as simple as going into the settings and updating it in the software update menu. If there is a macOS update, but Settings says "Your system is up to date.", refer to "I get gray screen in recovery mode, what do I do?".

However, if it is a bigger update like from 14.0 to 15.0, you can't do it his way, it won't let you download for some reason. Instead, you have to go to the App Store, search up the update by it's (code)name and download the full installer (it will redirect you to the settings) and then replace your EFI folder with a newer version after I've released it. Only start the install process after I've released a new EFI, or do it yourself if I take a long time (doing yourself has some risks though, so I recommend you make a Time Machine backup before doing that)! Just downloading before I haven't released a new one yet and quitting the installer is okay. After that, you can open the Install macOS xxx app, and follow the steps to install the update.

These steps might change as I haven't gotten to test it on an actual update except for Ventura and it didn't work this way, but I may have just messed up something else. I will test it again once macOS 15 comes out.
# Credits
[devboloji][devboloji] for the base EFI

[Olarila][installer] for the installer

[OpenIntelWireless][intelwireless] for Wi-Fi and Bluetooth kexts

[acidanthera][acidanthera] for a lot of kexts!

[KingOfFright][kingoffright] for it's ACPIBatteryManager (battery status) kext fork. An earlier fork is made by [RehabMan][rehabman] and the original is by [gsly][gsly].

Apple for macOS

...And many others!

If you like the project, star this and [Infinix-Hackintosh-Opencore-Guide][devboloji]. Also consider donating to [Olarila][installer]!


[devboloji]: https://github.com/devboloji/Infinix-Hackintosh-Opencore-Guide
[installer]: https://www.olarila.com/topic/6278-olarila-vanilla-images-macos-installer/
[applealc]: https://github.com/acidanthera/AppleALC/wiki/Supported-codecs
[intelwireless]: https://github.com/OpenIntelWireless/
[acidanthera]: https://github.com/acidanthera
[kingoffright]: https://github.com/KingOfFright/OS-X-ACPI-Battery-Driver
[rehabman]: https://github.com/RehabMan/OS-X-ACPI-Battery-Driver
[gsly]: https://github.com/gsly/OS-X-ACPI-Battery-Driver
[hungarocell]: https://github.com/kodfodrasz/macos-hungarian-pc-keyboard-layout
