Ofono Telephony functions
=========================

Plasma Mobile uses ofono to communicate with telephony hardware.
Ofono allows to make calls, send and receive sms and use the mobile broadband network.

Phonesim
~~~~~~~~

Phonesim will add a fake phone modem,
that can be controlled via a Qt based user interface
from which it will be possible to test various aspects of the phone UI:
making calls, receiving, signal strength, send SMS and so on.
It will not generate any real call,
but only make the UI think a SIM is working and that a phone call is in progress.

Usually you can install ofono-phonesim from the package manager of your distribution.
If it's not available, you can compile it from `source <https://git.kernel.org/pub/scm/network/ofono/phonesim.git>`_.

To set up ofono-phonesim for development:

- edit /etc/ofono/phonesim.conf, uncomment everything so that it looks like
  ::

     [phonesim]
     Driver=phonesim
     Address=127.0.0.1
     Port=12345

- start ofonod as root
- start phonesim
  Please note that the binary may be called ofono-phonesim in some distributions.
  ::

     phonesim -p 12345 -gui /usr/share/phonesim/default.xml

- from the oFono source directory, call ./test/enable-modem to bring the modem up, the control UI should come up
- call ``./test/online-modem`` to activate the test phonesim modem


On an actual device
~~~~~~~~~~~~~~~~~~~

Ofono can be controlled (for development purposes), using scripts located in ``/usr/share/ofono/scripts/``.
