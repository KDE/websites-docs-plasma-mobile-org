.. raw:: mediawiki

   {{Warning|This page is obsolete, use [https://www.plasma-mobile.org/neon-arch-reference-rootfs/ plasma-mobile ] web site instructions for flashing}}

Flashing
========

For an ongoing updated guide, please follow these instructions:

-  https://plasma-mobile.org/nexus-5/

Prerequisites
-------------

The device must be connected to the Linux host with the USB cable all
the time, unless told to reconnect. The device must also be unlocked and
prepared to be flashed, see `this
page <../FlashingKubuntuPhonePreparation>`__ for details.

Configure the host device
-------------------------

Install Android tools.

On Ubuntu do:

``sudo apt-get install android-tools* phablet-tools``

Connect the device and type:

``lsusb``

an output like this will come out:

``Bus 004 Device 010: ID 18d1:4ee1 Google Inc. Nexus 4 / 10``

18d1 is the vendor id, 4ee1 is the product id.

Numbers may change with other devices.

Create /etc/udev/rules.d/51-android.rules with:

| ``SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666"``
| ``SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_adb"``
| ``SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_fastboot"``

replace vendor and product identifiers with yours.

Now Reload uev's rules to make these changes effective:

``sudo udevadm control --reload-rules``

Reconnect your device.

Type:

``sudo adb devices``

your device will be listed.

Unlock the device
-----------------

Go to the bootloader:

-  Power off the phone
-  Keep volume down and power pressed together until the bootloader is
   shown (the bootloader has an android with the lid open like this:

http://www.androidcentral.com/sites/androidcentral.com/files/postimages/684/android-az-bootloader.jpg)

From the Linux host type:

``sudo fastboot oem unlock``

Select "Yes" on the phone using volume keys, confirm pressing the power
key once.

Now reboot:

``sudo fastboot reboot``

Since unlocking wipes data, Android will restart the first time wizard.
Let the wizard come up, then press Power button to just power off the
phone, you will not need to complete the wizard.

Flashing Plasma Mobile
----------------------

For an ongoing updated guide for all flashing options, please go here:

-  https://plasma-mobile.org/nexus-5/

Basic Device Setup after flashing is done
-----------------------------------------

After installation, enable ssh:

Make sure the device is connected through your USB port, then log in to
it using

``sudo adb shell``

Optionally you can make this shell have bigger 'inner' size by running

``stty rows 40 cols 160``

This is not needed but will make the line not break when entering long
commands. Adjust the rows and cols according to your screen/settings.

Writable root
~~~~~~~~~~~~~

By default, the root filesystem is read-only. Not too useful for
developers. A script is run on first boot to make it writable and makes
a stamp file \```plasma-phone-devel-setup-run```, check this file exists
then run

``sudo reboot ``

and it will boot up with writable root.

The verbose version (so you know what's going on):

| ``sudo touch /userdata/.writable_image``
| ``sudo touch /userdata/.adb_onlock``
| ``sudo reboot``

Resizing the root parition
~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to have enough space available for installing stuff, you should
resize the root partition using

``sudo resize-root-partition``

This resized your root partition to about 6GB

Enable SSH access
~~~~~~~~~~~~~~~~~

After reboot above for writeable root it should then run sshd, it will
make the stamp file \```plasma-phone-devel-setup2-run```.

If not run it manually:

``sudo ssh-setup``

The verbose version (so you know what's going on):

| ``sudo bash``
| ``echo manual > /etc/init/ssh.override``
| ``echo "exec /usr/sbin/sshd -D -o PasswordAuthentication=yes" >> /etc/init/ssh.override``
| ``sudo service ssh start``
| ``sudo setprop persist.service.ssh true``
| ``sudo reboot``

Connect Wifi
~~~~~~~~~~~~

We've included a small script which sets up a wifi connection (WPA-PSK)
for NetworkManager. This can be run once writeable root is set up (as
above).

``wifi-setup SSID PASSWORD``

If you're using a different security mechanism for your wifi network,
it's time to read the nmcli documentation. Look into /usr/bin/wifi-setup
for inspiration.

On and on...
------------

Now follow the instructions for your `Development
Setup <Plasma/Mobile/DevelopmentSetup>`__ to get going.
