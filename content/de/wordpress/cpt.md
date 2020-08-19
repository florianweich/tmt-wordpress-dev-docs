---
title: Custom Post Types
description: ''
position: 112
category: Wordpress
examples:
  - Job-Bereiche
  - Veranstaltungen
  - Orts- und Adress-Listen
  - Produkt-Datenbanken
---

Ein bei TMT häufig verwendetes Feature zur strukturierten Erweiterung von Wordpress-Webseiten sind [Custom Post Types](https://wordpress.org/support/article/post-types/#custom-post-types).

CPTs erlauben es eigene Typen von Posts anzulegen, die abgesetzte Menü- und Eingabestrukturen sowie angepasste Ausgaben/Templates haben. Somit lassen sich beispielsweise kleine Datenbanken innerhalb einer Wordpress-Webseite aufbauen.

Typische Anwendungsbeispiele sind etwa:

<list :items="examples"></list>

## Taxonomien und Terms

Taxonomien sind in der Wordpress-Welt im Grunde Kategorien, die mit einem bestimmten Post Type verknüpft werden können und es somit ermöglichen beispielsweise Filter aufzubauen oder Kategorie-Archive zu erstellen.

Die Taxonomie ist dabei die übergordnete Einheit (z.B. "Berufe"), ein `Term` ist dann die jeweils ausgewählte Instanz (z.B. "Webentwickler"). Eine Mehrfachauswahl ist ebenfalls möglich (wenn konfiguriert).

<alert type="warning">**Achtung!** Nach Möglichkeit sollten für Custom Post Types eigene Taxonomien angelegt werden, auch wenn es möglich ist, die Wordpress-eigenen zu verweden.</alert>

## CPT UI

Das Wordpress-Plugin [Custom Post Types UI](https://de.wordpress.org/plugins/custom-post-type-ui/) kann bei der Entwicklung/Erstellung von Custom Post Types eine enorme Hilfe sein. Es bietet ein User Interface zum Anlegen und Bearbeiten von CPTs im Wordpress-Backend und kann außerdem noch Taxonomien erzeugen.

Gerade im Anfangsstadium der Entwicklung ist es relativ praktisch alle von Wordpress zur Verfügung stehenden Optionen beim Erstellen von Beitragstypen und Taxonomien zu sehen und mit einem Klick konfigurieren zu können.

Am Ende lassen sich für die so erstellten CPTs und Taxonomien PHP-Code erzeugen, der wiederum direkt ins Plugin aufgenommen werden kann. Das Plugin CPT UI selbst sollte im Produktiv-System _nicht_ aktiviert sein müssen!

<alert type="info">**Info!** Der PHP-Code für Custom Post Types und Taxonomien gehört in die Datei `/admin/class-wp-plugin-name-admin.php` (s. [WP-Plugin](/wordpress/plugin)).</alert>
