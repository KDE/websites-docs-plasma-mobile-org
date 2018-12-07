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

Plasma Mobile application development
-------------------------------------

Getting involved with Plasma Mobile application environment is a perfect
opportunity to familiarize with a set of important technologies:

-  Qt, the cross-platform application framework for creating
   applications that run on various software and hardware platforms with
   little or no change in the underlying codebase
-  QML, the UI specification and programming language that allows
   designers and developers to create applications with fluid
   transitions and effects, which are quite popular in mobile devices.
   QML is a declarative language offering a highly readable,
   declarative, JSON-like syntax with support for imperative JavaScript
   expressions.
-  Qt Quick, the standard library of types and functionality for QML. It
   includes, among many others, visual types, interactive types,
   animations, models and views. A QML application developer can get
   access this functionality with a single import statement.
-  CMake, the cross-platform set of tools designed to build, test and
   package software, using a compiler-independent method.
-  Kirigami, a set of QtQuick components, facilitating the easy creation
   of applications that look and feel great on mobile as well as on
   desktop devices, following the Kirigami Human Interface Guidelines.

Documentation resources
~~~~~~~~~~~~~~~~~~~~~~~

In this section you will find a set of technical resources that will
accompany you during your journey as a Plasma Mobile developer. If you
are just starting out with Qt, QML and CMake, you will find here enough
detail so as to feel comfortable with the technologies related to Plasma
Mobile development. If you are an experienced Qt developer, you can find
here valuable resources so as to comply with best practices.

QtQuick and QML
^^^^^^^^^^^^^^^

-  `QML Applications <https://doc.qt.io/qt-5/qmlapplications.html>`__
-  `First Steps with QML <https://doc.qt.io/qt-5/qmlfirststeps.html>`__
-  `Getting Started Programming with Qt
   Quick <https://doc.qt.io/qt-5/gettingstartedqml.html>`__
-  `QML Glossary <https://doc.qt.io/qt-5/qml-glossary.html>`__
-  `QML Reference <https://doc.qt.io/qt-5/qmlreference.html>`__
-  `QML types list <https://doc.qt.io/qt-5/qmltypes.html>`__

Kirigami
^^^^^^^^

-  `Kirigami Human Interface
   Guidelines <https://community.kde.org/KDE_Visual_Design_Group/KirigamiHIG>`__
-  `Kirigami
   API <https://api.kde.org/frameworks/kirigami/html/index.html>`__

CMake
^^^^^

-  `CMake Documentation <https://cmake.org/documentation/>`__
-  `CMake Guidelines and
   How-tos <https://community.kde.org/Guidelines_and_HOWTOs/CMake>`__

First steps with Plasma Mobile development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now you are familiar with the technologies related with Plasma Mobile,
it's time to create our first Plasma Mobile application. Creating a
Plasma Mobile application based on Kirigami components is quite easy.

To properly build a plasma mobile application, a set of development
packages should be installed. When working on a KDE Neon based
development environment, we suggest installing:

Following the below steps we will create a simple Kirigami application
in a few minutes.

-  Download
   `hellokirigami <https://community.kde.org/File:Hellokirigami.tar.gz>`__
   archive
-  Unzip the archive
-  Open src/contents/ui/main.qml
-  Change text:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   text:  qsTr("Hello Kirigami")
   </nowiki>}}

to

-  Using Konsole, navigate to hellokirigami folder:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   cd hellokirigami
   </nowiki>}}

-  Create build folder:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   mkdir build
   </nowiki>}}

-  Navigate to build folder:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   cd build
   </nowiki>}}

-  Build and install, executing the below commands:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   cmake -DKDE_INSTALL_USE_QT_SYS_PATHS=ON ..
   make
   sudo make install 
   </nowiki>}}

-  Run the application, typing:

.. raw:: mediawiki

   {{Input|1=<nowiki>
   hellokirigami
   </nowiki>}}

If you can see this image:

.. figure:: Hellokirigami.png
   :alt: Hellokirigami.png
   :width: 250px

   Hellokirigami.png

you have successfully created your first Plasma Mobile application!

To display the application like it would be displayed on the phone, use:
