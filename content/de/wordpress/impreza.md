---
title: Impreza
description: 'Keiner schmeckt mir so wie diesaaa'
position: 114
category: Wordpress
---

Bei TMT kommt das Wordpress-Theme [Impreza](http://impreza-landing.us-themes.com/) als Basis der meisten WP-Webseiten zum Einsatz. Es kommt mit einer recht guten Unterstützung für [Custom Post Types](/wordpress/cpt) und [Advanced Custom Fields](/wordpress/acf) daher.

Auch ohne eigenes Plugin/PHP-Code lassen sich bereits sehr individuelle Inhaltsvorlagen ("Content templates") erstellen, Archivseiten und -layouts ("Grid layouts") generieren und Filterfunktionen ("Grid filter") integrieren.

<alert type="info">**Tipp!** Für simple Datenstrukturen und Ausgaben reichen die Impreza-Bordmittel oft schon aus.</alert>

## Plugin-Integration

Vor allem für komplexere Filter- und Suchfunktionen sowie umfangreichere Templates empfiehlt sich aber die Verwendung eines eigenen Plugins. Für die Integration mit Impreza und dessen eigenen Ausgabe-Funktionen (s. oben) sind aber einige Punkte zu beachten.

<alert type="warning">**Achtung!** Um die Verwendung mit Impreza möglichst einfach und Update-sicher zu gestalten, weichen die hier dokumentierten Vorgehensweisen stark von den gängingen Wordpress-Methoden für Theme-Entwicklung ab.</alert>

### Shortcodes

Da mit Impreza ein Theme verwendet wird, dass stark auf den [WBBakery Page Builder](https://wpbakery.com/) sowie eigene Template Builder für Custom Post Types und Archivseiten setzt, empfiehlt sich häufig die Verwendung von [Wordpress-Shortcodes](https://codex.wordpress.org/Shortcode_API) um eigene Funktionen und Ausgaben zu "injizieren".

Im Grunde werden Shortcodes beim Rendern der Seite durch eigenes HTML und Logik/Funktionen ersetzt. Aus `[custom_search]` wird so also in der finalen Ausgabe ein eigenes Suchformular aus unserem Plugin, das man wiederum frei beispielsweise innerhalb einer Archivseite positionieren kann. Die Liste der `Posts` selbst kann dann wiederum durch Impreza mit Hilfe eines Grid Layouts erfolgen.

Der Vorteil: Unsere Funktionen sind dadurch sehr modular integrierbar. Impreza selbst ist sehr modular aufgebaut und Wordpress mit seinen [Hooks](https://developer.wordpress.org/plugins/hooks/) funktioniert ebenfalls nach einem Baukasten-Prinzip.

<alert type="info">**Tipp!** Eigene WBBakery Page Builder-Elemente lassen sich ebenfalls programmieren und bieten erweiterte bzw. benutzerfreundlichere Eingabemöglichkeiten und sind im Grunde auch nur Shortcodes.<br/>Mehr dazu [hier](https://wpbakery.com/features/extend-wpbakery-page-builder/).</alert>

> Mehr zur Verwendung von [Shortcodes](/wordpress/shortcodes).

### Archivseiten

Hat man eine Archiv-Ausgabe beispielsweise mit Impreza erstellt, lassen sich über Wordpress-Hooks leicht Filterfunktionen oder andere Anpassungen realisieren.

Dazu erweitert oder verändert man einfach die Parameter des globalen `WP_Query`-Objekts im entsprechenden Hook.

> Mehr zur Manipulation von [Queries](/wordpress/queries).

## Impreza-Elemente verwenden

Immer mal wieder möchte man Impreza-eigene Elemente bei der Ausgabe nutzen. Beispielsweise benötigt man einen Button innerhalb eines Shortcodes, der den Look and Feel von Impreza haben soll.

<alert type="warning">**Achtung!** Das "Nachbauen" von solchen Elementen in HTML und CSS ist meist fehleranfällig und wenig zukunftssicher, da sich CSS-Klassen oder Strukturen mit jedem Update verändern können.</alert>

> Everything is a shortcode!

Auftritt Impreza als "Render-Sklave":

Da das Theme den [WBBakery Page Builder](https://wpbakery.com/) verwendet, sind diese eigenen Elemente auch "nur" Shortcodes und können wiederum mit Namen und Parametern aufgerufen werden. Die interne Darstellung überlassen wir dann dem Theme.

### Beispiel: Button

Folgendes Code-Beispiel rendert einen Impreza-Button:

```php
do_shortcode(
  '[us_btn
    el_class="my-custom-class"
    label="My Button Title"
    link="' . get_the_permalink() . '"
    icon="fas|fa-home"
  ]',
);
```

Das Resultat ist ein Button mit einer zusätzlichen CSS-Klasse `.my-custom-class`, dem Titel `"My Button Title"` und sogar einem [Font Awesome](https://fontawesome.com/)-Icon.

<alert type="info">**Info!** Will man wissen, wie die internen Shortcodes aufgebaut sind, wirft man am besten einen Blick in die "Classic"-Ansicht des [WBBakery Page Builder](https://wpbakery.com/). Im Text enthalten sind dort die rohen Shortcodes.</alert>
