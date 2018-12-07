Plasma Mobile Developer Guide
=============================

The development environment needed depends on the specific area of
Plasma Mobile you want to contribute. There are three kinds of
development environment:

#. General Linux-based development environment
#. Plasma Mobile emulated environment
#. Mobile device running plasma mobile

For most tasks you will not need an actual mobile device running Plasma
Mobile. Moreover, although you may contribute to Plasma Mobile using any
kind of linux-based development environment , we strongly suggest to use
the Plasma Mobile pre compiled image, since such an environment will
make the development much easier and facilitate the testing of your work
on a machine that emulates a mobile device.

Plasma Mobile emulated environment
----------------------------------

Get the Plasma Mobile installation image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At first, you have to download the Plasma Mobile `installation
image <https://www.plasma-mobile.org/get/#desktop>`__. This image is
using the same packages as the Neon based reference rootfs, just
compiled for amd64. We will use it to set up our development virtual
machine but you may use it to test Plasma Mobile in a non-android intel
tablet as well.

Compile QEMU
~~~~~~~~~~~~

To proceed to installing, QEMU should be installed. QEMU is a free and
open-source hosted hypervisor that performs hardware virtualization.
Nevertheless, in many distributions QEMU is not packaged with Virgil 3d
support. If your distribution provides such a qemu package you may omit
and proceed to just installing QEMU using you distribution's application
store.

Install Virglrenderer
^^^^^^^^^^^^^^^^^^^^^

Virgil is a project that allows the guest operating system to use the
capabilities of the host GPU to accelerate 3D rendering. OpenGL support
on the guest system -Plasma Mobile virtual machine- is required for
executing Calamares, that will enable us to install Plasma Mobile. We
can compile Virgil executing the following commands:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   git clone git://git.freedesktop.org/git/virglrenderer
   cd virglrenderer
   ./autogen.sh
   make
   sudo make install
   </nowiki>}}

Install build dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^

Depending on your distribution, dependency package names may vary. In
KDE Neon, you may install the packages required to compile QEMU
executing:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   sudo apt install libgtk-3-dev libspice-server-dev libspice-protocol-dev libusbredirparser-dev build-essential libepoxy-dev libdrm-dev libgbm-dev libx11-dev libpulse-dev libsdl2-dev
   </nowiki>}}

Install QEMU
^^^^^^^^^^^^

To let our system have access to /usr/local/lib we run:

To compile QEMU with the set of options that fit our needs and then
install it we execute:

Create Virtual Machine
^^^^^^^^^^^^^^^^^^^^^^

Now, we need to create the Plasma Mobile virtual machine, running: Then,
we execute Konsole, select a user password and open Calamares running:

Follow Calamares instructions to install our system.

-  Select your language
-  Select your time zone
-  Select your keyboard
-  Choose the default partitioning scheme
-  Create the system user
-  Validate your options

Then, installation process will start.

When completed, shutdown your virtual machine.

Since installation has been completed, we no longer need the
installation image. So, we will start our ready-to-use workstation
executing:

Mobile device running plasma mobile
-----------------------------------

Currently, you may run Plasma Mobile on an actual mobile device, either
by installing postmarketOS or using Halium as hardware adaption layer.

Using postmarketOS
~~~~~~~~~~~~~~~~~~

`PostmarketOS <https://postmarketos.org/>`__ is a touch-optimized,
pre-configured Alpine Linux-based distribution. Currently, postmarketOS
has been ported to many devices. Ensure that your device belongs to the
relative `list <https://wiki.postmarketos.org/wiki/Devices>`__ and
proceed to installation, following the `installation
instructions <https://wiki.postmarketos.org/wiki/Installation_guide>`__
provided by postmarketOS team. When asked during installation, just
select “plasma-mobile” as the user interface:

Using Halium
~~~~~~~~~~~~

`Halium <https://halium.org/>`__ provides the minimal android layer that
allows a non-Android graphical environment to interact with the
underlying Android kernel and access the hardware. Currently Halium has
been ported to many devices. The Plasma Mobile team provides a
Neon-based rootfs which can be used along with the Halium builds. This
image is based on the dev-unstable branch of KDE Neon, and always ships
the latest versions of KDE frameworks, kwin and Plasma Mobile.

To run Plasma Mobile using Halium as hardware adaption layer:

-  Ensure that Halium has been
   `ported <https://github.com/halium/projectmanagement/issues?q=is%3Aissue+is%3Aopen+label%3APorts>`__
   to your device
-  Download the Neon-based rootfs
   `image <https://www.plasma-mobile.org/get/>`__
-  Get the source
   `manifest <https://github.com/halium/projectmanagement/issues?q=is%3Aissue+is%3Aopen+label%3APorts>`__
-  Follow the Halium documention `detailed
   instructions <http://docs.halium.org/en/latest/>`__ to install Halium
