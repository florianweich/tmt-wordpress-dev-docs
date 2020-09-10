---
title: Vue.js + WP REST API
description: ''
position: 1113
category: Vue.js
---

## Erweiterte Datenquelle

Eine häufige Anwendung der [WP REST API](https://developer.wordpress.org/rest-api/) ist die Darstellung von mehreren Orten auf einer Karte, beispielsweise auf Archiv- und Suchseiten für [Custom Post Types](/wordpress/cpt).

Gerade wenn mehrere Datensätze auf einmal angezeigt werden sollen, kommt Wordpress gerne mal ins Schwitzen und die Serverlast steigt. Ein Lösungsansatz ist es also Datensätze dynamisch über die REST-API nachzuladen und somit erst nach dem eigentlichen Seitenaufbau darzustellen.

<alert type="warning">**Achtung!** Die WP REST API sollte generell **deaktiviert** und nur einzelne Endpunkte aktiviert werden, die tatsächlich öffentlich verfügbar sein sollen! Unterstützung dafür bietet das Plugin [Disable REST API](https://de.wordpress.org/plugins/disable-json-api/).</alert>

## Advanced Custom Fields

Um die eigenen Felder in der REST API auszugeben, wird ein Plugin benötigt, das diese Funktion nachrüstet. Mehr Informationen [hier](/wordpress/acf#wp-rest-api-integration).

### Datensparsamkeit

Oft ist es **nicht** gewünscht, dass alle zur Verfügung stehenden Daten über die REST-Schnittstelle öffentlich abrufbar sind. Eine client-seitige Authentifizierung ist hier auch wenig sinnvoll, weil die Daten zur Darstellung trotzdem öffentlich verfügbar sein müssen und somit keine User-basierte Auth erfolgen kann. Ein öffentlich hinterlegter Auth-Token bzw. HTTP Basic Authentication wäre also sozusagen der Schlüssel unter der Fußmatte, den jeder finden kann, der möchte.

### Eigener REST-Endpunkt

Die Lösung für oben genanntes Problem ist daher häufig nur die Einrichtung eines eigenen REST-Endpunktes, der dann wirklich nur die Daten liefert, die tatsächlich benötigt werden.

Beispielsweise benötigt eine Liste von Orten auf einer Karte kaum mehr als einen Titel, eine Adresse und Koordinaten.

> Mehr zur Erstellung eines eigenen WP REST-Endpunktes [hier](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/).
