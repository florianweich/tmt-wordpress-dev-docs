---
title: Docker
description: ''
position: 11
category: Dev-Umgebung
requirements:
  - Getrennte Installationen mit docker-compose
  - Xdebug-Integration
  - 'Optional: Datenbank-Verwaltungstool'
---

Für die lokale Entwicklung und dem Betrieb mehrerer paralleler Wordpress-Installationen eignet sich Docker.

<list :items="requirements"></list>

## docker-compose Stack

<alert type="info">**Info!** Nachfolgende Infos gelten erst Mal vorübergehend. Mittelfristig ist geplant die Produktiv-Systeme bei TMT mit einem eigenen Docker-Image nachzubilden.</alert>

Der einfachste Wordpress-Stack erfordert ein Wordpress-Image (z.B. WP + Apache) sowie ein Datenbank-Image (z.B. MariaDB).  
Optional dazu kann ein Datenbank-Verwaltungstool (z.B. Adminer) hilfreich sein, einen schnellen Blick in die Datenbank zu werden.

```docker[docker-compose.yml]
version: '3.4'

services:
  db:
    image: mariadb:10.1
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: SECRET_HERE
      MYSQL_DATABASE: SECRET_HERE
      MYSQL_USER: SECRET_HERE
      MYSQL_PASSWORD: SECRET_HERE

  wordpress:
    depends_on:
      - db
    image: andreccosta/wordpress-xdebug
    ports:
      - 8000:80
    volumes:
      - ./wp-content:/var/www/html/wp-content
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: SECRET_HERE
      WORDPRESS_DB_PASSWORD: SECRET_HERE
      XDEBUG_CONFIG: remote_host=host.docker.internal

#  adminer:
#    depends_on:
#      - db
#    image: adminer
#    ports:
#      - 8001:8080

volumes:
  db_data:
```

<alert type="warning">**Achtung!** Nicht vergessen Passwörter zu setzen.</alert>

Die Ordnerstruktur sieht dann wie folgt aus:

```bash
├── docker-compose.yml
├── uploads.ini
└── wp-content/
```

### Basis-Image

Als Basis-Image kommt hier `andreccosta/wordpress-xdebug` zum Einsatz. Das Image basiert auf dem offiziellen Wordpress-Image, kommt aber mit vorkonfigurierter Xdebug-Integration.

> Mehr Infos auf [Docker Hub](https://hub.docker.com/r/andreccosta/wordpress-xdebug) ansehen.

### Volumes

Sowohl der Ordner `/wp-content` als auch die Datei `uploads.ini` wird in den Container hinein gemountet (s. Volumes im Service `wordpress` in der [`docker-compose.yml`](#docker-compose-stack) oben).

Themes und Plugins sind so leicht über das Host-System zu erreichen und über die `uploads.ini` lassen sich die PHP-Einstellungen verändern.

### `uploads.ini` Konfiguration

Die Datei erhöht die Upload- und Memory-Limits der PHP-Installation im Container und macht lokales Testen z.B. mit großen Dateiuploads etwas angenehmer.

```ini
file_uploads = On
memory_limit = 128M
upload_max_filesize = 64M
post_max_size = 128M
max_execution_time = 600
```

### Cronjobs

WP-Cronjobs laufen innerhalb des Docker-Containers nicht. Daher ist die Verwendung des alternativen Cron-Modus von Wordpress empfehlenswert. Dieser wird in der `wp-config.php` aktiviert und kann somit auch über die `WORDPRESS_CONFIG_EXTRA` Umgebungs-Variable im `docker-compose`-File konfiguriert werden:

```docker[docker-compose.yml]
WORDPRESS_CONFIG_EXTRA: |
  define( 'ALTERNATE_WP_CRON', true );
  define( 'DISABLE_WP_CRON', true );
```

Anschließend müssen dann aber die Cronjobs durch Aufruf der `wp-cron.php` manuell gestartet werden. Dies kann natürlich auch via "echtem" Cronjob von extern via HTTP-Request erfolgen:

```sh
curl http://localhost:8000/wp-cron.php
```

## Eigenes Dockerfile

Das Basis-Image `wordpress:latest` verwendet üblicherweise die jeweils neuste PHP-Version. Idealerweise sollten aber natürlich die Entwicklungs-Umgebung sowie das Produktiv-System gleiche Vorraussetzungen bieten.

Es empfiehlt sich daher (gerade für umfangreichere Projekte) ein eigenes Dockerfile mit einer konkreten PHP-Version zu erstellen. Gleiches gilt natürlich für die Art/Version der verwendeten Datenbank.

<alert type="info">**Info!** Unter dem Pfad `/wp-admin/site-health.php?tab=debug` lassen sich im Bereich `Server` und `Datenbank` schnell die installierten Versionen auslesen.</alert>
