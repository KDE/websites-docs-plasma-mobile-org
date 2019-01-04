Getting the Code
================

You can check out any of these repos easily:

``git clone kde:``\ 

You should add the "kde" alias to your `Git
setup <https://techbase.kde.org/Development/Git/Configuration#URL_Renaming>`__.
The code for various Plasma Mobile components can be found on `the git
repository <https://phabricator.kde.org/diffusion/query/H_KxUC6zq6ET/>`__
(but please read below for more details).

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
`download.kde.org <http://download.kde.org/stable/plasma/>`__. Git
repositories can be browsed on `cgit.kde.org <https://cgit.kde.org/>`__
and `invent.kde.org <https://invent.kde.org/>`__.

As Plasma Mobile does not have stable release yet, we advise you to pull
the code to create tarballs directly from our git repositories' master
branches. Relevant repositiories for Plasma Mobile are:

General Mobile Components
-------------------------

The following git modules are generally useful on mobile devices and
provide touch-friendly functionality.

-  `plasma-settings <https://invent.kde.org/kde/plasma-settings>`_: Settings application and modules.
-  `plasma-camera <https://invent.kde.org/kde/plasma-camera>`_: Camera application.
-  *marble*: Maps application.
-  *koko*: Gallery application.
-  *vvave*: Music player.
-  *okular*: Document Viewer.
-  *discover*: Software Center.
-  *plasma-angelfish*: Proof-of-concept demo webbrowser for phones.
-  *plasma-samegame*: Small example game, pure QML.
-  *mtp-server*: Fork of Ubuntu's MTP server
-  `kaidan <https://git.kaidan.im/kaidan/kaidan>`_: XMPP based Messenger.
-  `qmlkonsole <https://invent.kde.org/jbbgameich/qmlkonsole>`_: Terminal application.

Phone Specific
--------------

These additional git modules are useful for smartphones.

-  *plasma-phone-components*: Dialer and Phone shell QtQuick code,
   application launcher model, etc.

Development Tools
-----------------

-  *xbuilder*: Script to set up an SDK.
-  *xutils*: Utilities for the SDK.

Building the code
=================

In order to build Plasma Mobile, please refer to the `build
instructions <https://community.kde.org/Frameworks/Building>`__.
