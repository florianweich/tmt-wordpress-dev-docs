---
title: Shortcode
description: ''
position: 1112
category: Vue.js
---

## Vue-App mit Shortcode mounten

Eine [Vue.js](https://vuejs.org/)-Applikation benötigt lediglich einen Mount-Punkt wie beispielsweise ein `div` mit einer `ID` um ausgeführt werden zu können. Dementsprechend einfach lässt sich via Shortcode eine Vue-App platzieren.

```php
/**
  * Renders a tiny Vue.js map app.
  */
public function render_shortcode_map() {
  return '<div id="vue_map" style="min-height: 500px;"></div>';
}
```

<alert type="info">**Info!** Durch die Angabe der vorraussichtlichen Höhe dieses Divs können [Layout Shifts](https://web.dev/optimize-cls/) durch das Laden der Karte vermieden werden.</alert>

### Erw. Beispiel mit Attributen

Durch Shortcode-Attribute können natürlich leicht Informationen über das CMS zur JS-Anwendung übermittelt werden. Durch die Ausgabe als globale Variable stehen die Daten dann in der JavaScript-Umgebung zur Verfügung.

```php
/**
  * Renders a tiny Vue.js map app with passed information.
  */
public function render_shortcode_map($atts) {
  /// Access shortcode attrs
  $attributes = shortcode_atts(
    [tmt_tile_server_api_key => ''],
    $atts,
    'vue_map',
  );

  /// Grab API Key
  $apiKey = $attributes['tmt_tile_server_api_key'];

  /// Build HTML markup
  $markup = '<script>window.tmtTileServerApiKey = ';
  $markup .= json_encode($apiKey) . ';</script>';
  $markup .= '<div id="bier_map"style="min-height: 500px;"></div>';

  return $markup;
}
```

<alert type="warning">**Achtung!** So sollten natürlich keine Authentifizierungs- oder andere sensible Daten übermittelt werden, da sie so frei zugänglich im Quelltext der Seite enthalten sind!</alert>
