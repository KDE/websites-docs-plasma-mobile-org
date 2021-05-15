Setup the development environment
=================================

The development environment needed depends on the specific area of
Plasma Mobile you want to contribute. There are three kinds of
development environment:

#. General Linux-based development environment
#. Plasma Mobile emulated environment
#. Mobile device running plasma mobile

For most tasks you will not need an actual mobile device running Plasma
Mobile. In specific, the development environment needed for working on each task can be found in the description of each `phabricator task <https://phabricator.kde.org/project/view/28/>`__.

General Linux-based development environment
-------------------------------------------
When a mobile device is not needed, you may use your Linux distribution for development. In such a case, there are several options offered in order to set up the development environment.

Flatpak
~~~~~~~
Flatpak is a framework for distributing Linux applications. You may build a Plasma Mobile application that runs on top of the KDE Application Platform runtime, using the KDE Software Development Kit (the matching SDK of the platform runtime). For more information, please check the `application development <AppDevelopment.html>`__ section.

kdesrc-build
~~~~~~~~~~~~
The *kdesrc-build* command line tool downloads, manages, and builds KDE source code repositories. Following the relative `instructions <https://community.kde.org/Get_Involved/development#Set_up_kdesrc-build>`__ you can safely build and install KDE libraries and applications from source, without messing with the installation of your host system.

Plasma Mobile emulated environment
----------------------------------

Get the installation image
~~~~~~~~~~~~~~~~~~~~~~~~~~

At first, you have to download the Plasma Mobile `installation
image <https://www.plasma-mobile.org/get/#desktop>`__. This image is
using the same packages as the Neon based reference rootfs, just
compiled for amd64. We will use it to set up our development virtual
machine but you may use it to test Plasma Mobile in a non-android intel
tablet as well.

Install QEMU
~~~~~~~~~~~~

To proceed to installing, QEMU is needed. QEMU is a free and open-source hosted hypervisor that performs hardware virtualization. We will also need Virgil 3D. Virgil is a project that allows the guest operating system to use the capabilities of the host GPU to accelerate 3D rendering. OpenGL support on the guest system -Plasma Mobile virtual machine- is required for executing Calamares, that will enable us to install Plasma Mobile.

Nevertheless, in some distributions QEMU is not packaged with Virgil 3D support. If your distribution provides such a qemu package just install QEMU using you distribution's application store. If you are using KDE Neon or an Ubuntu derivative, since Virgil 3D is not enabled in the QEMU package of the Ubuntu repository, you can install the `qemu-virgil <https://snapcraft.io/qemu-virgil/>`__ snap package and connect to the kvm interface, executing the following commands:

.. highlight:: bash

::

    sudo snap install qemu-virgil
    sudo snap connect qemu-virgil:kvm

    
Create Virtual Machine
^^^^^^^^^^^^^^^^^^^^^^

Now, we are going to create the Plasma Mobile virtual machine, running:

::

    qemu-virgil.qemu-img create -f raw plamodisk 40G
    qemu-virgil -enable-kvm -show-cursor -m 2048 -boot menu=on -cdrom /path/to/neon-pm-devedition-gitunstable-YYYYMMDD-HHMI-amd64.iso -device virtio-vga,virgl=on -display gtk,gl=on -boot order=d -drive file=plamodisk,format=raw

If you have no permission to access the KVM kernel module, you can execute:

::

    sudo chmod 666 /dev/kvm

During booting, we suggest you to increase the window size of the QEMU window. If the resolution of your screen is low, some parts of the windows you open (e.g. Calamares) may be hidden. As a workaround, you can execute the below steps:

- Select View > Grab Input, or just press Ctrl+Alt+G. Probably mouse pointer will not be displayed anymore in the guest system.
- Holding Alt key, drag the window to show the invisible part
- Press Ctrl+Alt+G again to display the mouse pointer

To start the installation process, we execute Konsole, select a user password and open Calamares running:

::

    passwd
    sudo -E calamares

Follow Calamares instructions to install our system.

-  Select your language
-  Select your time zone
-  Select your keyboard
-  Choose the default partitioning scheme
-  Create the system user
-  Validate your options

Then, installation process will start. When completed, shutdown your virtual machine.

Since installation has been completed, we no longer need the
installation image. So, we will start our ready-to-use workstation
executing:

::

    qemu-virgil -device virtio-vga,virgl=on -display gtk,gl=on -m 2G -enable-kvm -boot order=d -drive file=plamodisk,format=raw

Plasma Mobile device environment
--------------------------------

You may also run Plasma Mobile on an actual mobile device, either
by installing postmarketOS or using Halium as hardware adaption layer.

postmarketOS
~~~~~~~~~~~~

`PostmarketOS <https://postmarketos.org/>`__ is a touch-optimized,
pre-configured Alpine Linux-based distribution. Currently, postmarketOS
has been ported to many devices. Ensure that your device belongs to the
relative `list <https://wiki.postmarketos.org/wiki/Devices>`__ and
proceed to installation, following the `installation
instructions <https://wiki.postmarketos.org/wiki/Installation_guide>`__
provided by postmarketOS team. When asked during installation, just
select “plasma-mobile” as the user interface:

::

    Available user interfaces (5):
    * none: No graphical environment
    * hildon: (X11) Lightweight GTK+2 UI (optimized for single-touch touchscreens)
    * luna: (Wayland) webOS UI, ported from the LuneOS project (Not working yet)
    * plasma-mobile: (Wayland) Mobile variant of KDE Plasma, optimized for touchscreen
    * weston: (Wayland) Reference compositor (demo, not a phone interface)
    * xfce4: (X11) Lightweight GTK+2 desktop (stylus recommended)
    User interface [weston]: plasma-mobile
