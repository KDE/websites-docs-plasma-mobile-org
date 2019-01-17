Plasma Settings infrastructure
==============================

Introduction
~~~~~~~~~~~~

This tutorial teaches you how you can load Plasma settings modules into
your app, and create your own modules.

Plasma Settings is an app, much like Plasma Desktop's kcmshell that
shows and loads configuration modules. These configuration modules are
plugins providing a QML package and an optional C++-plugin which exports
custom-written configuration objects as QObject to the declarative
environment.

You can query available modules using the --list argument to
plasma-settings:

::

   $ plasma-settings --list
   kcm_mobile_time ........... Timezone, Date Display
   kcm_mobile_power .......... Lock, Sleep, Timeout
   kcm_mobile_theme .......... Font and Theme
   kaccounts ................. Add Your Online Accounts

You can load an individual module by supplying its plugin name as
argument to active-settings:

::

   plasma-settings org.kde.active.settings.time

will open the plasma-settings app and load the "Time and Date" module on
startup.

Architecture
~~~~~~~~~~~~
The Plasma Settings app consists of a number of parts, a Plasma App,
which loads a QML package providing the chrome for active-settings, a
set of Declarative components which encapsulate loading settings modules
and a set of settings modules, which provide the UI and backend code for
a specific settings domain (i.e. Time and Date, Browser settings, etc.).

Integrating a Settings Module into an App
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to integrate a settings module "inline" into your app, you can
use the SettingsItem component, which comes with the ActiveSettings
declarative plugin. SettingsItem provides a PageStack (from
PlasmaComponents) with a bit of additional API, the module property.
Creating an Item with a settings module is as easy as:

::


   import org.kde.active.settings 0.1 as ActiveSettings
   [...]
   ActiveSettings.SettingsItem {
       id: webSettingsItem
       module: "org.kde.active.settings.time"
       anchors { ... }
   }

In order to speed up loading your app, you will want to lazy-load the
settings module. This is very easy by using the PageStack features that
SettingsItem encapsulates:

::

   import QtQuick.Controls 2.2 as Controls
   import org.kde.active.settings 0.1 as ActiveSettings
   [...]
   ActiveSettings.SettingsItem {
       id: settingsItem
       initialPage: someOtherItem
       anchors { [...] }
   }

   Controls.Button {
       // This button toggles the settings item and someOtherPage
       [...]
       onClicked: {
           if (settingsItem.module != "org.kde.active.settings.web") {
               settingsItem.module = "org.kde.active.settings.web"
           } else {
               // Switching back...
               settingsItem.replace(someOtherItem);
           }
       }
   }

   Item {
       id: someOtherItem
       /* this guy is shown before any module is loaded */
   }

Creating Your Own Settings Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Simple, QML-only Module
-----------------------

Writing a basic ActiveSettings configuration module is as simple as
creating a Plasma Package, using the X-KDE-ServiceTypes
"Active/SettingsModule". The service type registers your package as
settings module, so active-settings will find (and list) it, and so it
can be loaded using the SettingsItem QML binding. A simple active
settings package will look like this:

::

   ├── CMakeLists.txt
   ├── contents
   │   └── ui
   │       └── Web.qml
   └── metadata.desktop

The metadata.desktop file holds the plugin information, which script to
load from the plugin initially, and a bunch of metadata, just like
normal Plasma Packages. A simple metadata.desktop file will look like
this:

::

   [Desktop Entry]
   Name=Web and Browser
   Comment=Settings for history, caching, etc.
   Encoding=UTF-8
   Type=Service
   Icon=preferences-system-network
   X-KDE-ServiceTypes=Active/SettingsModule
   X-KDE-PluginInfo-Author=Sebastian Kügler
   X-KDE-PluginInfo-Email=sebas@kde.org
   X-KDE-PluginInfo-Name=kcm_mobile_web
   X-KDE-PluginInfo-Version=1.0
   X-KDE-PluginInfo-Website=http://plasma-mobile.org
   X-KDE-PluginInfo-Category=Online Services
   X-KDE-PluginInfo-License=GPL
   X-Plasma-MainScript=ui/Web.qml

The interesting bits, specific to active-settings are the plugin name,
the package name and the mainscript. The plugin name is used to find the
package, and will translates to the "module" property of SettingsItem.
Web.qml points to a normal Item { [...] } in a file, normal rules apply
here.

The CMakeLists.txt file takes care of proper installation and will be
needed in order to install and package your settings module. It looks
like this:

::

   kpackage_install_package(package kcm_mobile_web kcms)

Make sure the names of the .desktop files in CMakeLists.txt are correct,
since incorrect names lead to problems finding and loading your package,
or even to conflicts between different modules. In case of doubt check
active-settings --list for already installed modules. After you
installed the plugin (or changed its metadata) you'll need to run
"kbuildsycoca4" in order to update the plugin metainformation cache.

KConfig Bindings
----------------

Active Settings provides declarative bindings for KConfigGroup. This
means that you can instantiate KConfig objects in your QML code, read
and write settings. For many basic use-cases, this provides enough
flexibility to do everything that's needed. The browser settings module
uses this mechanism:

::

   import QtQuick.Controls 2.2 as Controls
   import org.kde.active.settings 0.1 as ActiveSettings

   [...]
   ActiveSettings.ConfigGroup {
       id: adblockConfig
       file: "active-webbrowserrc"
       group: "adblock"
   }
   [...]
   Controls.Switch {
       [...]
       onClicked: adblockConfig.writeEntry("adBlockEnabled", checked);
       Component.onCompleted: checked = adblockConfig.readEntry("adBlockEnabled");
   }

This corresponds to the following snippet in you active-webbrowserrc
config file (for example i ~/.kde4/share/config/):

::

   [adblock]
   adBlockEnabled=true

ConfigGroup will sync() the config file 5 seconds after a
writeEntry(...) call, or on destruction of the module (for example by
loading another module or page into the SettingsItem.

Functions available are:

-  readEntry(key): fetches a stored config value
-  writeEntry(key, value): writes a config value
-  deleteEntry(key): deletes the stored value, resetting the app
   behavior to the default.

If you find yourself needing more advanced features from C++ code, you
can extend your settings module using a C++ plugin. Of course you can
choose to use both, the already provided KConfig bindings, and an
additional plugin.

Extending your Settings Module with with C++
--------------------------------------------

In some cases, you will find a pure declarative settings module too
limited. By extending a settings module with C++ functionality, you can
implement functionality in a C++ plugin, which gets automatically loaded
with your C++ plugin. This loading is done in the SettingsComponent item
provided by the ActiveSettings import. You will usually want to use a
SettingsItem in your code, like in the above example. SettingsItem
encapsulates the module loading mechanism and provides a PageStack
interface. When a new settings module is loaded in the UI (by setting
SettingsItem "module" property, the .desktop file is checked for an
X-KDE-Library entry (X-KDE-Library=kcm_mobile_time in the Time and
Date example).

This loads a small plugin, consisting of two classes:

-  A QObject based class, which registers one or more additional Object
   to the declarative runtime:

::

   K_PLUGIN_FACTORY(TimeSettingsFactory, registerPlugin<TimeSettingsPlugin>();)
   K_EXPORT_PLUGIN(TimeSettingsFactory("active_settings_time"))

   TimeSettingsPlugin::TimeSettingsPlugin(QObject *parent, const QVariantList &list)
       : QObject(parent)
   {
       qmlRegisterType<TimeSettings>();
       qmlRegisterType<TimeZone>();
       qmlRegisterType<TimeSettings>("org.kde.active.settings", 0, 1, "TimeSettings");
   }

The name provided as second argument to K_EXPORT_PLUGIN macro is the one
you specify in you metadata.desktop file as X-KDE-Library.

-  One or more QObject-derived classes which export domain specific
   settings using QProperties, getters and setters.

::

   class TimeSettings : public QObject
   {
       Q_OBJECT

       [...]
       Q_PROPERTY(bool twentyFour READ twentyFour WRITE setTwentyFour NOTIFY twentyFourChanged)

   public:
       TimeSettings();
       virtual ~TimeSettings();

       [...]
       bool twentyFour();

   public slots:
       [...]
       void setTwentyFour(bool t);

   signals:
       [...]
       void twentyFourChanged();

   private:
       TimeSettingsPrivate* d;
   };

The types are basically reimplemented QObjects, which expose settings to
the QML parts of your settings module. `Qt's
documentation <http://doc.qt.nokia.com/4.8-snapshot/qml-extending.html>`__
has more information on how this works exactly.

In your declarative code, you can then import and instantiate these
objects.

::

   import org.kde.active.settings 0.1

   TimeSettings {
       id: timeSettings
   }

   [...]

   PlasmaComponents.Switch {
       id: twentyFourSwitch
       checked: timeSettings.twentyFour
       onClicked : timeSettings.twentyFour = checked
   }

You will typically want to put code for reading the property in the ctor
or getter, and code for writing options, or updating other parts of the
UI, but of course more complex constructions are also entirely possible,
since the settings plugins can basically provide any kind of QML
extensions. When writing to configuration files, you should not forget
to sync(); your KConfigObject, and to make sure that apps pick up the
changed setting, for example by monitoring the configuration file for
changes (watch for the "created()" signal, not for the changed signal,
as KConfig doesn't directly write to the config file, but to a temorary
file and then atomically moves them.) The plugin has minimal build
dependencies, so that providing a settings plugin along with your app is
very easy.

You can have a look into the
`modules <https://invent.kde.org/kde/plasma-settings/tree/master/modules>`__
directory of Plasma Settings to get some inspiration, or a functioning
base for modules to play around with.
