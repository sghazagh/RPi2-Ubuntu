# RPi2-Ubuntu
###############
This script is based on Pi2buntu script available on "https://github.com/FuturePilot/Pi2buntu" for creating a minimal Ubuntu image for the Raspberry Pi 2. This script only supports the Raspberry Pi 2 as it uses the Ubuntu armhf port which is optimized for armv7.
This version has been optimized the SDCard size and added few more applications.

If you just want an image and don't want to mess around with creating your own, see the [release page](https://github.com/sghazagh/RPi2-Ubuntu/releases).

**Default User:** pi

**Default Password:** Raspberry

**Note:** This image is intended for advanced users. It has no GUI. If you are not comfortable with the command line this image is not for you.

###Overview
**What does this thing do?**

This script utilizes debootstrap to create a minimal Ubuntu chroot using Ubuntu's armhf packages. It also uses qemu-debootstrap as this script is intended to be run on the x86(_64) architecture and that is the easiest way to set up a foreign architecture chroot. It then adds some custom tweaks and configs specific for the Raspberry Pi 2. Finally it creates a disk image that you can then put on an SD card.

**Kernel**

This setup uses the official Raspberry Pi kernel and not Ubuntu's kernel. These updates are managed through a [Raspberry Pi 2 PPA](https://launchpad.net/~fo0bar/+archive/ubuntu/rpi2) (thanks Ryan Finnie)

**Custom Tweaks**

There are a few custom tweaks that have been included. 

 - The sound module `snd_bcm2835` is loaded by default.
 - `snd_soc_pcm512x_i2c` `snd_soc_pcm512x` `snd_soc_tas5713` `snd_soc_wm8804` have been blacklisted as they are not applicable to the Raspberry Pi 2

**Things You May Notice Are Missing**

 - rpi-update
 As mentioned above the kernel and firmware are handled through the PPA. rpi-update is not needed
 - raspi-config
 There is no raspi-config so any of the things that it does will need to be done manually. However porting it over is something I would like to do eventually.

###Upgrading to new Ubuntu releases
It is possible to upgrade using the do-release-upgrade utility, however it requires some manual intervention.

After running sudo do-release-upgrade it will eventually warn you that third party sources have been disabled. DO NOT CONTINUE YET. Don't do anything. Leave that prompt up and open another terminal or screen window. Open /etc/apt/sources.list.d/fo0bar-ubuntu-rpi2-*.list with your favorite editor and remove the comment in front of the deb line. The distribution should already be changed to the version you are upgrading to, but if it's not, change that too. Save the file. Repeat for /etc/apt/sources.list.d/futurepilot-ubuntu-raspberry-pi-2-*.list as well.

Now go back to the do-release-upgrade window and continue with the upgrade. This will ensure that packages from the PPAs will be used over the ones from the Ubuntu repos.

It may be possible to do the upgrade without following these steps and just leave the PPAs disabled until after the upgrade but I did not care to find out if it would work or epic fail and decided to be safe than sorry.

###Special Thanks
Not all of this is my work so this is to give credit where credit is due.

 - wintrmute for the resize_rootfs.sh script
 - Ryan Finnie for the Raspberry Pi 2 PPA and for creating the original script this one is based on
 - [https://wiki.ubuntu.com/UbuntuDevelopment/Ports](https://wiki.ubuntu.com/UbuntuDevelopment/Ports) for the `qemu-arm-static` idea
