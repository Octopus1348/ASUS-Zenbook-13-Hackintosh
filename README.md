# ASUS Zenbook 13 Hackintosh
This is a pre-built OpenCore Hackintosh EFI for Zenbook 13 laptops. This has been tested on the BX325J model.

Find out your audio driver codec before installing, it should be something like ALC*** with Realtek and CX**** with Conexant. Search up a tutorial on how for your OS online (you'll see why in step 7).
# What's not working
Sleep. You'll have to disable it by running "sudo pmset disablesleep 1" in the Terminal app.

Detecting that the laptop is charging. It will still show the battery level, and that it's draining, just if you plug it in, it wont show, and if you charged over your % before charging, it won't show higher % until you reboot.

Bluetooth is a bit buggy. If you try to connect to a Bluetooth device, and see that Bluetooth just turned off, you can't turn it back on until a reboot. But after that the device will be connected.
# What's working
Everything not listed in the previus section
# How to install
1. First go to [the installer website][installer], and select the latest version I support (see in the releases tab). I recommend downloading the torrent as it is fast. It will redirect you to a site called linkvertise, turn off your adblocker here, and click on free access with ads. Click on "I'm interested" then on "Explore web page" and on the webpage wait a few seconds, then close the tab and go back to linkvertise. Now click on the tiny "I already completed this step" button, and now you have access to the download.
2. After you've downloaded it, prepare a USB that is at least 16 GBs in size, and attach it to the computer. In this step everything on the USB will be erased, so back them up before proceeding. Now download and install BalenaEtcher and and inside it, select the .raw file you just downloaded, then select the USB device, and click Flash. This will take some time.
3. Now prepare the partitions. Use something like GParted on a live Linux USB, or other partition software on Windows. If you want to dual-boot, also give space to the future macOS (I think you'll need minimum 40 GBs), it can be any filesystem because we will format it as APFS later anyway. Also give some space (like 200 MBs) to the a new EFI partition formatted as FAT32, and copy my EFI folder there. Make sure to unzip it, and that it contains the Boot and OC folders, not another EFI, if it does, use the EFI folder inside the EFI folder.
4. Now, reboot your computer, and press the UEFI key (for me, it's F2, but if it doesn't work, try other Fn keys or Escape). Press F8 to select the boot device, and select your EFI partition (not the USB!), most likely called UEFI OS, and when you see the options, select "Install macOS XXX" with an orange Apple. It can take a minute to boot, don't worry.
5. After it loaded, select the language, click Continue, and double-click Disk Manager, click on this button: ![the button](https://github.com/Octopus1348/ASUS-Zenbook-13-Hackintosh/assets/105970916/47c4620a-775d-4049-8299-03e1b8703908), and select "Show all devices", now if you wanted to dual-boot, select the macOS partition (NOT THE EFI) it's mostly identifiable by the size, click erase, and name it something like macOS, then proceed with the deletion. If you didn't want to dual-boot, do the same, just with your previus OS partition Now, click Done, and on the red window button to go back.
6. Now double-click the Install macOS XXX button, continue, accept the agreement in which they specifically tell you only to install on a Mac, and choose the partition you just named macOS (or any other name) and continue. This will take some time, so grab a cup of water while you wait.
7. After it installed and booted go trough the setup, and check if audio (and microphone) is working.
8. If not, go to the [AppleALC supported codecs][applealc] page, search with Cmd+F (Cmd is the Windows key) to find your audio driver codec, and save the possible layouts onto your desktop (a cool macOS feature, select and hold the text for a while and drag it onto your desktop to save it). Now in Finder, go into your EFI partition, if you didn't name it, it's probably called NO NAME, and go into EFI, OC, and open config.plist. Now use Cmd+F to search for "alcid=" without quotes, and change the number to one of the supported codecs you have in the file on your desktop, save, and reboot. Check if sound is working again, and if not, change the alcid to another number from the supported codecs, save and reboot, and repeat this until the audio is working.
9. Enjoy!

# Credits
[devboloji][baseefi] for the base EFI

[Olarila][installer] for the installer


[baseefi]: https://github.com/devboloji/Infinix-Hackintosh-Opencore-Guide
[installer]: https://www.olarila.com/topic/6278-olarila-vanilla-images-macos-installer/
[applealc]: https://github.com/acidanthera/AppleALC/wiki/Supported-codecs
