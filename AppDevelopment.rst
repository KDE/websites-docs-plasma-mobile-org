Plasma Mobile application development
=====================================

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

Using the Kirigami application template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We will use the KDE flatpak SDK to develop and package the app, so all
that is required is a working flatpak and flatpak-builder installation.

On Debian and derivates, you can use
``sudo apt install flatpak flatpak-builder``.

First, clone the app template:
``git clone https://invent.kde.org/jbbgameich/plasma-mobile-app-template.git``

This repository can be used as a template to develop Plasma Mobile
applications. It already includes templates for the qml ui, a c++ part,
app metadata and flatpak packaging.

Local building and testing using the SDK
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   # Install the SDK
   flatpak install flathub org.kde.Sdk # Only needs to be done once

   # Build
   flatpak-builder flatpak-build-desktop --force-clean --ccache *.json

   # Start
   export QT_QUICK_CONTROLS_MOBILE=true QT_QUICK_CONTROLS_STYLE=Plasma # Required for making the application look like started on a phone
   flatpak-builder --run flatpak-build-desktop *.json hellokirigami


If you can see this image:

.. figure:: Hellokirigami.png
   :alt: Hellokirigami.png
   :width: 250px

   Hellokirigami.png

you have successfully created your first Plasma Mobile application!

Creating a flatpak for the phone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This assumes your system is already set up as described
`here <https://community.kde.org/Guidelines_and_HOWTOs/Flatpak>`__. Make
sure your system also supports qemu user emulation. If not, you can find
help for example `here <https://wiki.debian.org/QemuUserEmulation>`__

::

   flatpak install flathub org.kde.Sdk/arm/5.11 # Only needs to be done once
   flatpak-builder flatpak-build-phone --repo=arm-phone --arch=arm --force-clean --ccache *.json
   flatpak build-bundle arm-phone app.flatpak org.kde.hellokirigami

Now your app is exported into app.flatpak. You can copy the file to the
phone using scp:

::

   scp app.flatpak phablet@10.15.19.82:/home/phablet/app.flatpak

::

   ssh phablet@10.15.19.82
   flatpak install app.flatpak

Your new application should now appear on the homescreen.

Using the template to develop your application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Edit the files to fit your naming and needs. In each command, replace
“io.you.newapp” and “newapp” with the id and name you want to use

::

   sed -i 's/org.kde.hellokirigami/io.you.newapp/g;s/hellokirigami/newapp/g' $(find . -name "CMakeLists.txt" -or -name "*.desktop" -or -name "*.xml" -or -name "*.json")

   for file in $(find . -name "org.kde.hellokirigami*"); do mv $file $(echo $file | sed 's/org.kde.hellokirigami/io.you.newapp/g'); done

Submitting your new application to the repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once your application is working and is usable, you can submit a patch
to include it into the KDE flatpak repository.

After setting up git with the recommended `KDE
settings <https://community.kde.org/Infrastructure/Git#Pushing>`__, you
can create a new file io.you.newapp.remoteapp in the
flatpak-kde-applications repository.

``git clone kde:flatpak-kde-applications && cd flatpak-kde-applications``

Paste the following content into the file:

::

   ID=io.you.newapp
   JSON=io.you.newapp.json
   GITURL=https://gitlab.com/you/newapp.git

You can now submit the patch on
`Phabricator <https://community.kde.org/Infrastructure/Phabricator>`__.
Once accepted, your app will be automatically built, published and made
available in Discover (if the KDE flatpak repository is enabled on the
device).
