---
title: Assets
description: ''
position: 117
category: Wordpress
---

Das _richtige_ Laden von `JavaScript`- und `CSS`-Dateien ist in Wordpress besonders wichtig. Dafür bietet WP einige Funktionen zum Registrieren und "Einreihen" (sog. "Enqueueing") von Skripten und Stylesheets.

Der Vorteil der Registrieren/Einreihen-Vorgehensweise ist, dass sich Abhängigkeiten von Skripten herstellen lassen und Caching-Plugins (wie zum Beispiel [WP Rocket](https://wp-rocket.me/)) Dateien zusammenführen oder beispielsweise auch automatisch minifizieren können, da es eine "offizielle" Liste mit Assets gibt.

Außerdem sollte immer darauf geachtet werden, dass Skripte und Stylesheets wirklich nur geladen werden, wenn sie auch auf der aktuellen Seite benötigt werden.

## Enqueuing

JavaScript-Dateien werden über die Funktion `wp_enqueue_script()` und Stylesheets über die Funktion `wp_enqueue_style()` der "Schlange" für das Laden hinzugefügt.

Das klingt jetzt erst mal etwas seltsam - bedeutet aber lediglich, dass sie der Liste von Assets hinzugefügt werden und somit an der entsprechenden Stelle im `head`- bzw. `body`-Tag ausgegeben werden.

```php[public/class-wp-plugin-name-public.php]
public function enqueue_scripts() {
  wp_enqueue_script(
    $this->plugin_name,
    plugin_dir_url(__FILE__) . 'js/wp-plugin-name-public.js',
    [],
    $this->version,
    false,
  );
}
```

> Mehr Informationen zum Einfügen von CSS & JavaScript gibt es [hier](https://developer.wordpress.org/themes/basics/including-css-javascript/).

<alert type="info">**Info!** Auch hier sollte zwischen Skripten und Styles für den Admin- sowie dem Public-Bereich unterschieden werden. Siehe dazu auch [Projekt-Struktur](/development/laravel-mix#projekt-struktur).</alert>

### Abhängigkeiten

Der dritte Parameter der `wp_enqueue`-Funktionen ist ein Array mit Anhängigkeiten. Darüber lassen sich also andere Skripte oder CSS-Dateien referenzieren und als notwendig für die Ausführung festsetzen.

Das bedeutet einfach gesagt, dass Wordpress dafür sorgt, dass zunächst die Skripte geladen werden, die als Abhängigkeiten definiert wurden und dann erst unser eigenes Skript geladen wird.

Ein weiterer Vorteil dieser Methode ist, dass beispielsweise Drittanbieter-Bibliotheken wie jQuery nicht mehrmals geladen werden, wenn sie bereits für andere Skripte als Abhängigkeit geladen wurden:

```php[public/class-wp-plugin-name-public.php]
wp_enqueue_script(
  $this->plugin_name,
  plugin_dir_url(__FILE__) . 'js/wp-plugin-name-public.js',
  ['jquery'], # Dependencies
  $this->version,
  false,
);
```

> Mehr zu den Funktionen [wp_enqueue_script()](https://developer.wordpress.org/reference/functions/wp_enqueue_script/) und [wp_enqueue_style()](https://developer.wordpress.org/reference/functions/wp_enqueue_style/).

## Bedingtes Laden

Idealerweise sollten keine JS- oder CSS-Dateien durch den Browser geladen werden, deren Inhalte nicht auf der aktuellen Seite benötigt werden.

### Use Case

Ein Shortcode rendert ein relativ umfangreiches Formular zur Dateneingabe, das als Vue.js-Applikation aufgebaut ist. Dieser Shortcode befindet sich lediglich auf einer bestimmten Unterseite.

Der notwendige JavaScript-Code zur Ausführung sollte dann nicht schon auf anderen Seiten oder gar der Startseite geladen werden, da dies die Ladezeiten der Seiten unnötigt verschlechtert.

#### Beispiel

```php[public/class-wp-plugin-name-public.php]
public function enqueue_scripts() {
  global $post; # global object set by Wordpress

  if (
    has_shortcode($post->post_content, 'custom_shortcode')
  ) {
    wp_enqueue_script(
      $this->plugin_name,
      plugin_dir_url(__FILE__) . 'js/custom-vue-app.js',
      ['wp-api'],
      $this->version,
      false,
    );
  }
}
```

Die JS-Datei `custom-vue-app.js` wird hier also nur geladen, wenn der Shortcode `[custom_shortcode]` auf der Seite enthalten ist.
