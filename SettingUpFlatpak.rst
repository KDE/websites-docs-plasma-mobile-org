Use with precaution

Install
=======

 $ sudo -i

echo "deb http://ppa.launchpad.net/alexlarsson/flatpak/ubuntu xenial
main" > /etc/apt/sources.list.d/alexlarsson-ubuntu-flatpak-xenial.list

apt-key adv --keyserver keyserver.ubuntu.com --recv-keys
C793BFA2FA577F07

apt update

ntpdate pool.ntp.org

apt install flatpak

chmod o+s /usr/lib/flatpak/flatpak-bwrap

flatpak remote-add gnome --from https://sdk.gnome.org/gnome.flatpakrepo

flatpak remote-add gnomeapps --from
https://sdk.gnome.org/gnome-apps.flatpakrepo

flatpak install gnomeapps org.gnome.gedit

Run
===

``flatpak run org.gnome.gedit``

Building flatpaks in Docker
===========================

When building flatpaks in Docker, use a privileged container.
