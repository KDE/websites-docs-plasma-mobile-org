Introduction
============

What is Plasma Mobile?
~~~~~~~~~~~~~~~~~~~~~~

Plasma Mobile offers a free (as in freedom and beer), user-friendly,
privacy-enabling and customizable platform for mobile devices. Based on
the `Halium <https://halium.org/>`__ project, we have official
installable prototypes for two Android devices. However, Plasma Mobile
is under heavy development and unfortunately cannot be used as a daily
driver for most people.

Halium isn't Plasma Mobile: Halium is only providing support to packing
a basic Android system based upon LineageOS into an LXC container into a
normal GNU/Linux system. Then the host system is able to use the
proprietary android firmware into a normal system using
`libhybris <https://en.wikipedia.org/wiki/Hybris_(software)>`__, which
is the simplest way without reverse-engineering all drivers.
Unfortunately, this requires old and often outdated Android kernels to
be used under-the-hood.

Why?
~~~~

The most common offerings on mobile devices lack openness and trust. In
a world of walled gardens, we want to create a platform that respects
and protects the user's privacy to the fullest. We want to provide a
fully open base which others can help develop and use for themselves, or
in their products.

Can I use it?
~~~~~~~~~~~~~

Official Plasma Mobile images based on Halium and KDE Neon Git-Unstable
are provided. We mainly test and support the Nexus 5 and Nexus 5X at
this time, but you can potentially install it on any device supported by
Halium. These Halium images are where most current development is
happening, and scripts are supplied to ease installation of the Plasma
Mobile rootfs onto Nexus 5 and 5X devices. `See further instructions
here. <https://www.plasma-mobile.org/neon-arch-reference-rootfs/>`__

There is also postmarketOS, a touch-optimized Alpine Linux with support
for many more devices, and while it's in very early stages of
development, it offers Plasma Mobile as an available interface for the
devices it supports. `You can see the list of supported devices here,
but given the state of pmOS, your mileage may
vary. <https://wiki.postmarketos.org/wiki/Devices>`__

The interface is using KWin over Wayland and is now mostly stable,
albeit a little rough around the edges in some areas. A subset of the
normal KDE Plasma features are available, including widgets and
activities, both of which are integrated into the Plasma Mobile UI.

What can it do?
~~~~~~~~~~~~~~~

There are quite a few touch-optimized apps that are now being bundled
with the Neon-based Plasma Mobile image, allowing a wide range of basic
functions. These are mostly built with Kirigami, KDE's interface
framework allowing convergent UIs that work very well in a touch-only
environment. The included software is bound to change with time, but in
the current "edge" rootfs, you can find:

-  The Angelfish web browser, based on QtWebEngine/Chromium, with full
   touch support including pinch-to-zoom
-  The vvave music player
-  The Discover software center (playing the role of an "app store" in
   this context)
-  Marble Maps
-  The Index file manager (from the Maui Project but designed for both
   Android and Plasma Mobile)
-  A simple camera application
-  Peruse, a comic book reader
-  vPlayer, a video player that can play local and remote files, along
   with support for searching and playing YouTube videos

(Note: The list of included software will change often. We'll attempt to
keep this list updated but it may be inaccurate at times.)

Other software can be installed from Discover. Any Qt5 app (including
software designed for the desktop, such as Calligra) will run without
many issues, though it will still be difficult to use on a touch
display. Currently, software built with the GTK toolkit *will* run, but
with major scaling issues, and without any support for the on-screen
keyboard. If possible, use software built with Kirigami for best
results.

You cannot currently connect to the cell network, and as such, you
cannot make calls or texts. Similarly, data is also non-functional.
However, a full-featured dialer is included, and we are currently
exploring options for integrating SMS. Once the relevant pieces are put
in for cell connections, you should be able to immediately make calls
with the current dialer.

Supported features of the Nexus 5 and Nexus 5X include the camera,
Wi-FI, battery monitoring and power settings, and audio (including
hardware volume buttons).

A list of most known issues with the current "edge" rootfs can be found
`here <https://notes.kde.org/public/plamo-testing>`__ and
`here. <https://phabricator.kde.org/tag/plasma%3A_mobile/>`__

Where can I find...
~~~~~~~~~~~~~~~~~~~

More info, such as installation instructions, is available in the
`Plasma Mobile Wiki <http://community.kde.org/Plasma/Mobile>`__ and on
the `Plasma Mobile website <http://www.plasma-mobile.org>`__.

The code for various Plasma Mobile components can be found on
`git.kde.org <https://projects.kde.org/projects/playground/mobile>`__.

You can also ask your questions in the `Plasma Mobile community groups
and channels <https://www.plasma-mobile.org/join/>`__.
