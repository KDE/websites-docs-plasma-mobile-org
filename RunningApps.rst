Running Apps
============

This document describes the necessary environment variables and steps
for running and debugging applications in Plasma Mobile.

It is not necessary for end-users to set these. With a recent Plasma
Mobile image, apps can be run from the drawer easily, however this may
be useful for prospective devs working in different environments where
this is not already set.

Environment Setup
-----------------

In order to run applications on the device, you need to set up a few
environment variables:

| ``export QT_QPA_PLATFORM=wayland``
| ``export QT_QPA_PLATFORMTHEME=KDE``
| ``export QT_WAYLAND_DISABLE_WINDOWDECORATION=1``
| ``export XDG_CURRENT_DESKTOP=KDE``
| ``export KSCREEN_BACKEND=QScreen``
| ``export KDE_FULL_SESSION=1``
| ``export KDE_SESSION_VERSION=5``

Manually starting the Plasma shell (or applications)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to run the Plasma phone shell, do

| ``export $(dbus-launch)``
| ``exec /usr/bin/plasmashell -p org.kde.plasma.phone``

If kded5 has crashed, you may have to start it manually:

``/usr/bin/kded5 &``

Debugging Applications
----------------------

-  installation of debug packages and gdb
