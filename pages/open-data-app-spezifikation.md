---
layout: default
title: Open Data Apps (ODA)
---

# Open Data App Spezifikationen

Eine Open Data App (ODA) ist eine nützliche Web-App, die Daten aus einem Open Data Portal bezieht und damit interagiert, beispielsweise durch Visualisierung.

ODAs ermöglichen den einfachen visuellen Zugang zu öffentlichen Daten und fördern Open Data Sichtbarkeit, Transparenz und Innovation.

Eine ODA kann in einem Open Data App Store (ODAS) veröffentlicht werden.

## Vorbemerkung

In diesem Dokument wird **soll**, **muss**, **darf** und **kann** verwendet und das ist auch genauso gemeint!

## Architektur einer ODA

Eine ODA ...

- besteht aus statischen Dateien (js, html, css).
- sollte alle statischen Dateien aus dem ODAS beziehen.
- muss alle Daten aus einem Open Data Portal beziehen.
- muss ein Web-UI haben, das folgende Inhalte enthält: Beschreibung, Kontakt, Datenschutz-Informationen, Impressum, Header, Footer.
- muss ihre Konfiguration über den ODAS beziehen (Config-Datei).
- wird mit einem Link gestartet.
- läuft in einem Browser-Fenster (nicht im IFRAME???).
- besitzt eine Datei `app_package.json`, in der die Konfiguration der ODA beschreiben ist

Die ODA ist grundsätzlich serverless.

## Funktion einer ODA

- Die ODA tut genau das, was in der Beschreibung ausgeführt ist
- Eine ODA darf keinen Schadcode enthalten oder nachladen.
- Eine ODA darf nichts Betrügerisches tun.

### Die Datei `app_package.json`

Die Datei `app_package.json` liegt im Root Verzeichnis der ODA.

In der Datei sind alle Metadaten zur App (Lizenz, Version, Konfiguration,...) enthalten.

Die Datei `/app_package.json` muss folgende Top-Level Elemente enthalten:

- `app-entwickler-id`: siehe ODAS
- `app-entwickler-name`: siehe ODAS
- `name-in-url`: wird später beim Aufruf der App in der URL verwendet [a-z0-0-]
- `name`: Name der App, wie er im ODAS angezeigt wird
- `version`: Versionsnummer (Format: 1.2.3)
- `odas-app-icon`: Icon der App für die Anzeige im ODAS. Dateiname ohne Pfad. Datei muss in /assets liegen. Größe: 512x512px. Format: png, jpg, svg
- `app-icon`: Icon der App für die Anzeige in der App z.B. links oben Datei muss in /app liegen. Größe: 1:3 Verhältnis, z.B. 60px Breite x 180px Höhe. Format: png, jpg, svg
- `kurzbeschreibung`: kurze Beschreibung der App (für ODP-Betreiber)
- `beschreibung`: Beschreibung der App (für ODP-Betreiber). Hier sollten Funktionweise, Datenformate, Konfigurationen etwas stehen
- `screenshots`: Array mit mehreren Screenshots. Müssen in `assets` liegen
- `apiversion`: Version der config-API. Muss aktuell 1.0 sein
- `instanz-config`: Formularspezifikation zur Konfiguration der Config-Datei durch den Open Data Portalbetreiber (siehe unten)

#### Multiline-Strings in `app_package.json`

Wenn längerer Text in einem Wert enthalten sein soll, kann dieser
als "Multiline-String" definiert werden.

Jeder Wert inder `app_package.json`, der ein Array von Strings ist und
der erste String ein "_multiline_" ist, wird als Multiline-String interpretiert.

Beispiel für einen Wert, der als Multiline-String definiert ist:

```
{
  ...
  "mehrzeiliger-string": [
    "_multiline_",
    "erste zeile ",
    "zweite Zeile\n",
    "dritte Zeile"
  ],
  ...
}
```

Dieser Wert wird im ODAS interpretiert als:

```
  "mehrzeiliger-string":"erste zeile zweite Zeile\ndritte Zeile"
```

Ein vollständiges Beispiel für ein app_packages ist unten dargestellt.

### ODA-Assets

ODA-Assets sind statische Dateien, die von der App benötigt werden, z.B. CSS-Dateien, Icons, ...

Die ODA-Assets sind im Verzeichnis `/assets` zu speichern.

Folgende Dateinamen ist nicht zulässig:

- `_odp-logo.png`

#### Aufbau der `instanz-config`

Die `instanz-config` definiert, wie das Formular zur Bearbeitung der Konfiguration durch den
Open Data Portalbetreiber aufgebaut ist (siehe unten).

Es wird jedes Feld des Konfigurationsformulars für diese ODA einzeln spezifiert.
Der App-Entwickler legt die Felder selber fest, die zur Konfiguration der App benötigt werden.

Aufbau der `instanz-config`:

Der Key (z.B. `titel`) muss eindeutig sein.

Feld-Angaben:

- `label`: beschreiftung des Feldes
- `hilfe`: Kurzhilfe, was mit dem Feld eingegeben werden soll

* `erforderlich`: "ja" oder "nein"
* `format`: Format des Feldes. Als Formate sind zulässig:
  - string (mit Längenangabe `laenge`
  - html
  - url
  - email
  - markdown
  - image (das image muss über als ODA-Asset gespeichert sein
* `default`: Vorbelegungen können mit dem `default` Key angegeben werden

Beispiel für `instanz-config`:

```
  "instanz-config": {
    "titel": {
      "label": "Titel",
      "hilfe": "Der Titel wird in der Titelzeile der App angezeigt",
      "default": "Generic Open Data App",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "icon": {
      "label": "App-Icon",
      "hilfe": "Das Icon wird links oben in der Titelzeile angezeigt",
      "format": {
        "typ": "image",
        "hoehe": 100,
        "breite": 300
      },
      "erforderlich": "ja"
    },
    "kontakt": {
      "label": "Kontakt",
      "hilfe": "Der Text wird im Menüpunkt 'Kontakt' anzeigt",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "Bei Fragen zur App wenden Sie sich bitte an: ",
        "Tel.: <a href='tel:{{anbieter.telcode}}'>{{anbieter.tel}}</a>",
        "Fax: <a href='tel:{{anbieter.fax}}'>{{anbieter.faxcode}}</a>",
        "E-Mail: <a href='mailto:{{anbieter.email}}'>{{anbieter.email}}</a>"
      ],
      "erforderlich": "ja"
    },

```

### Veröffentlichung einer ODA im ODAS

Eine ODA muss eine Datei `/app_package.json` haben. In der sind
alle Metadaten zur App (Lizenz, Version, Konfiguration,...)
enthalten.

Die Datei `/app_package.json`

- muss folgende Top-Level Elemente enthalten:
  - app-entwickler-id: siehe ODAS
  - app-entwickler-name: siehe ODAS
  - name-in-url: Teil der Url
  - name
  - version
  - odas-app-icon
  - app-icon
  - kurzbeschreibung
  - beschreibung
  - screenshots
  - apiversion
  - instanz-config: Formularspezifikation zur Konfiguration der Config-Datei durch den Open Data Portalbetreiber

Die in der ODA enthaltenen statischen Dateien sollten
im Verzeichnis `/assets` liegen.

## Vollständiges Beispiel einer `app_package.json`

```
{
  "app-entwickler-id": "12343",
  "app-entwickler-name": "ondics-gmbh",
  "name-in-url": "generic",
  "name": "Generic Open Data App",
  "version": "1.0.0",
  "odas-app-icon": "assets/odas-app-icon.png",
  "app-icon": "assets/app-icon.png",
  "kurzbeschreibung": "Diese Open Data App ist Anschauungsobjekt und Musterbeispiel für weitere Apps.",
  "beschreibung": [
    "_multiline_",
    "Die Open Data App ist Musterbeispiel für weitere Apps. Inhaltlich macht sie ",
    "fast nichts, sondern zeigt nur die Config Daten an. ",
    "Die Daten werden aus einem Open Data Portal bezogen. ",
    ""
  ],
  "screenshots": [
    "Desktop_Screenshot.png",
    "Mobile_Screenshot.png"
  ],
  "api-version": "1",
  "instanz-config": {
    "seitentitel": {
      "label": "Seitentitel",
      "hilfe": "Der Seitentitel wird im Browser-Tab der App angezeigt",
      "default": "Generic ODA",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "lizenz": {
      "label": "Lizenz",
      "hilfe": "Angabe der Lizenz, die für die Nutzung der App gilt.",
      "default": "Open Software License (OSL)",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "titel": {
      "label": "Titel",
      "hilfe": "Der Titel wird in der Titelzeile der App angezeigt",
      "default": "Generic Open Data App",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "icon": {
      "label": "App-Icon",
      "hilfe": "Das Icon wird links oben in der Titelzeile angezeigt",
      "format": {
        "typ": "image",
        "hoehe": 100,
        "breite": 300
      },
      "erforderlich": "ja"
    },
    "kontakt": {
      "label": "Kontakt",
      "hilfe": "Der Text wird im Menüpunkt 'Kontakt' anzeigt",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "Bei Fragen zur App wenden Sie sich bitte an: ",
        "Tel.: <a href='tel:{{anbieter.telcode}}'>{{anbieter.tel}}</a>",
        "Fax: <a href='tel:{{anbieter.fax}}'>{{anbieter.faxcode}}</a>",
        "E-Mail: <a href='mailto:{{anbieter.email}}'>{{anbieter.email}}</a>"
      ],
      "erforderlich": "ja"
    },
    "beschreibung": {
      "label": "Beschreibung",
      "hilfe": "Der Text wird im Menüpunkt 'Über diese App' anzeigt",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "Die Generic-App zeigt die jeweils definierten Configs.",
        "Die Daten werden tagesaktuell gehalten. Bei Änderungen versuchen wir, ",
        "den Datenbestend immer sofort zu aktualisieren.\n ",
        "## App-Anbieter",
        "Die App wird bereit gestellt von <a href=\"{{anbieter.url}}\"><{{anbieter.name}}>",
        "## Daten",
        "Die Daten kommen aus  unserem Open Data Portal <a href=\"{{odp.url}}\"><{{odp.name}}>",
        "Die Datenquelle ist in unserem Open Data Portal <a href=\"{{appinstanz.datensatz-url}}\"><{{appinstanz.datensatz-name}}>.",
        "## App-Entwickler",
        "Die App wurde entwickelt von <a href=\"{{app.developer-url}}\"><{{app.developer-name}}>. App-Version: {{app.version}}.",
        "## Open Data App Store",
        "Der Open Data App Store betrieben von <a href=\"{{odas.betreiber.url}}\"><{{odas.betreiber.name}}>\""
      ],
      "erforderlich": "ja"
    },
    "impressum": {
      "label": "Impressum",
      "hilfe": "Der Text wird im Menüpunkt 'Impressum' anzeigt",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "{{anbieter.name}}",
        "{{anbieter.kontakt-bezeichnung}}",
        "{{anbieter.strasse}}",
        "{{anbieter.plzort}}",
        "Telefon: {{anbieter.tel}}",
        "Website: <a href='{{anbieter.url-extern}}' target='_blank'>{{anbieter.url-extern}}</a>"
      ],
      "erforderlich": "ja"
    },
    "datenschutz": {
      "label": "Datenschutz",
      "hilfe": "Der Text wird im Menüpunkt 'Datenschutz' anzeigt",
      "format": {
        "typ": "markdown"
      },
      "default": "Alle Daten sind geschützt.",
      "erforderlich": "ja"
    },
    "urlDaten": {
      "label": "URL zum Datensatz im ODP",
      "hilfe": "Der Datensatz im Open Data Portal mit den Daten der Generic App",
      "format": {
        "typ": "url"
      },
      "default": "",
      "beispiel": "",
      "erforderlich": "ja"
    },
    "fusszeile": {
      "label": "Fußzeile",
      "hilfe": "Wird auf der Website unten in der Fusszeile angezeigt.",
      "format": {
        "typ": "String",
        "laenge": 50
      },
      "default": "(C) Copyright {{jahr}} | App & Daten: {{anbieter.name}} | App-Entwickler: {{app.developer.name}} | ODAS: {{odas.betreiber.name}}",
      "erforderlich": "ja"
    },
    "apiUrl": {
      "label": "URL zur Datensatz-API",
      "hilfe": "Von dieser API-URL zum Datensatz werden die Ressoucen bezogen",
      "format": {
        "typ": "url"
      },
      "default": "",
      "beispiel": "",
      "erforderlich": "ja"
    },
    "sprache": {
      "label": "Sprache der App",
      "hilfe": "Angabe der App-Sprache",
      "format": {
        "typ": "dropdown",
        "optionen": ["de"],
        "default": "de"
      },
      "erforderlich": "ja"
    }
  }
}
```

## Entwicklung einer ODA

Systemvoraussetzungen (empfohlen):

- Ubuntu
- Docker, Docker-Compose, Make

Die Entwicklung kann mit der `odas-app-generic` begonnen werden.
Zunächst wird das Repo gecloned:

    $ git clone ...

Jetzt bauen wird das Image mit dem Webserver Nginx:

    $ make build

Und starten den Conatiner (derTCP/IP-Port kann in `docker-compose.yml` geändert werden).

    $ make up

Die ODA steht dann unter http://localhost:8090 zur Verfügung.

Die App kann wieder gestoppt werden mit

    $ make down

Um eine ODA auszuliefern und in den ODAS einzustellen, wird die zip-Datei ersellt:

    $ make zip

Folgende Dateien müssen dazu vorhanden sein:

```
odas-app-beispiel/
├── app/
│ ├── index.html
│ ├── app-base.css
│ ├── app.css
│ ├── app-base.js
│ ├── app.js
│ ├── logo.png
│ └── favicon.png
├── assets/
│ ├── Desktop_Screenshot.png
│ ├── Mobile_Screenshot.png
├── Changelog.md
└── app-package.json
```

Der Ordner "app" sowie der Ordner "assets" dürfen beliebig viele Dateien enthalten.

## Beispiel App Prompt

Prompt:

agiere als softwareentwickler für eine web-app.

die technischen rahmenbedingungen der app sind folgende;

- die web-app besteht aus einem header, einem footer und einem inhaltsbereich.
  header und footer stehen bereits fest. es muss nur der inhaltsbereich erstellt werden.
- in dem kommentar des codes-template stehen die übergabeparameter.
- Mit der app() Funktion soll der Content für die Seite generiert werden. Die übergebene
  Variable configdata enthählt die apiUrl. Diese enthält einen Link
  zu einem Datensatz oder einer Datei aus einem Open Data Portal. Falls benötigt
  soll die App von dort die Daten beziehen.
- die app() funktion muss in Javascript geschrieben werden.
- Der generierte Content soll ausschließlich in das enclosingHtmlDivElement geladen werden.
- Mit der Funktion addToHead können Skripte oder Stylesheets hinzugefügt werden per Javascript
- Alles innerhalb der beiden Funktionen ist nur BeispielCode und soll ersetzt werden.

die app soll folendes tun:

- anzeige eines Ping Pong Spiels
- der Spieler (rechts) soll mit den Pfeiltasten auf und ab fahren
- der Spieler (links) ist der Computer
- das Spiel soll über die Ganze breite gehen

aufgabe:
Erstelle die app. fülle dazu die funktion app() {} und ggf. addToHead() {}

hier ist der code-template:

```
/\*

- Diese Funktion ist für die Inhalte der Startseite
- zuständig.
-
- Der umschließebde HTML code ist:
-      <body>
-      <div class="container mt-4" id="main-content">
-          ...
-      </div>
-      </body>
- Als CSS Framnework wird Bootstrap 5.3 verwendet.
-
- ConfigData ist ein JSON enthält die Referenz
- auf die Daten im CKAN Open Data Portal:
-     {
-         "apiurl": "https://open-data-musterstadt.ckan.de/dataset/db92da8e40f9/download/formular_multitemplate.json"
-     }
-
- @param {Object} configdata - Alle Konfigurationsdaten der App
- @enclosingHtmlDivElement - HTML Knoten des umschließenden Tags
- @returns {string | NULL} - darzustellendes HTML oder NULL wenn HTML Knoten direkt manipuliert wurde
  \*/

function app(configdata = [], enclosingHtmlDivElement) {
// hier muss der app-code stehen
}

/\*

- Diese Funktion kann Bibliotheken und benötigte Skripte laden.

- @returns {string} - HTML mit script, link, etc. Tags
  \*/
  function addToHead() {
  }
```

## Offene Punkte

- Mehrsprachigkeit: Nur App oder auch Daten? Statisch oder konfiguriert?

## Autor und Kontakt

Kontakt: [info@ondics.de
](info@ondics.de)

Website: [ondics.de
](https://ondics.de)

(C) 2025 Ondics GmbH
