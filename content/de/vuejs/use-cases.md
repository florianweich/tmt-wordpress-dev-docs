---
title: Anwendungsfälle
description: ''
position: 1111
category: Vue.js
cases:
  - Formulare & Validierung
  - Komplexere Suchfunktionen
  - Karten-Darstellungen
  - WP REST API-Integration
  - Integration externer APIs
---

Sollte die Verwendung von [Vanilla JavaScript](https://wiki.selfhtml.org/wiki/Vanilla-JS) (bzw. [TypeScript](https://www.typescriptlang.org/)) nicht mehr ausreichen und komplexere, dynamische Features bei der Realisierung einer WP-Webseite erforderlich sein, bietet sich die Verwendung von [Vue.js](https://vuejs.org/) an.

<alert type="warning">**DON'T!** Bitte bitte niemals mehr jQuery nutzen! Das ist einfach raus.</alert>

## Use Cases

Die Anwendungsfälle für Vue.js sind vielfältig:

<list :items="cases"></list>

Bei der Verwendung ist aber auf jeden Fall das Verhältnis von Aufwand/Nutzen zu bedenken und insbesondere die Nachteile für [SEO](/vuejs/seo) sind zu beachten, sollten komplette Seiteninhalte damit abgebildet werden!

## Beispiele

Anwendung findet Vue.js bereits in folgenden produktiv genutzten Wordpress-Seiten:

- (Geo-)Suchfunktion für Kinos auf [Wagner im Kino](https://www.wagner-im-kino.de/kinos-tickets/)
- Einbindung externer Veranstaltungen auf [Markgrafenkirchen](https://www.markgrafenkirchen.de/veranstaltungen/#/)
- Komplexe Formulareingaben inkl. Validierung in der [UDB Bayreuth](https://udb.bayreuth.de/)
