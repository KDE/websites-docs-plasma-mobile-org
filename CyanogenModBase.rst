Introduction
------------

Plasma Mobile is free mobile platform and it is capable of running on
any operating system base. Initial reference images were based on Ubuntu
Touch. This document explains how to install the Plasma Mobile with
CyanogenMod base on Nexus 5.

How to install
--------------

Use flashtool
~~~~~~~~~~~~~

There is handy script which can be used to flash plasma mobile easily

-  First put your device in fastboot mode and then,

| ``     git clone ``\ ```https://github.com/plasma-phone-packaging/pm-flashtool.git`` <https://github.com/plasma-phone-packaging/pm-flashtool.git>`__
| ``     cd pm-flashtool``
| ``     ./flash-plasma-phone``

-  If you don't want to re-download everything while re-flashing use, -c
   option of script.

``    ./flash-plasma-phone -c``

It will use cache from the cache subdir.

MultiROM instructions (For dualboot)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

(This guide assumes you already have MultiROM and multirom based
recovery installed)

-  First download the stripped down Cyanogenmod version from
   http://neon.plasma-mobile.org/cm-stripped.zip and install it as new
   ROM from MultiROM recovery.

-  Once installed, boot into newly installed Cyanogenmod ROM (would be
   named as package most likely), it will show just the Google logo at
   start.

-  After that use the flash tool to install plasma-phone.

| ``    git clone ``\ ```https://github.com/plasma-phone-packaging/pm-flashtool.git`` <https://github.com/plasma-phone-packaging/pm-flashtool.git>`__
| ``    cd pm-flashtool``
| ``    ./flash-multirom``

-  If you don't want to re-download everything while re-flashing use, -c
   option of script.

``    ./flash-multirom -c``

It will use cache from the cache subdir.

-  Once flash script is finished, it will reboot your device, at OS
   selection screen select the previously installed Cyanogenmod ROM, it
   will boot into Plasma phone.

Manual (Useful for enabling device and trying out porting)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Following guide assumes that your Nexus 5 device has an unlocked
bootloader.

-  Download and install the stripped-down CyanogenMod from the `zip
   file <http://neon.plasma-mobile.org/cm-stripped.zip>`__
-  Once installed, boot into cyanogenmod and restart adbd as root

``     adb root``

-  Once adbd is restarted as root, download and extract lxc for android.

| ``     wget ``\ ```https://jenkins.linuxcontainers.org/job/lxc-build-android/lastSuccessfulBuild/artifact/lxc-android.tar.gz`` <https://jenkins.linuxcontainers.org/job/lxc-build-android/lastSuccessfulBuild/artifact/lxc-android.tar.gz>`__
| ``     adb push lxc-android.tar.gz /tmp``
| ``     adb shell tar xvzf /tmp/lxc-android.tar.gz``

-  Create directory for the system container and extract rootfs into its
   location.

| ``     wget ``\ ```http://mobile.neon.pangea.pub:8080/job/img_phone_xenial_armhf/lastSuccessfulBuild/artifact/result/livecd`` <http://mobile.neon.pangea.pub:8080/job/img_phone_xenial_armhf/lastSuccessfulBuild/artifact/result/livecd>`__\ ``..rootfs.tar.gz``
| ``     adb push livecd..rootfs.tar.gz /tmp``
| ``     adb shell mkdir -p /data/lxc/containers/system/rootfs/``
| ``     adb shell tar xvzf /tmp/livecd..rootfs.tar.gz -C /data/lxc/containers/system/rootfs/``
| ``     wget ``\ ```https://raw.githubusercontent.com/plasma-phone-packaging/CM/master/lxc-system-config`` <https://raw.githubusercontent.com/plasma-phone-packaging/CM/master/lxc-system-config>`__
| ``     adb push lxc-system-config /data/lxc/containers/system/``

-  Start lxc container

| ``   adb shell``
| ``   root@hammerhead:/ # lxc-start -n system -F``
| ``   * Setting up X socket directories...``
| ``   ...done.``
| ``   Ubuntu Xenial Xerus (development branch) ubuntu-phablet console``
| ``   ubuntu-phablet login: phablet``
| ``   Last login: Wed May 12 08:39:00 UTC 1971 on lxc/console``
| ``   Welcome to Ubuntu Xenial Xerus (development branch) (GNU/Linux 3.4.0-cyanogenmod-g15e5a99-dirty armv7l)``
| ``  * Documentation:  ``\ ```https://help.ubuntu.com/`` <https://help.ubuntu.com/>`__
| ``  phablet@ubuntu-phablet:~$``

-  Start udev and lightdm

| ``   sudo service udev start``
| ``   sudo udevadm trigger --action=add``
| ``   sudo service lightdm start``

Tips and tricks
---------------

When you try to start container second time, it doesn't allow you to
login in. To work around this, From adb shell

| `` rm /data/lxc/containers/system/rootfs/etc/init/lxc-android-boot.conf``
| `` vi /data/lxc/containers/system/rootfs/etc/fstab``
| `` # remove the all mount points that says "added by lxc-android-boot except one with /vendor"``
