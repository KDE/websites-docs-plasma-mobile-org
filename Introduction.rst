Introduction
============

What is Plasma Mobile?
~~~~~~~~~~~~~~~~~~~~~~

Plasma Mobile offers a free (as in freedom and beer), user-friendly,
privacy-enabling and customizable platform for mobile devices. It is
shipped by different distributions (ex. postmarketOS, Manjaro, Neon),
and can run on the devices that are supported by the distribution. Most
of the testing is done on the pinephone, as it is easier for developers 
to obtain and develop on. However, depending on your distribution, you 
may be able to install it on your device!

**Note:** Halium support has been dropped from Neon images due to a lack of developers working on it.

Based on
the `Halium <https://halium.org/>`__ project, we have an official
installable prototype for the Nexus 5X device. However, Plasma Mobile
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

Development Plasma Mobile images based KDE Neon Git-Unstable
are provided. You can also consider using other distributions, like
postmarketOS and Manjaro ARM.

We mainly test and support the Pinephone at
this time, but you can potentially install it on any device supported by
Halium. These Halium images are where most current development is
happening, and scripts are supplied to ease installation of the Plasma
Mobile rootfs onto a Nexus 5X device. `See further instructions
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
display. GTK software can also run as well, though for mobile optimization
they will need to have implemented libhandy. If possible, use software 
built with Kirigami for best results.

A list of most known issues with the current "edge" rootfs can be found
`here <https://notes.kde.org/public/plamo-testing>`__ and
`here. <https://phabricator.kde.org/tag/plasma%3A_mobile/>`__

Where can I find...
~~~~~~~~~~~~~~~~~~~

The code for various Plasma Mobile components can be found on
`invent.kde.org <https://invent.kde.org/plasma-mobile>`__.
See `Get the source code <https://docs.plasma-mobile.org/Code.html>`__ for the individual repositories.

You can also ask your questions in the `Plasma Mobile community groups
and channels <https://www.plasma-mobile.org/join/>`__.
