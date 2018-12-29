This page describes the plans to develop Plasma Mobile from a
high-level. Day-to-day development and planning is done in KDE's
Phabricator system.

Milestones
==========

We have defined a number of Milestones which will be followed "mostly"
in this order. You can follow the process in more detail on the `Plasma
Mobile Workboard <https://phabricator.kde.org/project/view/28/>`__.

Plasma Mobile 0.1 "Prototype" (finished)
----------------------------------------

The Plasma Mobile prototype shows the viability of Plasma on a handheld
device. The prototype boots a reference device, is able to make phone
calls, select contacts from an address book and contains partly
functional outlook on how handset running Plasma could look like.
\ https://www.youtube.com/watch?v=auuQA0Q8qpM\  Plasma Mobile 0.1
provides:

-  Phone stack definition (libhybris if necessary, Frameworks 5 and
   Plasma on top)
-  Basic proof-of-concept Plasma Shell providing the handset UI,
   including:

   -  App launcher
   -  Draggable top panel
   -  Task switcher
   -  Settings application
   -  Proof-of-concept set of apps

Plasma Mobile 1.0 "Feature Phone" (WIP)
---------------------------------------

Plasma Mobile 1.0 provides an end-user ready experience with a minimal,
useful feature set. This includes the underlying OS and plumbing layers,
a workspace to launch and manage apps, some basic system functions to
set up the network, show connection status, etc. Functions that a 1.0
should provide:

-  Answering and initiating phone calls
-  Contacts / Address book
-  Sending and receiving SMS, possibly other IM service as SMS is pretty
   old fashioned
-  Input: good virtual keyboard with several layouts support,
   localization and right-to-left layouts support
-  Hardware functions:

   -  Volume control
   -  Network control (wifi and Mobile), incl. airplane mode

-  Basic settings

   -  Language / Locale
   -  Clock / Timezone
   -  Ringtone / Notification sounds / Do-not-disturb
   -  Mobile network functions and settings

      -  APN
      -  Roaming
      -  Tethering
      -  Data Limits

-  Web browser with basic functions (possibly 3rd party)
-  SDK: A software development kit allowing to hack on Plasma Mobile
   core and 3rd party apps
-  Appstore, installing, updating and removing apps
-  Photo / Video camera: allows recording photos and videos
-  Image / photo gallery
-  Video player

See `Plasma Mobile
1.0 <https://phabricator.kde.org/project/profile/247/>`__.

Plasma Mobile 2.0 "Basic Smartphone"
------------------------------------

Plasma Mobile 2.0 builds upon the basic functionality provided in 1.0,
and provides more functions:

-  Personal Information Management

   -  Email reading and sending
   -  Calendar
   -  Reminders

-  Multimedia

   -  Listening to audio / music

-  Mobile file manager
-  Accessing files through MTP (or equivalent protocol)
-  Applications ecosystem

See `Plasma Mobile
2.0 <https://phabricator.kde.org/project/view/248/>`__.

Plasma Mobile 3.0 "Featured Smartphone"
---------------------------------------

-  Cloud storage integration
-  Games
-  Cool apps
-  Working Android emulation
