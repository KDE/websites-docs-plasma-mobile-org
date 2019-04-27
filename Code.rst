Get the source code
===================

Frameworks
----------

Plasma Mobile is built on top of `KDE
Frameworks <https://projects.kde.org/projects/frameworks>`__. You can
find download links for the stable releases of these on
`download.kde.org <http://download.kde.org/stable/frameworks/>`__ and
its API documentation
`here <http://api.kde.org/frameworks-api/frameworks5-apidocs/>`__.

Plasma
------

Plasma provides components for multi-device workspaces. Plasma desktop
provides a stable, versatile and modular desktop.
Plasma Mobile shares many components and all of its underlying
architecture with Plasma desktop, but provides a UI and features
necessary and useful on mobile devices. The idea is to provide a base
platform, that can be augmented with device-specific modules. On a
laptop, one would typically install plasma-workspace and plasma-desktop,
while on a mobile device, one would install plasma-workspace and
plasma-mobile and friends. None of the repositories conflicts with
others, so it's possible to provide a system with UIs for multiple
formfactors, and decide at runtime, which UI to offer. You can find the
source code tarballs of the stable releases of Plasma on
`download.kde.org <http://download.kde.org/stable/plasma/>`_ or check the git
repositories on `cgit.kde.org <https://cgit.kde.org/>`_.

Plasma Mobile
-------------
Plasma Mobile does not have stable release yet. So, we advise you to pull
the code directly from our git repositories' master branches at `invent.kde.org <https://invent.kde.org/>`_. In the following section you may find direct links to the source code of several Plasma Mobile components.

General mobile components
~~~~~~~~~~~~~~~~~~~~~~~~~
The following git modules are generally useful on mobile devices and
provide touch-friendly functionality.

-  `plasma-settings <https://invent.kde.org/kde/plasma-settings>`_: Settings application and modules.
-  `plasma-camera <https://invent.kde.org/kde/plasma-camera>`_: Camera application.
-  `marble <https://cgit.kde.org/marble.git/>`_: Maps application.
-  `koko <https://cgit.kde.org/koko.git/>`_: Gallery application.
-  `vvave <https://cgit.kde.org/vvave.git/>`_: Music player.
-  `okular <https://cgit.kde.org/okular.git/>`_: Document Viewer.
-  `discover <https://cgit.kde.org/discover.git>`_: Software Center.
-  `plasma-angelfish <https://cgit.kde.org/plasma-angelfish.git>`_: Webbrowser for phones.
-  *plasma-samegame*: Small example game, pure QML.
-  *mtp-server*: Fork of Ubuntu's MTP server
-  `kaidan <https://git.kaidan.im/kaidan/kaidan>`_: XMPP based Messenger.
-  `qmlkonsole <https://invent.kde.org/jbbgameich/qmlkonsole>`_: Terminal application.
-  `peruse <https://cgit.kde.org/peruse.git>`_: A comic book viewer

Plasma phone components
-----------------------

These additional git modules are useful for smartphones:

-  `simplelogin <https://invent.kde.org/bshah/simplelogin>`_: Display manager for launching the plasma-phone shell
-  `plasma-phone-components <https://invent.kde.org/kde/plasma-phone-components>`_: Dialer and Phone shell QtQuick code,
   application launcher model, etc.

Build the source code
---------------------

In order to build Plasma Mobile from source, please refer to the `build instructions <https://community.kde.org/Frameworks/Building>`__.
