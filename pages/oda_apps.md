---
layout: default
title: Open Data Apps (ODA)
permalink: /oda_apps/
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
- kann ihre Konfiguration über den ODAS beziehen (Config-Datei).
- wird mit einem Link gestartet.
- läuft in einem Browser-Fenster (nicht im IFRAME???).

Die ODA ist grundsätzlich serverless.

## Funktion einer ODA

- Eine ODA darf keinen Schadcode enthalten oder nachladen.
- Eine ODA darf nichts Betrügerisches tun.

### Veröffentlichung einer ODA im ODAS

Eine ODA muss eine Datei `/app_package.json` haben. In der sind
alle Metadaten zur App (Lizenz, Version, Konfiguration,...)
enthalten.

Die Datei `/app_package.json`

- muss folgende Top-Level Elemente enthalten:
  - app-entwickler: siehe ODAS
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

Beispiel:

```
{
  "app-entwickler": "12343",
  "app-entwickler-name": "ondics-gmbh",
  "name-in-url": "generic",
  "name": "Generic Open Data App",
  "version": "1.0.0",
  "odas-app-icon": "assets/odas-app-icon.png",
  "app-icon": "assets/app-icon.png",
  "kurzbeschreibung": "Diese Open Data App ist Anschauungsobjekt und Musterbeispiel für weitere Apps.",
  "beschreibung": [
    "Die Open Data App ist Musterbeispiel für weitere Apps. Inhaltlich macht sie ",
    "fast nichts, sondern zeigt nur die Config Daten an. ",
    "Die Daten werden aus einem Open Data Portal bezogen. ",
    ""
  ],
  "screenshots": [
    "/assets/Desktop_Screenshot.png",
    "/assets/Mobile_Screenshot.png"
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
        "{{anbieter.name}}",
        "{{anbieter.kontakt-bezeichnung}}",
        "{{anbieter.strasse}}",
        "{{anbieter.plzort}}",
        "Telefon: {{anbieter.tel}}",
        "Fax: {{anbieter.fax}}",
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

### Aufbau der instanz-config

Es wird jedes Feld des Konfigurationsformulars für diese ODA spezifiert.

- Format-Typen der Felder: string, html, url, email
- beim Format-Typ `string` ist noch eine maximale `laenge` angebbar
- Vorbelegungen können mit dem `"default"` Key angegeben werden
- in `"default"` Werten sind folgende Platzhalter (z.B. `anbieter-tel`) möglich:

```
  jahr: "Aktuelles Jahr",
  anbieter: {
    name: "Name des Anbieters",
    email: "E-Mail des Anbieters",
    tel: "Telefonnummer des Anbieters",
    fax: "Faxnummer des Anbieters",
    url: "Webseite des Anbieters",
    strasse: "Straße des Anbieters",
    plzort: "Postleitzahl und Ort des Anbieters",
    kontaktbezeichnung: "Kontaktbezeichnung des Anbieters",
  },
  app: {
    version: "Version der App",
    developer: {
      name: "Name des Entwicklers",
      email: "E-Mail des Entwicklers",
      "developer-url": "Webseite des Entwicklers",
    },
  },
  appinstanz: {
    "datensatz-url": "URL des Datensatzes",
    "datensatz-name": "Name des Datensatzes",
  },
  odp: {
    name: "Name des ODP",
    url: "URL des ODP",
  },
  odas: {
    betreiber: {
      name: "Name des Betreibers",
      url: "Webseite des Betreibers",
    },
  },
```

## Entwicklung einer ODA

Die Entwicklung kann mit der `odas-app-generic` begonnen werden:

    $ git clone ...

Jetzt bauen wird das Image mit dem Webserver Nginx:

    $ make build

Und starten den Conatiner (derTCP/IP-Port kann in `docker-compose.yml` geändert werden).

    $ make up

Die ODA steht dann unter http://[ip-adresse]:[Port] zur Verfügung.

Die App kann wieder gestoppt werden mit

    $ make down

Um eine ODA auszuliefern und in den ODAS einzustellen, wird die zip-Datei ersellt:

    $ make zip

Die ZIP-Datei muss folgende Verzeichnisstruktur besitzen:

```
odas-app-beispiel/
├── app/
│ ├── index.html
│ ├── script.js
│ ├── logo.png
│ └── favicon.png
├── assets/
│ ├── Desktop_Screenshot.png
│ ├── Mobile_Screenshot.png
├── Changelog.md
└── app-package.json
```

## Offene Punkte

- Mehrsprachigkeit: Nur App oder auch Daten? Statisch oder konfiguriert?

## Autor und Kontakt

Kontakt: [info@ondics.de
](info@ondics.de)

[ondics.de
](https://ondics.de)

(C) 2025 Ondics GmbH
