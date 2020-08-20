---
title: Shortcodes
description: ''
position: 115
category: Wordpress
---

Wie bereits im Bereich [Shortcodes](/wordpress/impreza#shortcodes) erwähnt, bietet die Plugin-Integration über Shortcodes einige Vorteile. Auf dieser Seite gibt es dazu einige Tipps und Implementierungs-Beispiele.

## Beispiele

### Archiv-Seite

Auf der Archiv-Seite eines [Custom Post Types](/wordpress/cpt) soll eine angepasste Such- und Filterfunktion integriert werden.

#### 1. Vorbereitungen

In Impreza wurde bereits ein Content-Template für die entsprechende Archivseite angelegt. Am Anfang der Seite wurde dort der Shortcode `[custom_search]` hinterlegt.

Anschließend wird ein Impreza-Grid für den `current_query` verwendet, um die aktuellen Inhalte des Wordpress-Loops auszugeben.

<img src="/impreza_content_template_shortcode.png" alt="Impreza Content-Template mit Shortcode">

#### 2. Shortcode registrieren

Damit der Shortcode auch ausgeführt wird, muss er in Wordpress registriert werden. Gleichzeitig teilt man WP mit, welche Funktion zum Rendern des Shortcodes verantwortlich ist:

```php[includes/class-wp-plugin-name.php]
private function define_public_hooks() {
  /* Render the custom search form */
  add_shortcode('custom_search', [
    $plugin_public,
    'render_shortcode_custom_search',
  ]);
}
```

Hier wird der also Shortcode `[custom_search]` registriert, der beim Rendern die Funktion `render_shortcode_custom_search()` in der `Public`-Klasseninstanz `$plugin_public` aufruft.

#### 3. Ausgabe

Die einfachste Art der Ausgabe ist es, einfach einen String in der Shortcode-Funktion zurückzugeben.

```php[public/class-wp-plugin-name-public.php]
/**
  * Render the shortcode for the custom search form.
  *
  * @since 1.0.0
  */
public function render_shortcode_custom_search() {
  return 'I am just a text!';
}
```

Da aber die gewünschten Inhalte meist komplexer sind, bietet es sich an, eigene Templates/PHP-Dateien für die Ausgaben zu verwenden.

#### 4. Template

Da die Shortcode-Funktion selbst lediglich einen String zurückgeben darf, kann ein [Ausgabepuffer](https://www.php.net/manual/de/ref.outcontrol.php) verwendet werden, um eine PHP-Datei (also ein Template-Partial) als fertig gerenderten String zurückzugeben.

```php[public/class-wp-plugin-name-public.php]
/**
  * Render the shortcode for the custom search form.
  *
  * @since 1.0.0
  */
public function render_shortcode_custom_search() {
  /// EXTENDED LOGIC HERE
  ...

  /// Render template
  $rendered_contents = '';
  ob_start();
  include plugin_dir_path(__FILE__) .
    'partials/wp-plugin-name-public-display.php';
  $rendered_contents = ob_get_contents();
  ob_end_clean();

  return $rendered_contents;
}
```

<alert type="warning">**Achtung!** Das Template-Partial sollte keine Programmlogik enthalten und zu einem großen Teil aus HTML bestehen.</alert>

### Detail-Seite

Auf der Detail-Seite eines [Custom Post Types](/wordpress/cpt) soll eine angepasste Ausgabe eines [ACF](/wordpress/acf)-Feldes integriert werden.

#### 1. Vorbereitungen

In Impreza wurde bereits ein Content-Template für die entsprechende Detailseite angelegt. An der gewünschten Stelle wurde der entsprechende Shortcode `[custom_map]` hinterlegt, der eine Karte ausgeben soll.

Für die restlichen Ausgaben (Titel, Haupttext, einfache ACF-Felder, etc.) werden die Impreza-eigenen Page Builder-Elemente verwendet.

#### 2. Shortcode registrieren und Template rendern

Analog zum obigen Beispiel mit der [Archiv-Seite](#archiv-seite), wird der entsprechende Shortcode wieder registriert und die Render-Funktion ausgeführt.

#### 3. ACF-Felder abfragen

Über die eingebauten Funktionen von Advanced Custom Fields können Felder anhand des Namens abgefragt und im Template verwendet werden.

```php[public/class-wp-plugin-name-public.php]
$coordinates = get_field('location');

if (!empty($coordinates)) {
  /// Do something with the data and render template
} else {
  return 'Kein Ort hinterlegt.';
}
```

<alert type="warning">**Achtung!** Das obige Beispiel benötigt zur Darstellung von Karten natürlich noch JavaScript. Auf das richtige Registrieren ("Enqueueing") von JS- und CSS-Dateien wird im Kapitel [Assets](/wordpress/assets) eingegangen.</alert>

### Archiv-Seite mit verschiedenen CPTs

Impreza bringt von Haus aus lediglich ein Archiv-Template mit.

Sollen also verschiedene Ausgaben innerhalb eines Shortcodes für unterschiedliche Archiv-Typen (also Custom Post Types) benötigt werden, müssen die Archiv-Typen im Code unterschieden werden.

```php[public/class-wp-plugin-name-public.php]
if (is_post_type_archive('cpt_a')) {
  /// Render Custom Post Type A
} elseif (is_post_type_archive('cpt_b')) {
  /// Render Custom Post Type B
}
```
