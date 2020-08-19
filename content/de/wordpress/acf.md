---
title: Advanced Custom Fields
description: ''
position: 113
category: Wordpress
advantages:
  - Erweiterte Datentypen für eigene Felder
  - Benutzerfreundliche Eingabe-UI
  - Gute Integration in Impreza
  - Gute Integration in die WP REST API
  - Sehr gute Dokumentation
---

[Advanced Custom Fields](https://www.advancedcustomfields.com/) ist ein Plugin, das einige sehr praktische Funktionserweiterungen für die Wordpress-eigenen Benutzerdefinierten Feldern mit sich bringt.

<alert type="info">**Info!** Bei TMT kommt die PRO-Version mit erweitertem Funktionsumfang zum Einsatz (Repeater Field, Gallery, Options Pages).</alert>

## Vorteile

ACF (PRO) bringt im Grunde erweiterte Datentypen mit sich (z.B. Datumsfelder, Google Maps oder Bildergalerien) und stellt benutzerfreundliche Eingabemöglichkeiten im Backend zur Verfügung. Außerdem ist das Plugin recht gut in [Impreza](/wordpress/impreza) integriert und kann so leicht mit dem [WPBakery Page Builder](https://wpbakery.com/) verwendet werden.

<list :items="advantages"></list>

### Native Wordpress-Integration

Die eingegeben Daten werden direkt oder serialisiert als gewöhnliches, Wordpress-eigenes Custom Field gespeichert. Die Ausgabe in einem PHP-Template erfolgt über ACF-eigene Funktionen, die ebenfalls sehr gut und umfangreich dokumentiert sind.

> Zur ACF (PRO) Dokumentation geht es [hier](https://www.advancedcustomfields.com/resources/).

<alert type="warning">**Achtung!** Da ACF oft kritisch für die Ausführung des eigenen Codes ist, empfiehlt sich ein früher Check ob es aktiviert ist (z.B. am Einstiegspunkt unseres Plugins).</alert>

## Code-Generator

Ähnliche wie [CPT UI](/wordpress/cpt#cpt-ui) bringt auch Advanced Custom Fields einen Generator mit, der PHP-Code für das Plugin erzeugen kann.

Auch hier gilt, dass ACF im Produktiv-System keine editibaren Feldgruppen mehr enthalten sollte. Stattdessen gehören die Feld-Definitionen direkt ins Plugin.

<alert type="info">**Info!** Der PHP-Code für Advanced Custom Fields gehört in die Datei `/admin/class-wp-plugin-name-admin.php` (s. [WP-Plugin](/wordpress/plugin)).</alert>

## WP REST API-Integration

Die ACF-Felder sind standardmäßig nicht in der Wordpress REST API integriert. Dies lässt sich aber leicht mit Hilfe des Plugins [ACF to REST API](https://de.wordpress.org/plugins/acf-to-rest-api/) ändern.

> Eine Dokumentation der Erweiterungs- und Anpassungsmöglichkeiten befindet sich auf [GitHub](https://github.com/airesvsg/acf-to-rest-api/).

## Options Pages

Eine sehr praktische Funktion der ACF PRO-Version ist das Anlegen von Einstellungsseiten.

Im Code selbst definiert man lediglich die Position und Metadaten der Options-Seite und befüllt diese dann mit ACF-Feldern (beispielsweise über die Feldgruppen-UI oder auch direkt im Plugin-Code).
Gegenüber der Standard-Methode von Wordpress erspart man sich so einige Schritte (z.B. Aufbau des Eingabeformulars, Validierung, etc.).

> Mehr dazu in der [Dokumentation](https://www.advancedcustomfields.com/resources/options-page/).
