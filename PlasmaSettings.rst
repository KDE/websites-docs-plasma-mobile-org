Settings module development
===========================

Introduction
~~~~~~~~~~~~

Settings in Plasma are provided by Configuration Modules (KCM). These can be loaded by multiple wrapper applications
such as ``systemsettings5`` on the desktop, ``plasma-settings`` on mobile or ``kcmshell5`` for standalone config windows.

You can query the available KCMs using ``kcmshell5 --list``. To load an individual module in a standalone window pass its
name to kcmshell5, e.g. ``kcmshell5 kcm_kaccounts``. To open it in plasma-settings use the ``-m`` parameter, e.g. ``plasma-settings -m kcm_kaccounts``.

Basic KCM
~~~~~~~~~

KCMs consist of a KPackage holding the QML UI and a C++ library holding the logic. Some legacy KCMs are based on QtWidgets,
however this is not recommended for new KCMs and it's not possible to load these in ``plasma-settings``.

The basic structure of a hypothetical time settings module is the following:

::

   ├── CMakeLists.txt
   ├── timesettings.{cpp,h}
   └── package
       ├── metadata.desktop
       └── contents/ui
           └── Time.qml


CMakeLists.txt
--------------
.. code-block:: cmake

    set(timesettings_SRCS # Specify source files for the library
        timesettings.cpp
    )

    add_library(kcm_time MODULE ${timesettings_SRCS})

    target_link_libraries(kcm_time
        Qt5::Core
        KF5::CoreAddons
        KF5::I18n
        KF5::QuickAddons
    )

    kcoreaddons_desktop_to_json(kcm_time "package/metadata.desktop")

    install(TARGETS kcm_time DESTINATION ${KDE_INSTALL_PLUGINDIR}/kcms) # Install the library to the kcm location

    install(FILES package/metadata.desktop RENAME timesettings.desktop DESTINATION ${KDE_INSTALL_KSERVICES5DIR}) # Install the desktop file
    kpackage_install_package(package kcm_time kcms) # Install our QML kpackage.

timesettings.h
--------------
.. code-block:: c++

    /*
        SPDX-FileCopyrightText: Year Author <author@domanin.com>
        SPDX-License-Identifier: GPL-2.0-or-later
    */

    #ifndef TIMESETTINGS_H
    #define TIMESETTINGS_H

    #include <KQuickAddons/ConfigModule>

    class TimeSettings : public KQuickAddons::ConfigModule
    {
        Q_OBJECT
    public:
        TimeSettings(QObject *parent, const QVariantList &args);
        ~TimeSettings() override;

    public Q_SLOTS:
        void defaults() final;
        void load() final;
        void save() final;
    };

    #endif

:kdeclarativeapi:`KQuickAddons::ConfigModule` serves as the base class for all QML-based KCMs. Please consult the API documentation for a full description.

timesettings.cpp
----------------
.. code-block:: c++

    /*
        SPDX-FileCopyrightText: Year Author <author@domain.com>
        SPDX-License-Identifier: GPL-2.0-or-later
    */

    #include "timesettings.h"

    #include <KPluginFactory>
    #include <KLocalizedString>
    #include <KAboutData>

    K_PLUGIN_CLASS_WITH_JSON(TimeSettings, "metadata.json")

    TimeSettings::TimeSettings(QObject *parent, const QVariantList &args)
        : KQuickAddons::ConfigModule(parent, args)
    {
        KAboutData *aboutData = new KAboutData("kcm_time",
                                            i18nc("@title", "Audio"),
                                            "0.1",
                                            QStringLiteral(""),
                                            KAboutLicense::LicenseKey::GPL_V2,
                                            i18nc("@info:credit", "Copyright Year Author"));

        aboutData->addAuthor(i18nc("@info:credit", "Author"),
                            i18nc("@info:credit", "Author"),
                            QStringLiteral("author@domain.com"));

        setAboutData(aboutData);
        setButtons(Help);
    }

    TimeSettings::~TimeSettings()
    {
    }

    void TimeSettings::load()
    {
    }

    void TimeSettings::save()
    {
    }

    void TimeSettings::defaults()
    {
    }

    #include "timesettings.moc"

package/metadata.desktop
------------------------
::

    [Desktop Entry]
    Name=Time
    Comment=Configure Time
    Encoding=UTF-8
    Type=Service
    Icon=preferences-system-time
    X-KDE-Library=kcm_time
    X-KDE-ServiceTypes=KCModule
    X-KDE-FormFactors=desktop,handset,tablet,mediacenter
    X-Plasma-MainScript=ui/Time.qml
    X-KDE-System-Settings-Parent-Category=personalization
    X-KDE-Keywords=Time,Date,Clock
    X-KDE-ParentApp=kcontrol

Example metadata file

| ``Name`` defines the name of the KCM which is shown in the settings app.
| ``Description`` is a short, one sentence description of the module.
| ``X-KDE-Library`` must match the library name defined in CMakeLists.txt.
| ``X-KDE-FormFactors`` defines on which kinds of devices this KCM should be shown.
| ``X-Plasma-MainScript`` points to the main QML file in the KPackage.
| ``X-KDE-System-Settings-Parent-Category`` defines the category systemsettings5 is showing the module in.
| ``X-KDE-Keywords`` defines Keywords used for searching modules.

package/contents/ui/Time.qml
----------------------------
::

    /*
        SPDX-FileCopyrightText: 2020 Nicolas Fella <nicolas.fella@gmx.de>
        SPDX-License-Identifier: GPL-2.0-or-later
    */

    import QtQuick 2.12
    import QtQuick.Controls 2.12 as Controls

    import org.kde.kirigami 2.7 as Kirigami
    import org.kde.kcm 1.2

    SimpleKCM {

        Controls.Label {
            text: "Configure Time"
        }
    }

Basic KCM QML file

Depending on the content use one of the following root type:
    - Use :kdeclarativeapi:`ScrollViewKCM` for content that is vertically scrollable, such as ListView.
    - Use :kdeclarativeapi:`GridViewKCM` for arranging selectable items in a grid.
    - Use :kdeclarativeapi:`SimpleKCM` otherwise.

Multi-page KCM
~~~~~~~~~~~~~~

KCMs can consist of multiple pages that are dynamically opened and closed. To push another page use
::

    kcm.push("AnotherPage.qml")

AnotherPage.qml should have one of the above types as root element. To pop a page use
::

    kcm.pop()
