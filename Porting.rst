This guide is aimed to porting Plasma Mobile to ARM devices which
requires Android binary blobs. This guide covers various parts,

-  Kernel, either provided by vendor or thirdparty with config options
   enabled to support LXC containers
-  AOSP/LineageOS base, built with core_tiny target, just base items
-  LXC container with Neon/Ubuntu rootfs containing KWin/Wayland and
   Plasma Mobile

Kernel
------

Kernel needs to support the various config options, you can use
lxc-checkconfig script provided by lxc userspace tools to check if all
required features are enabled, for example:

::

   % CONFIG=arch/arm64/configs/bullhead_defconfig lxc-checkconfig
   --- Namespaces ---
   Namespaces: enabled
   Utsname namespace: enabled
   Ipc namespace: enabled
   Pid namespace: enabled
   User namespace: enabled
   Network namespace: missing
   Multiple /dev/pts instances: enabled

   --- Control groups ---
   Cgroup: enabled
   Cgroup clone_children flag: enabled
   Cgroup device: enabled
   Cgroup sched: enabled
   Cgroup cpu account: enabled
   Cgroup memory controller: enabled
   Cgroup cpuset: enabled

   [..]

Below is the list of the config options,

::

   CONFIG_CGROUP_DEVICE=y
   CONFIG_CPUSETS=y
   CONFIG_CGROUP_MEM_RES_CTLR=y
   CONFIG_CGROUP_PERF=y
   CONFIG_UTS_NS=y
   CONFIG_IPC_NS=y
   CONFIG_USER_NS=y
   CONFIG_PID_NS=y
   CONFIG_DEVPTS_MULTIPLE_INSTANCES=y

AOSP/LineageOS base
-------------------

AOSP/LineageOS base should have all non-required things removed and some
changes in base to accomodate needs of libhybris and container
containing Linux system. Below is list of various changes,

Export PATH and LD_LIBRARY_PATH to point to lxc userspace tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We use prebuilt lxc userspace tools from `linux containers
CI <https://jenkins.linuxcontainers.org/>`__. Those are installed in
/data partition which by default is not in the PATH android shell looks
binaries into. Modify the system/core/rootdir/init.environ.rc file to
include the path of LXC userspace tools in $PATH and export
LD_LIBRARY_PATH.

::

   project system/core/
   diff --git a/rootdir/init.environ.rc.in b/rootdir/init.environ.rc.in
   index c32337a..9d91d60 100644
   --- a/rootdir/init.environ.rc.in
   +++ b/rootdir/init.environ.rc.in
   @@ -1,6 +1,6 @@
    # set up the global environment
    on init
   -    export PATH /sbin:/vendor/bin:/system/sbin:/system/bin:/system/xbin
   +    export PATH /data/lxc/lxc/bin:/sbin:/vendor/bin:/system/sbin:/system/bin:/system/xbin
        export ANDROID_BOOTLOGO 1
        export ANDROID_ROOT /system
        export ANDROID_ASSETS /system/app
   @@ -8,6 +8,7 @@ on init
        export ANDROID_STORAGE /storage
        export ASEC_MOUNTPOINT /mnt/asec
        export LOOP_MOUNTPOINT /mnt/obb
   +    export LD_LIBRARY_PATH /data/lxc/lxc/lib
        export BOOTCLASSPATH %BOOTCLASSPATH%
        export SYSTEMSERVERCLASSPATH %SYSTEMSERVERCLASSPATH%
        export LD_PRELOAD libsigchain.so%TARGET_LDPRELOAD%

However, LD_LIBRARY_PATH doesn't work for AOSP based android image and
needs investigation. See `phabricator
task <https://phabricator.kde.org/T4941>`__ for more details.

Disable enforced PIE executables in bionic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Currently LXC userspace tools doesn't have executables compiled with PIE
support, See `github issue <https://github.com/lxc/lxc-ci/issues/7>`__
for more information, to workaround this, we patch bionic and disable
the check which enforces only PIE executables.

::

   diff --git a/linker/linker.cpp b/linker/linker.cpp
   index 54867dc..55ca67a 100644
   --- a/linker/linker.cpp
   +++ b/linker/linker.cpp
   @@ -2401,11 +2401,11 @@ static ElfW(Addr) __linker_init_post_relocation(KernelArgumentBlock&amp; args, ElfW(
      si-&gt;dynamic = nullptr;
      si-&gt;ref_count = 1;
    
   -  ElfW(Ehdr)* elf_hdr = reinterpret_cast&lt;ElfW(Ehdr)*&gt;(si-&gt;base);
   -  if (elf_hdr-&gt;e_type != ET_DYN) {
   -    __libc_format_fd(2, &quot;error: only position independent executables (PIE) are supported.\n&quot;);
   -    exit(EXIT_FAILURE);
   -  }
   +  //ElfW(Ehdr)* elf_hdr = reinterpret_cast&lt;ElfW(Ehdr)*&gt;(si-&gt;base);
   +  //if (elf_hdr-&gt;e_type != ET_DYN) {
   +  //  __libc_format_fd(2, &quot;error: only position independent executables (PIE) are supported.\n&quot;);
   +  //  exit(EXIT_FAILURE);
   +  //}
    
      // Use LD_LIBRARY_PATH and LD_PRELOAD (but only if we aren't setuid/setgid).
      parse_LD_LIBRARY_PATH(ldpath_env);

This patch needs to be dropped when LXC have proper PIE support.

Disable SELinux
~~~~~~~~~~~~~~~

SELinux and optionally audit as well needs to be disabled to run
container on Android. For that edit BOARD_KERNEL_CMDLINE in the
BoardConfig.mk file of device tree to pass
androidboot.selinux=permissive and selinux=0.

::

   BOARD_KERNEL_CMDLINE := console=tty0 androidboot.hardware=hammerhead user_debug=31 maxcpus=2 msm_watchdog_v2.enable=1 androidboot.bootdevice=msm_sdcc.1 androidboot.selinux=permissive

Remove the nosuid,nodev option from the data partition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Android by default mounts /data partition with nosuid,nodev options and
this results in problem when using executables with suid in rootfs,
given it is put inside /data partition. To fix this, we change
fstab.devicename in devicetree to remoev nosuid and nodev option.

::

   diff --git a/fstab.hammerhead b/fstab.hammerhead
   index a582221..39455c1 100644
   --- a/fstab.hammerhead
   +++ b/fstab.hammerhead
   @@ -4,7 +4,7 @@
    # specify MF_CHECK, and must come before any filesystems that do specify MF_CHECK
    
    /dev/block/platform/msm_sdcc.1/by-name/system       /system         ext4    ro,barrier=1                                                    wait
   -/dev/block/platform/msm_sdcc.1/by-name/userdata     /data           ext4    noatime,nosuid,nodev,barrier=1,data=ordered,nomblk_io_submit,noauto_da_alloc,errors=panic wait,check,encryptable=/dev/block/platform/msm_sdcc.1/by-name/metadata
   +/dev/block/platform/msm_sdcc.1/by-name/userdata     /data           ext4    noatime,nodev,barrier=1,data=ordered,nomblk_io_submit,noauto_da_alloc,errors=panic wait,check,encryptable=/dev/block/platform/msm_sdcc.1/by-name/metadata
    /dev/block/platform/msm_sdcc.1/by-name/cache        /cache          ext4    noatime,nosuid,nodev,barrier=1,data=ordered,nomblk_io_submit,noauto_da_alloc,errors=panic wait,check
    /dev/block/platform/msm_sdcc.1/by-name/persist      /persist        ext4    nosuid,nodev,barrier=1,data=ordered,nodelalloc,nomblk_io_submit,errors=panic wait
    /dev/block/platform/msm_sdcc.1/by-name/modem        /firmware       vfat    ro,shortname=lower,uid=1000,gid=1000,dmask=227,fmask=337,context=u:object_r:firmware_file:s0        wait

Apply bionic patch to shift TLS slots
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

TLS slots used by the bionic conflicts with the libc/hybris, this
results in OpenGL and/or Qt based applications crashing, to fix this
Ubuntu Touch and Mer uses
`patch <https://code-review.phablet.ubuntu.com/#/c/4/>`__ to not cause
conflicts between libc and bionic. Apply this patch to bionic. If linked
patch doesn't cleanly apply to your bionic checkout, required changes
are simple enough to apply manually.

Fix Permission for backlight brightness file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default brightness sysfs file permission doesn't allow other user to
change brightness, this results in kwin failing to turn backlight on and
off. To fix that in init.devicename.rc change the permission as shown
below:

::

   diff --git a/init.hammerhead.rc b/init.hammerhead.rc
   index a08d015..e34c563 100644
   --- a/init.hammerhead.rc
   +++ b/init.hammerhead.rc
   @@ -138,7 +138,7 @@ on boot
        chown system system /sys/class/leds/green/on_off_ms
        chown system system /sys/class/leds/blue/on_off_ms
        chown system system /sys/class/leds/red/rgb_start
   -    chmod 664 /sys/class/leds/lcd-backlight/brightness
   +    chmod 666 /sys/class/leds/lcd-backlight/brightness
    
    on post-fs-data
        write /sys/kernel/boot_adsp/boot 1

LXC container and Neon/Ubuntu rootfs
------------------------------------

LXC userspace tools
~~~~~~~~~~~~~~~~~~~

Once minimal android system is functional (you can boot into it). Next
step is to get LXC userspace tools and the rootfs for Plasma Mobile, LXC
userspace tools can be downloaded from `Linuxcontainers
CI <https://jenkins.linuxcontainers.org/job/lxc-build-android/lastSuccessfulBuild/artifact/lxc-android.tar.gz>`__.
Once downloaded, you need to extract it in / of the device.

::

   adb push lxc-android.tar.gz /data/
   adb shell tar xf /data/lxc-android.tar.gz

Once done, verify from adb shell that LXC userspace tools are functional
by running ``lxc-start`` in adb shell and see if it prints help for it.

Container configuration
~~~~~~~~~~~~~~~~~~~~~~~

We use the following configuraiton for the Neon/Ubuntu container
containing Plasma Mobile system,

::

   lxc.rootfs = /data/lxc/containers/system/rootfs
   lxc.utsname = armhf

   lxc.network.type = none
   lxc.mount.auto = cgroup

   lxc.devttydir = lxc
   lxc.pts = 1024
   lxc.arch = armhf

   lxc.kmsg = 0
   # todo, bind only required stuff
   lxc.mount.entry = /dev dev/ none bind,optional,create=dir
   lxc.mount.entry = /system system/ none bind,optional,create=dir
   lxc.mount.entry = /vendor vendor/ none bind,optional,create=dir

This configuration goes into /data/lxc/containers/system/config file.

Neon/Ubuntu rootfs
~~~~~~~~~~~~~~~~~~

For reference images we use the Neon/Ubuntu based rootfs which uses the
various packages rebuilt weekly. Packages are built on `Mobile Neon
CI <http://mobile.neon.pangea.pub:8080/>`__ and is distributed as
`Ubuntu package archive <http://neon.plasma-mobile.org:8080/>`__. This
packages are then used to build the rootfs images. rootfs images are
built using live-config available `on
github <https://github.com/plasma-phone-packaging/live-config>`__.
Rootfs images are built weekly at every Thursday and if new images
passes QA then is available at
`neon.plasma-mobile.org <http://neon.plasma-mobile.org/rootfs/>`__.

You need to download the latest rootfs from there and extract it to
/data/lxc/containers/system/rootfs/

::

   adb push plasma-rootfs.tar.gz /data/plasma-rootfs.tar.gz
   adb shell mkdir -p /data/lxc/containers/system/rootfs/
   adb shell tar xf /data/plasma-rootfs.tar.gz -C /data/lxc/containers/system/rootfs/
   adb shell rm /data/plasma-rootfs.tar.gz

Note that initially container is not autostarted and you will need to
start it manually using following command,

::

   lxc-start -n system -F

This will give you login prompt and you can login using phablet user.

Starting Plasma Mobile
~~~~~~~~~~~~~~~~~~~~~~

LXC container is configured to autostart forked version of sddm, which
autologins into phablet user and starts KWin/Wayland which in turn
starts the Plasma Mobile shell. KWin/Wayland have hwcomposer backend
which uses the libhybris to initalize the OpenGL. However if for some
reason KWin/Wayland can't initalize the HWcomposer it will exit. To
figure out if hwcomposer is functional you can run test_hwcomposer
command with EGL_PLATFORM=hwcomposer environment variable set.

::

   $ EGL_PLATFORM=hwcomposer test_hwcomposer

If test_hwcomposer fails to run, you can run it under strace for
debugging. If you need help with making it work, please contact `Plasma
devel mailing list <mailto:plasma-devel@kde.org>`__ with strace output.

TODO: document common problems here
