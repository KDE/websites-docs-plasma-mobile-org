Some general instructions are provided
`here <https://www.plasma-mobile.org/neon-arch-reference-rootfs/>`__ for
flashing Plasma Mobile onto your device.

Currently, for testing, you may also wish to install a newer version of
Plasma Mobile. **./pm-flash -p edge** will install an
`"edge" <https://images.plasma-mobile.org/edge-rootfs/>`__ rootfs, which
has been rebased on Neon 18.04 and has much newer packages. These are
rebuilt regularly. Before reporting bugs, we recommend you test with
this to see if the issues still occur with the latest code.

After flashing, the script now automatically resizes your root partition
for you, and you can connect to Wi-Fi from inside Plasma Mobile easily.
Extra steps are no longer necessary. If your Plasma Mobile device has a
USB connection to your PC, you can connect to it via SSH with **ssh
phablet@10.15.19.82**

By extension, SFTP is also supported, so if you want to copy a file from
your PC to the device, you can either add it as a remote folder inside
Dolphin, or use something like **scp /path/to/file
phablet@10.15.19.82:/home/phablet**

--------------

`See this page for the current state of Plasma Mobile on the Raspberry
Pi <https://community.kde.org/Raspberry_Pi>`__
