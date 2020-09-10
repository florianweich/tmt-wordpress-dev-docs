---
title: SEO
description: 'SEO in der Kombination Wordpress +  Vue.js/JavaScript'
position: 1116
category: Vue.js
---

Ein Nachteil der Verwendung von JavaScript-generierten Inhalten _innerhalb_ einer Wordpress-Seite ist, dass diese Client-seitig gerendert werden.

Das bedeutet, dass Inhalte, die mit Vue.js beispielsweise von einer externen API abgerufen werden _noch_ nicht bei der Wordpress-Response des Webservers zur Verfügung stehen.

<alert type="warning">**Achtung!** Vue.js sollte nicht für SEO-relevante Seiteninhalte verwendet.</alert>

## Typische Probleme

### Routing

Wird der [Vue Router](https://router.vuejs.org/) für das interne Routing verwendet, muss dieser bei der Einbettung in Wordpress-Seiten im sog. [Hash-Modus](https://router.vuejs.org/api/#mode) laufen.

Google und andere Suchmaschinen entfernen allerdings den Hash-Teil einer URL und können somit nicht zwischen den Unterseiten in der SPA unterscheiden.

Reine JavaScript/Vue-Applikationen können dies durch Verwendung der [HTML5 History Mode](https://router.vuejs.org/guide/essentials/history-mode.html#html5-history-mode)-API und der entsprechenden Server-Konfiguration leicht ausgleichen.

### Social Media

Werden komplette Inhalte beispielsweise einer externen API mit Vue.js innerhalb einer statischen Wordpress-Webseite gerendert, enthalten diese keine Metatags für Social Media (z.B. `OpenGraph`), da der `head`-Tag bereits vor dem Ausführen des client-seitigen JavaScripts gerendert wurde.

Reine JavaScript/Vue-Applikationen können dies durch [Server Side Rendering](https://vuejs.org/v2/guide/ssr.html) leicht ausgleichen.

<alert type="info">Die Vorteile eines CMS wie Wordpress und einer Single Page Application lassen sich aber natürlich auch kombinieren. Auftritt [JAMstack](https://www.smashingmagazine.com/2020/02/headless-wordpress-site-jamstack/).</alert>

## OK 👍

- Darstellung dynamischer Elemente:
  - Kartenausgaben
  - Bildergalerien
  - Modal/Overlays
- User Interface Elemente:
  - Formulare & Validierung
  - Suchfunktionen

## NOT OK 👎

- Komplette Seiteninhalte
- Kritische, SEO-relevante Informationen
