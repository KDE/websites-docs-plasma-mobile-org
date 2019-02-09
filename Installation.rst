Installation
=========================================

In this page you will find detailed instructions about flashing Plasma Mobile to your mobile phone.

.. tip::  Before proceeding, please ensure that you have installed adb and fastboot on you workstation

If you flash Plasma Mobile for the first time, you likely need to follow
steps 1A and 1B.
After successfully done those steps, you can always jump straight to
step 2 for future flashing.

1A. Unlock the bootloader
~~~~~~~~~~~~~~~~~~~~~~~~~

In `xda forum <https://forum.xda-developers.com/nexus-5x/general/guides-how-to-guides-beginners-t3206930>`_ you will find instructions to unlock the bootloader of the Nexus 5X. Skip this step if you phone is already unlocked.


1B. Format cache and userdata
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. attention:: This will complete erase the memory of your Nexus 5X device. Please ensure you have a backup of all your data before you proceed.

Put your Nexus 5X in fastboot mode (press volume down + power button
at once), then open a terminal and run:

::

    fastboot format cache 
    fastboot format userdata

2. Flash Plasma Mobile
~~~~~~~~~~~~~~~~~~~~~~

While your phone is still in fastboot mode, start flashing it by cloning the pm-flashtool repository and executing the pm-flash script:

::

    git clone https://github.com/plasma-phone-packaging/pm-flashtool.git
    cd pm-flashtool
    ./pm-flash

If you have already executed the pm-flash script in the past and you would like to avoid to download all the files again, the -c parameter of script can be used to re-use the **cache** instead.

You may also try the -p parameter. The -p (platform) parameter currently accepts **neon**, **arch** and **edge**. Edge rootfs is the latest development snapshot of Plasma Mobile and the platform recommended for testing new features and bug fixes.

.. tip:: Before reporting bugs, we recommend testing with the edge image to see if the issues still occur with the latest code.

The pm-flash script will download the files required and will store them in the cache subdir. For example:

-   pm-rootfs-20180124-113114.tar.gz (Plasma Mobile rootfs)
-   recovery.img (TWRP recovery img)
-   system.img (Halium image containing the hardware adaption for each device)
-   boot.img (Halium initrd)

Then the TWRP Recovery img is used as a tool to flash all other
components to the target device.

.. attention:: The above could take quite some time (15-20 minutes) and requires you to enter your password to continue.

Useful tips
~~~~~~~~~~~

If flashing has completed successfully, you can reboot your phone and you should automatically login to a working session. The username of the logged-in user is phablet and the password is ‘1234’.

Now you will probably need some tips that will make your life a little bit easier.

Copy a file to the phone via USB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To copy a file from your host-system to the phone, execute:

::

    scp /path/to/file phablet@10.15.19.82:/home/phablet

Remote connection via USB
^^^^^^^^^^^^^^^^^^^^^^^^^

If your phone is connected via usb and running Plasma Mobile, it can be
remotely controlled from the attached computer via the command line using
ssh:
::

    ssh phablet@10.15.19.82

Connect via WiFi
^^^^^^^^^^^^^^^^
To easily establish a wifi connection, execute:
::

    nmcli dev wifi con "ssid" password "password"

Resize root partition
^^^^^^^^^^^^^^^^^^^^^
To resize the root partition on the phone, first reboot to recovery
and then from adb shell, run the following:

::

    e2fsck -yf /data/rootfs.img
    resize2fs -f /data/rootfs.img 1024000

This will double the size of the rootfs.
