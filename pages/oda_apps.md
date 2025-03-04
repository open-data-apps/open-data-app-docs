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
  - apiVersion
  - instanz-config: Formularspezifikation zur Konfiguration der Config-Datei durch den Open Data Portalbetreiber

Die in der ODA enthaltenen statischen Dateien sollten
im Verzeichnis `/assets` liegen.

Beispiel:

```
{
  "app-entwickler": "12343",
  "app-entwickler-name": "mueller-gmbh",
  "name-in-url": "telefonbuch",
  "name": "Telefonbuch",
  "version": "1.0.0",
  "odas-app-icon": "assets/odas-app-icon.png",
  "app-icon": "assets/app-icon.png",
  "kurzbeschreibung": "Telefonbuch mit Bereichauswahl auf Basis von CSV-Dateien",
  "beschreibung": [
    "Das Telefonbuch zeigt Namen, Stellen und Telefonnummern an. Die Daten werden aus einem Open",
    "Data Portal bezogen und müssen in Form von JPG-Dateien vorliegen.",
    "Die Daten müssen in einem Open Data Portal",
    "wie folgt aufgebaut sein: \n",
    "* Datensatz: Mit allen Telefonbüchern als Ressourcen",
    "* Ressourcen: Jede Telefonbuch Ressource muss als CSV Datei angelegt werden. Diese müssen folgendem Format entsprechen:",
    "name, stelle, telefonnummer"
  ],
  "screenshots": ["assets/Desktop_Screenshot.png", "assets/Mobile_Screenshot.png"],
  "apiVersion": "1",
  "instanz-config": {
    "seitentitel": {
      "label": "Seitentitel",
      "hilfe": "Der Seitentitel wird im Browser-Tab der App angezeigt",
      "default": "Telefon-App",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "titel": {
      "label": "Titel",
      "hilfe": "Der Titel wird in der Titelzeile der App angezeigt",
      "default": "Telefonbuch",
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
        "hoehe": 64,
        "breite": 189
      },
      "erforderlich": "ja"
    },
    "kontakt": {
      "label": "Kontakt",
      "hilfe": "Der Text wird im Menüpunkt 'Kontakt' anzeigt",
      "format": {
        "typ": "html"
      },
      "default": "Tel.: <a href='tel:{{anbieter.telcode}}'>{{anbieter.tel}}</a><br>Fax: <a href='tel:{{anbieter.fax}}'>{{anbieter.faxcode}}</a><br>E-Mail: <a href='mailto:{{anbieter.email}}'>{{anbieter.email}}</a>",
      "erforderlich": "ja"
    },
    "beschreibung": {
      "label": "Beschreibung",
      "hilfe": "Der Text wird im Menüpunkt 'Über diese App' anzeigt",
      "format": {
        "typ": "html"
      },
      "default": "Die Telefonbuch-App zeigt die Namen, Stellen und Telefonnummern für ausgewählte Telefondatensätze.\nDie Daten werden tagesaktuell gehalten. Bei Änderungen versuchen wir, den Datenbestend immer sofort zu aktualisieren. \n##App-Anbieter\nDie App wird bereit gestellt von <a href=\"{{anbieter.url}}\"><{{anbieter.name}}>\nDie Daten kommen aus  unserem Open Data Portal <a href=\"{{odp.url}}\"><{{odp.name}}> geladen\n##Datenquelle\nDie Datenquelle ist in unserem Open Data Portal <a href=\"{{appinstanz.datensatz-url}}\"><{{appinstanz.datensatz-name}}> geladen.\n## App-Entwickler\n Die App wurde entwickelt von <a href=\"{{app.developer-url}}\"><{{app.developer-name}}>. App-Version: {{app.version}}.\n## Open Data App Store\nDer Open Data App Store betrieben von <a href=\"{{odas.betreiber.url}}\"><{{odas.betreiber.name}}>\n",
      "erforderlich": "ja"
    },
    "impressum": {
      "label": "Impressum",
      "hilfe": "Der Text wird im Menüpunkt 'Impressum' anzeigt",
      "format": {
        "typ": "html"
      },
      "default": "{{anbieter.name}}\n{{anbieter.kontakt-bezeichnung}}\n{{anbieter.strasse}}\n{{anbieter.plzort}}\nTelefon: {{anbieter.tel}}\nFax: {{anbieter.fax}}\nWebsite: <a href='{{anbieter.url-extern}}' target='_blank'>{{anbieter.url-extern}}</a>",
      "erforderlich": "ja"
    },
    "datenschutz": {
      "label": "Datenschutz",
      "hilfe": "Der Text wird im Menüpunkt 'Datenschutz' anzeigt",
      "format": {
        "typ": "html"
      },
      "default": "Alle Daten sind geschützt.",
      "erforderlich": "ja"
    },
    "urlDaten": {
      "label": "URL zum Datensatz im ODP",
      "hilfe": "Der Datensatz im Open Data Portal mit den CSV-Einzeldateien des Telefonbuchs",
      "format": {
        "typ": "url"
      },
      "default": "{{appconfig.datensatz-url}}",
      "beispiel": "https://open-data-musterstadt.ckan.de/dataset/telefonbuch-musterstadt",
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
      "hilfe": "Von dieser API-URL zum Datensatz werden die Ressoucen bezogen (JSON)",
      "format": {
        "typ": "url"
      },
      "default": "{{appconfig.datensatz-apiurl}}",
      "beispiel": "https://open-data-musterstadt.ckan.de/api/3/action/package_show?id=telefonbuch-musterstadt",
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
├── Changelog.md
└── app-package.json
```

## Offene Punkte

- Mehrsprachigkeit: Nur App oder auch Daten? Statisch oder konfiguriert?

## Autor und Kontakt

Kontakt: info@ondics.de

https://ondics.de

(C) 2025 Ondics GmbH
