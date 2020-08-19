---
title: WP-Plugin
description: ''
position: 111
category: Wordpress
---

Zum Anlegen eines "leeren" Wordpress-Plugin-Projekts, gibt es ein praktisches Tool zum Erstellen eines Boilerplates (inklusive Vorgaben für Klassen-Namen und Konstanten):  
[WordPress Plugin Boilerplate Generator](https://wppb.me)

Der Generator gibt außerdem einen gut strukturierten Aufbau in `Admin` und `Public`-Klassen sowie weiteren Funktionen für das Laden von `JS/CSS`-Assets und Helfern für Code der beim (De)aktivieren/Deinstallieren des Plugins ausgeführt wird.

## Projekt-Struktur

Das ist die vorgegebene Struktur des Wordpress Plugin Boilerplate Generators.  
Die Kommentare neben den Ordnern und Dateien geben einen groben Überlick über die jeweiligen Funktionen und wo welche Teile des Codes hingehören.

```bash
├── README.md
├── admin/
│   ├── class-wp-plugin-name-admin.php # Admin/Backend-Code
│   ├── css/
│   │   └── wp-plugin-name-admin.css # Nicht direkt editieren!
│   ├── index.php
│   ├── js/
│   │   └── wp-plugin-name-admin.js # Nicht direkt editieren!
│   └── partials/ # Template-Teile
│       └── wp-plugin-name-admin-display.php
├── includes/
│   ├── class-wp-plugin-name-activator.php # Beim Plugin aktivieren
│   ├── class-wp-plugin-name-deactivator.php # Beim Plugin deaktivieren
│   ├── class-wp-plugin-name-i18n.php # Lädt Sprachdateien
│   ├── class-wp-plugin-name-loader.php # Lädt Hooks und Filter
│   ├── class-wp-plugin-name.php # Hooks und Filter registrieren
│   └── index.php
├── index.php
├── languages/ # Übersetzungstabellen für Mehrsprachigkeit
│   └── wp-plugin-name.pot
├── public/
│   ├── class-wp-plugin-name-public.php # Public/Frontend-Code
│   ├── css/
│   │   └── wp-plugin-name-public.css # Nicht direkt editieren!
│   ├── index.php
│   ├── js/
│   │   └── wp-plugin-name-public.js # Nicht direkt editieren!
│   └── partials/ # Template-Teile
│       └── wp-plugin-name-public-display.php
├── src/ # JS/CSS-Assets (s. Kapitel zu Laravel Mix)
├── uninstall.php # Beim Plugin deinstallieren
└── wp-plugin-name.php # Einstiegspunkt
```

### Wichtigste Dateien

Hier eine Übersicht der wichtigsten Dateien mit einer Kurzbeschreibung was hier jeweils reingehört.

#### `/wp-plugin-name.php`

Einstiegspunkt für das Plugin. Enthält neben Wordpress-Metadaten (Titel und Beschreibung des Plugins) unter anderem auch die Versionsnummer.

#### `/admin/class-wp-plugin-name-admin.php`

Enthält Code für den Admin/Backend-Bereich. Hier sollten beispielsweise Custom Post Types oder Advanced Custom Fields registriert werden.

#### `/public/class-wp-plugin-name-public.php`

Enthält Code für den Public/Frontend-Bereich. Hier sollte beispielsweise Shortcode-Code enthalten sein oder Templates geladen werden. Ausgabe-**Logik** gehört hier hin!

#### `/public/partials/`

In diesem Ordner sollten die eigentlichen Templates für die öffentlich sichtbaren Ausgaben enthalten sein. Möglichst **ohne Logik**!

#### `/includes/class-wp-plugin-name.php`

In dieser Datei sollten die Funktionen für die Wordpress `Hooks` und `Filter` sowie `Shortcodes` registriert werden. Eine Unterscheidung zwischen Admin- und Public-Code ist bereits vorgesehen und sollte unbedingt beachtet werden!

#### `/src/`

Enthält die Quellen für JS/TS- und (S)CSS-Assets für das Plugin, s. [Laravel Mix](/development/laravel-mix).

<alert type="warning">**Achtung!** Die JavaScript- und CSS-Dateien in den Ordnern `/public` und `/admin` sollten dementsprechend niemals händisch angefasst werden müssen, sondern stets durch den Build-Prozess von Laravel Mix aktualisiert werden.</alert>
