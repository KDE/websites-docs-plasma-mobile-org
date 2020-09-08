Get the source code
===================

Frameworks
----------

Plasma Mobile is built on top of `KDE
Frameworks <https://kde.org/products/frameworks/>`__. You can
find download links for the stable releases of these on
`download.kde.org <https://download.kde.org/stable/frameworks/>`__ and
its API documentation
`here <https://api.kde.org/>`__.

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
`download.kde.org <https://download.kde.org/stable/plasma/>`_ or check the git
repositories on `invent.kde.org <https://invent.kde.org/plasma>`_.

Plasma Mobile
-------------
Plasma Mobile does not have stable release yet. So, we advise you to pull
the code directly from our git repositories' master branches at `invent.kde.org <https://invent.kde.org/public/>`_. In the following section you may find direct links to the source code of several Plasma Mobile components.

General mobile components
~~~~~~~~~~~~~~~~~~~~~~~~~
The following git modules are generally useful on mobile devices and
provide touch-friendly functionality.

-  `plasma-settings <https://invent.kde.org/kde/plasma-settings>`_: Settings application and modules.
-  `plasma-camera <https://invent.kde.org/kde/plasma-camera>`_: Camera application.
-  `marble <https://invent.kde.org/education/marble>`_: Maps application.
-  `koko <https://invent.kde.org/graphics/koko>`_: Gallery application.
-  `vvave <https://invent.kde.org/maui/vvave>`_: Music player.
-  `okular <https://invent.kde.org/graphics/okular>`_: Document Viewer.
-  `discover <https://invent.kde.org/plasma/discover>`_: Software Center.
-  `plasma-angelfish <https://invent.kde.org/plasma-mobile/plasma-angelfish>`_: Webbrowser for phones.
-  `plasma-samegame <https://invent.kde.org/plasma-mobile/plasma-samegame>`_: Small example game.
-  `mtp-server <https://invent.kde.org/plasma-mobile/mtp-server>`_: Fork of Ubuntu's MTP server
-  `kaidan <https://invent.kde.org/kde/kaidan>`_: XMPP based Messenger.
-  `qmlkonsole <https://invent.kde.org/jbbgameich/qmlkonsole>`_: Terminal application.
-  `peruse <https://invent.kde.org/graphics/peruse>`_: A comic book viewer.
-  `calindori <https://invent.kde.org/kde/calindori>`_: Calendar and todo management application.
-  `plasmatube <https://invent.kde.org/lnj/plasmatube>`_: Work-in-progress youtube client.
-  `index <https://invent.kde.org/kde/index-fm>`_: File manager.
-  `pix <https://invent.kde.org/kde/maui-pix>`_: Maui Image Gallery.
-  `qrca <https://invent.kde.org/kde/qrca>`_: QR Code scanner.
-  `keysmith <https://invent.kde.org/kde/keysmith>`_: OTP client.
-  `kweather <https://invent.kde.org/plasma-mobile/kweather>`_: Weather app.
-  `kclock <https://invent.kde.org/plasma-mobile/kclock>`_: Clock app.
-  `kalk <https://invent.kde.org/plasma-mobile/kalk>`_: Calculator and units converter.
-  `trainer <https://invent.kde.org/plasma-mobile/trainer>`_: Sport exercises helper.
-  `alligator <https://invent.kde.org/plasma-mobile/alligator>`_: RSS feeds reader.
-  `krecorder <https://invent.kde.org/plasma-mobile/krecorder>`_: Audio recorder.

Plasma phone components
-----------------------

These additional git modules are useful for smartphones:

-  `simplelogin <https://invent.kde.org/bshah/simplelogin>`_: Display manager for launching the plasma-phone shell
-  `plasma-phone-components <https://invent.kde.org/kde/plasma-phone-components>`_: Phone shell QtQuick code,
   application launcher model, etc.
-  `plasma-dialer <https://invent.kde.org/kde/plasma-dialer>`_: Dialer application
-  `kpeoplevcard <https://invent.kde.org/pim/kpeoplevcard>`_: Provides the vCard based contact management backend used by the dialer, plasma-phonebook and others.
-  `plasma-phonebook <https://invent.kde.org/KDE/plasma-phonebook>`_: Contact management application.
-  `spacebar <https://invent.kde.org/plasma-mobile/spacebar>`_: Work-in-progress SMS application.

Build the source code
---------------------

In order to build Plasma Mobile from source, please refer to the `build instructions <https://community.kde.org/Frameworks/Building>`__.

Improving this documentation
-----------------------------

This documentation is written using Sphinx and can be edited easily as it is based on plain `reStructuredText <https://en.wikipedia.org/wiki/ReStructuredText>`_. Please open Merge Requests (MR) on:

 - `docs-plasma-mobile-org <https://invent.kde.org/websites/docs-plasma-mobile-org>`_: Plasma Mobile Documentation
 
