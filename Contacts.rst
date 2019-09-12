Contacts
========

Introduction
------------

Different apps might need to access or provide contacts.
For this purpose, Plasma Mobile uses the KPeople framework,
which provides read and write access to different sources.
The default KPeople backend is kpeople-vcard, which stores
its contacts in ``~/.local/share/kpeoplevcard``.
Access to this contacts can be restricted by restricting
access to this directory.

Alternative backends currently include kpeople-sink for
accessing remote contacts stored in dav source like Nextcloud.

Models
~~~~~~

KPeople provides Qt models for accessing different
person related data. :kpeopleapi:`PersonsModel` lists
all available contacts. Actions for each contact
can be accessed by providing `KPeople.PersonActions`
with a personUri from the :kpeopleapi:`PersonsModel`.


Person Data
~~~~~~~~~~~

:kpeopleapi:`PersonsModel` and :kpeopleapi:`PersonData`
also provide raw access to the VCard of a contact.
VCard is a standard that is supported on many platforms
and supports storing all kind of information about a person,
including instant messenger contacts, phone number, names etc.
It can be processed using a :kcontactsapi:`VCardConverter`.

Editing contacts
~~~~~~~~~~~~~~~~

To make changes to a contact, its vcard can be edited
by setting the custom property ``vcard`` of
:kpeopleapi:`PersonData` to the new raw vcard data.
New vcard data can be created using the :kcontactsapi:`Addressee` class,
from which the :kcontactsapi:`VCardConverter` can create a vcard.

Adding / Removing
~~~~~~~~~~~~~~~~~

:kpeopleapi:`PersonPluginManager` provides static functions
for adding and removing contacts.

:kpeopleapi:`PersonPluginManager::addContact` can be fed with
a QVariantMap, which has to contain the key "vcard" with vcard data.

:kpeopleapi:`PersonPluginManager::deleteContact` deletes the contact
with the specified uri.
