How to install Plasma Mobile with MultiROM
==========================================

MultiROM is useful for installing multiple ROM's on your android device,
more information can be found on

-  http://forum.xda-developers.com/google-nexus-5/orig-development/mod-multirom-v24-t2571011
-  https://play.google.com/store/apps/details?id=com.tassadar.multirommgr

You can also install multiple version of the Plasma Mobile, so this way
you can keep one copy for development, and one copy which is stable to
demonstrate and show off shiny plasma.

Tested devices
--------------

(If you test it with another devices please add it in following list)

-  LG Nexus 5 (hammerhead)

Prerequisites
-------------

This guide assumes that you have kernel_kexec patch applied as mentioned
in above links. You can skip installing MultiROM manager application and
TWRP recovery for now. (Keep reading)

Install patched MultiROM manager
--------------------------------

Currently MultiROM manager hard codes the image server to
system-image.tasemnice.eu, However thanks to user @sedrubal on Github,
MultiROM Manager is patched to use kubuntu.plasma-mobile.org instead of
system-image.tasemnice.eu server.

Download APK from https://user.fablab.fau.de/~ev80uhys/multirommgr/ and
install it using adb.

``adb install «Path to com.tassadar.multirommgr.debug.apk»``

Install Patched TWRP MultiROM recovery
--------------------------------------

MultiROM manager uses the patched TWRP recovery to finalize the
installation of the operating systems. This recovery uses PGP keys to
verify the Ubuntu touch images. However current recovery only have keys
for the http://system-image.tasemnice.eu and
http://system-image.ubports.com. So it also requires patching : Required
patch is :
https://github.com/Tasssadar/Team-Win-Recovery-Project/pull/16/

Since this is not merged yet, I have uploaded patched version at
https://share.kde.org/index.php/s/OIgNSGoymLRyahg

Download it and flash it with fastboot.

| ``adb reboot bootloader``
| ``fastboot flash recovery «path to twrp-kubuntu-keys-final.img»``

Finally install the Plasma Phone
--------------------------------

Reboot to normal Android, open MultiROM manager installed in earlier
step. In the Ubuntu Card, select devel or devel-proposed and click on
install..

It will take while to download. After download is over, it will reboot
to recovery, in recovery it will make real installation process, so
drink a coffee meanwhile.. ;-)

After that it is done installing it will reboot. Before android logo is
shown it will ask you to choose from Internal or utouch_devel, Select
utouch_devel to boot into your plasma.

Tips
----

Enabling developer mode
~~~~~~~~~~~~~~~~~~~~~~~

Enabling developer mode takes a little bit of extra effort, you will
have to download unlocked adbd from
http://people.canonical.com/~ogra/adbd and replace one in /usr/bin with
it

| ``wget ``\ ```http://people.canonical.com/~ogra/adbd`` <http://people.canonical.com/~ogra/adbd>`__
| ``cp adbd /usr/bin/adbd``
| ``sudo chmod 755 /usr/bin/adbd``
