---
title: Queries
description: ''
position: 116
category: Wordpress
---

Manchmal ist es notwendig eine Wordpress-Query (das betrifft sowohl Archivseiten als auch Suchanfragen) zu verändern bzw. zu erweitern. Beispielsweise kann die Anzahl der dargestellten Ergebnisse pro Seite verändert, Filter basierend auf [Advanced Custom Fields](/wordpress/acf) integriert oder eine Option zur Veränderung der Sortierreihenfolge eingebaut werden.

## Der `pre_get_posts`-Hook

Dieser Wordpress-Hook wird - wie der Name schon sagt - vor jedem Laden von Posts aus der Datenbank ausgeführt und erlaubt es zusätzliche Parameter für den aktuellen Query zu spezifieren.

<alert type="warning">**Achtung!** Da dieser Hook tatsächlich vor **jedem** Query ausgeführt wird, ist es wichtig zu prüfen, ob auch die richtige Abfrage verändert wird.</alert>

## Beispiele

Unsere Funktion zum Verändern der Haupt-Query wird wie andere `Actions` und `Filters` auch zunächst einmal in Wordpress registriert:

```php[includes/class-wp-plugin-name.php]
private function define_public_hooks() {
  $plugin_public = new Plugin_Public(
    $this->get_plugin_name(),
    $this->get_version(),
  );

  $this->loader->add_action(
    'pre_get_posts',
    $plugin_public,
    'alter_main_query',
  );
}
```

Beim Ausführen ist es wichtig zu überprüfen, dass wir auch die richtige Abfrage manipulieren:

```php[public/class-wp-plugin-name-public.php]
public function alter_main_query($query) {
  /// Don't do anything in backend or outside of main query
  if (is_admin() || !$query->is_main_query()) {
    return;
  }
}
```

### Einträge pro Seite verändern

Jetzt können wir beispielsweise leicht die Anzahl der angezeigten Einträge pro Seite für einen bestimmten [Custom Post Type](/wordpress/cpt) verändern:

```php[public/class-wp-plugin-name-public.php]
public function alter_main_query($query) {
  /// Display more items per page
  $posts_per_page = is_post_type_archive('cpt_a') ? 24 : 12;
  $query->set('posts_per_page', $posts_per_page);
}
```

> Eine Übersicht aller verfügbaren Parameter für die `WP_Query`-Klasse gibt es [hier](https://developer.wordpress.org/reference/classes/wp_query/#parameters).

### Nach ACF filtern

Um auf Basis von benutzerdefinierten Feldern (also auch [Advanced Custom Fields](/wordpress/acf)) in Wordpress filtern zu können, gibt es die Möglichkeit eine Meta-Query auszuführen. Diese ist Teil des `WP_Query`-Objekts und kann auch mehrere Unterabfragen enthalten:

```php[public/class-wp-plugin-name-public.php]
public function alter_main_query($query) {
  $meta_query = [];

  /// ACF meta query
  $filter_value = get_query_var('custom_filter');

  if (!empty($filter_value)) {
    $meta_query[] = [
      'key' => 'custom_field_key',
      'value' => $filter_value,
      'compare' => 'LIKE',
    ];
  }

  $query->set('meta_query', $meta_query);

  return $query;
}
```

> Mehr zum Thema ACF und Meta Queries gibt es in der [ACF Dokumentation](https://www.advancedcustomfields.com/resources/query-posts-custom-fields/).

<alert type="info">**Info!** Die oben verwendete Funktion `get_query_var()` setzt voraus, dass eine der Standard-Abfragevariablen von Wordpress verwendet oder eine eigene Variable vorab registriert wurde. Mehr dazu [hier](https://developer.wordpress.org/reference/functions/get_query_var/#custom-query-vars).</alert>

## Erweiterte Queries

Queries in Wordpress lassen sich leicht erweitern und anpassen. Hier noch einige Ansätze für spezielle Queries, die so nicht in WP enthalten sind, und wie sie sich nachrüsten lassen.

### Umkreissuche

Ein oft nachgefragtes Feature ist die Umkreissuche bzw. Geo-Abfrage. Dabei werden Datensätze um einem bestimmten Ort herum innerhalb eines vorgegebenen Radius gesucht.

Wichtigste Vorraussetzung dafür ist, dass jeder Datensatz Koordinaten enthält, die für eine Geo-Query genutzt werden können. [Advanced Custom Fields](/wordpress/acf) bietet mit dem [Google Map](https://www.advancedcustomfields.com/resources/google-map/)-Feld eine einfache Möglichkeit zur Eingabe. Adressdaten werden dabei automatisch geokodiert mit Breiten- und Längengrad abgespeichert.

Die Geo-Suchfunktion selbst lässt sich mit dem Plugin **[Geo Query](https://github.com/birgire/geo-query)** nachrüsten.

<alert type="info">**Info!** Wenn `Query Vars` für Benutzereingaben verwendet werden sollen (z.B. Ausgangskoordinaten) müssen diese vorab registriert werden. Mehr dazu [hier](https://developer.wordpress.org/reference/functions/get_query_var/#custom-query-vars).</alert>
