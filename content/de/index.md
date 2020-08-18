---
title: Einleitung
description: ''
position: 1
category: Übersicht
features:
  - lokale Dev-Umgebung auf Docker-Basis
  - Visual Studio Code-Unterstützung mit Xdebug-Integration
  - Build-Prozess mit Laravel Mix (JavaScript/TypeScript, SCSS, Vue.js, etc.)
  - Integration von Advanced Custom Fields PRO
  - Vue.js + WP REST API
  - Impreza-freundliche Anwendung
---

In diesem Dokument wird der Entwicklungs-Prozess für Wordpress-Plugins mit Custom Post Types und [Adanced Custom Fields PRO](https://www.advancedcustomfields.com/)-Integration dokumentiert.

Die Vorgehensweise ist dabei vor allem für die Verwendung innerhalb einer [Impreza](http://impreza-landing.us-themes.com/)-Installation optimiert. Dies ist ein Wordpress-Theme, das standardmäßig bei Kundenprojekten von [TMT](https://www.tmt.de/) zum Einsatz kommt.

Client-seitige JS-Erweiterungen der statischen Ausgaben werden dabei vor allem mit [Vue.js](https://vuejs.org/) realisiert. Kleinere Skripte sind nach Möglichkeit immer in [TypeScript](https://www.typescriptlang.org/) zu schreiben.

## Features

<list :items="features"></list>

Im nächsten Kapitel folgt das Setup der lokalen Entwicklungs-Umgebung.
