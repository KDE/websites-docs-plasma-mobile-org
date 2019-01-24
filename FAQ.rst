Frequently Asked Questions
==========================

Why is Plasma Mobile not using Mer/Nemomobile?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plasma Mobile is a software platform for mobile devices. It is not an
operating system in of itself, it consists of Qt5, the KDE Frameworks,
Plasma and various software that's part of the application set. Plasma
Mobile can work on top of the Mer distribution, but due to a lack of
time and resources, we're currently focusing on Halium as a base for
testing and development.

Can Android apps work on Plasma Mobile?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the future, potentially, but currently no. There are projects like
`Anbox <https://anbox.io/>`__ which are seeking to have Android apps run
on the Linux desktop without any performance loss, and with full
integration. This could be leveraged in the future to have Android apps
running on top of a GNU/Linux system with the Plasma Mobile platform,
but it's a complicated task, and it's not a priority right now.

Can I run Plasma Mobile on my mobile device?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Currently, Plasma Mobile runs on the following device types:

-  **(Recommended) Nexus 5X:** We offer official
   installation scripts for the Nexus 5X. The images are
   built on top of Halium and KDE Neon (16.04 in the latest stable
   rootfs, 18.04 in the latest "edge" rootfs). `You can find more
   information
   here. <https://www.plasma-mobile.org/neon-arch-reference-rootfs/>`__

-  **x86-based:** If you want to try out Plasma Mobile on an Intel
   tablet, desktop/laptop, or virtual machine, the `x86_64 Neon-based
   Plasma Mobile <https://www.plasma-mobile.org/get/>`__ image is for
   you. Keep in mind that it has not been updated since March 2018, and
   is still based on Ubuntu 16.04, meaning that newer hardware may have
   issues due to the older drivers. Information on how to permanently
   install it can be found on the `Plasma Mobile developer
   guide <https://community.kde.org/Plasma/Mobile/DevGuide>`__. **This
   image is not representative of the current state of Plasma Mobile,
   and has many issues due to its age.**

-  **postmarketOS devices:** postmarketOS is a touch-optimized,
   pre-configured Alpine Linux that can be installed on Android
   smartphones and other mobile devices. This project is in *very early
   stages of development* but it does currently offer support for a
   fairly wide range of devices, and it offers Plasma Mobile as an
   available interface. Please find your device from the `list of
   supported devices <https://wiki.postmarketos.org/wiki/Devices>`__ and
   see what's working, then you can follow the `pmOS installation
   guide <https://wiki.postmarketos.org/wiki/Installation_guide>`__ to
   install it on your device. Your mileage may vary, and it is **not**
   necessarily representative of the current state of Plasma Mobile.

-  **Other:** If your device is not listed here, you can manually
   install the Plasma Mobile rootfs onto your device if it's supported
   by Halium. You can check
   `here <https://github.com/Halium/projectmanagement/labels/Ports>`__
   to see if your device is listed, and if so, you can see the status
   and the functional device features. Assuming your device is listed
   and well-supported, you can follow the `Halium Porting
   Guide <https://docs.halium.org/en/latest/>`__ to install it on your
   own device, and reuse the manifest linked in the device-specific page
   on the Github. If there are no reports for your device, you can still
   install Halium on it similarly, but you'll have to follow the
   instructions in the porting guide to create the manifest yourself.

What are requirements of device for porting Plasma Mobile?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  For Android based devices (ARM and x86) a Halium port is required:

   -  Device tree & Kernel source
   -  Kernel version must be 3.8.0 or later
   -  Devices with MediaTek chipsets are not recommended
   -  16 GiB of storage is recommended, but 8 GiB could also work
   -  1 GiB of RAM is needed

-  Intel or AMD based devices (with open source firmware) just can use
   the x86 ISO

I've installed Plasma Mobile, what is the login password?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you've installed it onto your Nexus 5X via the installation script,
the password should be "1234", and you can then change it afterwards by
running "passwd" in Konsole.

If you're using the (much older) x86 image, no password is set by
default, and you'll have to set it by running "passwd" in Konsole before
you can authenticate for anything.

What's the state of the project?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plasma Mobile is currently under heavy development and is not intended
to be used as a daily driver. For more information, check out `this page
on the wiki which is kept up-to-date with the current project
status. <https://community.kde.org/Plasma/Mobile/General>`__
