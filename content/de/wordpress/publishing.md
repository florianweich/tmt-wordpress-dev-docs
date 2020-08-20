---
title: Veröffentlichung
description: ''
position: 118
category: Wordpress
---

Wordpress-Plugins können als `zip`-Datei direkt im Backend hochgeladen werden. Mit Hilfe des `git archive`-Kommandos lässt sich schnell eine `zip`-Datei des aktuellen Commits erstellen.

## `git archive`

Mit folgendem Befehl lässt sich der aktuelle `HEAD`-Commit in eine zip-Datei packen:

```bash
git archive -o wp-plugin-name.zip HEAD
```

<alert type="info">**Info!** Die vorhandenen `.gitignore`-Regeln werden dabei beachtet. Weitere Dateien und Ordner lassen sich mit Hilfe einer [`.gitattributes`](#gitattributes)-Datei vom "Archivieren" ausschließen.</alert>

### npm-Skript

Um das Ganze etwas bequemer zu machen, bietet es sich an den Befehl in der `package.json`-Datei als `npm`-Skript zu hinterlegen:

```json[package.json]
{
  "name": "wp-plugin-name",
  "scripts": {
    "zip": "git archive -o $npm_package_name.zip HEAD && \
            mv ./$npm_package_name.zip ~/Desktop/"
  }
}
```

Im Beispiel oben wird die Datei nach dem Zippen anschließend noch auf den Desktop verschoben.

<alert type="info">**Info!** Die Environment-Variable `$npm_package_name` wird von `npm`/`yarn` beim Ausführen des Skripts gesetzt und basiert auf dem hinterlegten Namen in der `package.json`-Datei.</alert>

### `.gitattributes`

Um weitere Dateien oder Ordner (zum Beispiel den `src`-Ordner) von der veröffentlichten `zip`-Datei auszuschließen, können `Git-Attribute` genutzt werden.

Dazu wird einfach eine `.gitattributes`-Datei im Projekt-Root angelegt und mit commited:

```txt[.gitattributes]
/.vscode export-ignore
/src export-ignore
.gitattributes export-ignore
.gitignore export-ignore
.prettierrc export-ignore
composer.json export-ignore
composer.lock export-ignore
mix-manifest.json export-ignore
package.json export-ignore
package-lock.json export-ignore
README.md export-ignore
tsconfig.json export-ignore
webpack.mix.js export-ignore
yarn.lock export-ignore
```
