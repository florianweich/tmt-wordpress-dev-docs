---
title: Visual Studio Code
description: ''
position: 12
category: Dev-Umgebung
---

Um die Nutzung mit Visual Studio Code zu optimieren, ist die Nutzung einiger Erweiterungen sinnvoll.

## Auto-Vervollständigungen

Um Autocompletions für PHP und vor allem die Wordpress-eigenen Funktionen zu bekommen, gibt es zwei Erweiterungen für VS Code:

- [PHP IntelliSense](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-intellisense)
- [WordPress Snippets](https://marketplace.visualstudio.com/items?itemName=wordpresstoolbox.wordpress-toolbox)

## Prettier

Für [Prettier](https://prettier.io/) (der "opinionated code formatter", bekannt vor allem für JavaScript/TypeScript) gibt es ein Plugin, das auch PHP formattieren kann.

> Mehr zum PHP-Plugin auf [GitHub](https://github.com/prettier/plugin-php).

### Einrichtung

Zunächst sollte die Prettier-Erweiterung für VS Code installiert werden: [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

Anschließend kann das PHP-Plugin via `npm` nachinstalliert werden.  
Folgende Einrichtungs-Bespiele gehen davon aus, dass bereits `yarn` oder `npm` für das Projekt eingereichtet wurde.

<code-group>
  <code-block label="Yarn" active>

```bash
  yarn add -D prettier @prettier/plugin-php
```

  </code-block>
  <code-block label="NPM">

```bash
  npm install -D prettier @prettier/plugin-php
```

  </code-block>
</code-group>

Mit aktivierter Option "Format On Save" (zu finden in den VS Code-Einstellungen) formattiert Prettier nun nach vordefinieten Regeln den Code, sobald die Datei gespeichert wird.

<alert type="info">**Hinweis!** Das Plugin befindet sich zum aktuellen Zeitpunkt noch in Beta-Phase und hat ein paar kleinere Macken bei Template-Code (gemischtes HTML/PHP).</alert>

### `.prettierrc`

Mit Hilfe der `.prettierrc`-Datei im Projekt-Root lassen sich Formatierungsregeln definieren. Hier ein Beispiel:

```json
{
  "phpVersion": "7.3",
  "braceStyle": "1tbs",
  "trailingCommaPHP": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

> Mehr zur Syntax [hier](https://prettier.io/docs/en/configuration.html).

Das obige Beispiel enthält auch Regeln für JavaScript (`"trailingComma": "es5"`) - diese können also auch gemischt werden.

<alert type="warning">**Achtung!** Unbedingt die korrekte `phpVersion` einstellen, die auch im Produktiv-System verwendet wird! Prettier verwendet nämlich sonst moderne PHP-Syntax, die unter alten Versionen nicht funktionieren würde.</alert>

## Debugger (Xdebug)

Das Docker-Image sollte bereits für Xdebug konfiguriert sein (s. [Docker](/development/docker))!

Außerdem muss die [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)-Erweiterung in Visual Studio Code installiert sein.

Folgende Launch-Konfiguration für den Debugger kann verwendet werden:

```json[launch.json]
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for XDebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
        "/var/www/html/wp-content/plugins/PLUGIN_NAME_HERE": "${workspaceRoot}"
      },
      "ignore": ["**/wp-admin/**/*.php", "**/wp-includes/**/*.php"]
    }
  ]
}
```

<alert type="warning">**Achtung!** Nicht vergessen den Pfad innerhalb des Docker-Containers zu unserem Plugin in den `pathMappings` zu hinterlegen.</alert>

> Hier gibt's generelle Infos zum [VS Code Debugger](https://code.visualstudio.com/docs/editor/debugging).
