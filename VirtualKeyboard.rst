Virtual Keyboard
================

Maliit is used as virtual keyboard on the device. This document holds
notes how to set it up. Maliit needs plasma-maliit-framework and
plasma-maliit-plugins. You can start Maliit by:

| ``export QT_IM_MODULE=maliit``
| ``export MALIIT_KEYBOARD_DATA_DIR=${PREFIX}/share/maliit/plugins/org/maliit/ # (where ${PREFIX} is where you installed the keyboard plugins to)``
| ``export MALIIT_DEBUG=1``
| ``maliit-server &``

The /etc/xdg/maliit.org/server.conf determines which keyboard is being
used:

| ``[maliit]``
| ``; onscreen\active=libmaliit-keyboard-plugin.so:en_us``
| ``onscreen\active=nemo-keyboard.qml:en_us``
| ``; onscreen\active=libubuntu-keyboard-plugin.so:en_us``
