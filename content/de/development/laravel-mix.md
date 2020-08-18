---
title: Laravel Mix
description: ''
position: 14
category: Dev-Umgebung
advantages:
  - Simplere Konfiguration als Webpack
  - Sinnvolle Voreinstellungen (ES6+, TS, SCSS, Vue.js, etc.)
  - Überwachung von Änderungen beim Speichern
  - Einfache Production-Builds (Minifizierung, Tree-shaking, etc.)
---

[Laravel Mix](https://laravel-mix.com) baut auf Webpack auf und abstrahiert einen Großteil der Anwendungsfälle auf simple Konfigurationen ("reasonable defaults") ohne dabei komplexe Webpack-Configs erstellen zu müssen.

Die Hauptanwendungen dabei sind das "Bündeln" von Assets. Also beispielsweise dem Kompilieren von SCSS zu gewöhnlichem CSS oder das Transformieren von ES6+ JavaScript bzw. TypeScript zu ES5-kompatiblen Code, der auch in älteren Browsern läuft.

Laravel Mix entstammt eigentlich dem Laravel-Projekt, kann aber auch _standalone_ verwendet werden.

<list :items="advantages"></list>

## Projekt-Struktur

Im Projekt-Root sollte ein Ordner `src` angelegt werden, der die unkompilierten Original-Dateien (JavaScript, SCSS, etc.) enhält.

Dieser enthält wiederum Unterordner mit Assets für den Admin- sowie den Public-Bereich.

> s. auch [Projektstruktur](/wordpress/plugin#projektstruktur).

```bash
src/
  admin/
    css/
      plugin-name-admin.scss
    js/
      plugin-name-admin.ts
  public/
    js/
      plugin-name-public.js
```

## Konfiguration

Zur Einrichtung den [Installations-Anweisungen](https://laravel-mix.com/docs/5.0/installation#stand-alone-project) von Laravel Mix folgen.

<alert type="warning">**Wichtig!** Anweisungen für **Standalone**-Installation folgen.</alert>

Hier ein Beispiel für eine `webpack.mix.js`-Konfiguration mit `SCSS`, `TypeScript` und `JavaScript`-Assets:

```js
const mix = require('laravel-mix');

mix
  .ts('src/public/js/plugin-name-public.ts', 'public/js/')
  .sass('src/public/css/plugin-name-public.scss', 'public/css/')
  .js('src/admin/js/plugin-name-public.js', 'admin/js/');
```

Weitere nützliche Funktionen sind beispielsweise:

- Extrahieren von Vendor-Bibliotheken bzw. Code Splitting
- Kopieren von Dateien
- Zusammenfügen von Dateien und Minifikation
- OS-Benachrichtigungen

<alert type="info">**Info!** Bei der Verwendung von TypeScript muss zusätzlich auch eine `tsconfig.json` Konfigurationsdatei angelegt werden.</alert>
