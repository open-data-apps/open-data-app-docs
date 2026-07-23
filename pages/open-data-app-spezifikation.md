---
layout: default
title: Open Data Apps (ODA)
permalink: /open-data-app-spezifikation/
---

# Open Data App Spezifikation

Eine Open Data App (ODA) ist eine statische Web-App, die offene Daten aus einem Open Data Portal lädt, visualisiert oder anderweitig nutzbar macht. ODAs können im Open Data App Store (ODAS) veröffentlicht und dort von Portalbetreibern für ihr eigenes Portal konfiguriert werden.

Diese Spezifikation trennt drei Ebenen:

- **Muss**: notwendig für ODAS-Kompatibilität.
- **Empfohlen**: Konventionen aus der Vorlage `oda-generic`, damit Apps gut wartbar und leicht prüfbar bleiben.
- **Ondics-Standard**: unser interner bzw. Showcase-Qualitätsanspruch für besonders sorgfältig ausgearbeitete Apps.

## ODAS-Kompatibilität

Eine ODA muss:

- aus statischen Dateien bestehen, insbesondere HTML, CSS, JavaScript und Assets.
- im Browser laufen und ohne eigenes Backend auskommen.
- ihre Laufzeitkonfiguration über den ODAS beziehen.
- ein Web-UI mit Hauptinhalt, Beschreibung, Kontakt, Impressum und Datenschutz-Informationen bereitstellen.
- alle app-spezifischen Metadaten in einer Datei `app-package.json` im Projektwurzelverzeichnis beschreiben.
- offene Daten aus einer konfigurierten oder dokumentierten Datenquelle laden.
- genau die Funktion ausführen, die in Beschreibung und Metadaten angegeben ist.
- frei von Schadcode sein und keine geheimen Tokens, Passwörter oder privaten Schlüssel enthalten.

Eine ODA darf zusätzliche JavaScript- oder CSS-Bibliotheken verwenden, solange diese für die App-Funktion notwendig oder nachvollziehbar sind und keine Sicherheitsrisiken erzeugen.

## Empfohlene Architektur

Für neue Apps wird die Vorlage [`oda-generic`](https://github.com/open-data-apps/oda-generic) empfohlen. Sie definiert den üblichen Aufbau:

```text
oda-example/
├── app/
│   ├── index.html
│   ├── app-base.js
│   ├── app-base.css
│   ├── app.js
│   ├── app.css
│   ├── favicon.png
│   └── logo_ondics.png
├── assets/
│   ├── odas-app-icon.svg
│   ├── Desktop_Screenshot.png
│   ├── Mobile_Screenshot.png
│   ├── branding.css
│   └── schema.json
├── odas-config/
│   └── config.json
├── app-package.json
├── CHANGELOG.md
├── README.md
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
└── Makefile
```

Bei Verwendung von `oda-generic` gilt als empfohlene Konvention:

- App-spezifisches JavaScript liegt in `app/app.js`.
- App-spezifisches CSS liegt in `app/app.css`.
- `app/app-base.js`, `app/app-base.css` und `app/index.html` bilden die Laufzeitvorlage und müssen für normale App-Entwicklung nicht angepasst werden.
- Die Hauptfunktion heißt `function app(configdata, enclosingHtmlDivElement)`.
- `addToHead()` steht außerhalb von `app()` und wird verwendet, wenn zusätzliche Ressourcen geladen werden müssen.
- Bootstrap 5.3.8 kann direkt über die Vorlage genutzt werden.

Andere statische Implementierungen sind möglich, solange sie die ODAS-Kompatibilität erfüllen.

## `app-package.json`

Die Datei `app-package.json` enthält Metadaten, Datenbeschreibung und die Konfigurationsfelder, aus denen ODAS die Instanz-Konfiguration erzeugt.

Wichtige Top-Level-Felder sind:

- `app-entwickler-id`: ID des Entwicklerkontos, siehe ODAS.
- `app-entwickler-name`: Name des Entwicklerkontos, siehe ODAS.
- `name-in-url`: wird beim Aufruf der App in der URL verwendet; nur Kleinbuchstaben, Zahlen und Bindestriche (`[a-z0-9-]`).
- `name`: Name der App, wie er im ODAS angezeigt wird.
- `version`: Versionsnummer im Format `1.2.3`.
- `odas-app-icon`: Icon der App für die Anzeige im ODAS. Datei muss im Lieferumfang liegen, üblicherweise unter `assets/`. Empfohlen: quadratisch (ca. 512x512px), Format PNG, JPG oder SVG. Eine Prompt-Vorlage zur KI-Erstellung des Icons: [odas-app-icon-prompt](https://github.com/open-data-apps/odas-app-icon-prompt).
- `app-icon`: Icon für die Anzeige in der App selbst (links oben in der Kopfzeile), Seitenverhältnis ca. 1:3 (z.B. 100 px hoch, 300 px breit). In der Vorlage zeigen beide Icon-Felder auf `assets/odas-app-icon.svg`.
- `kurzbeschreibung`: kurze Beschreibung der App für Portalbetreiber.
- `beschreibung`: ausführlichere Store-Beschreibung für Portalbetreiber; hier sollten Funktionsweise, Datenformate und Konfigurationsmöglichkeiten stehen.
- `tags-funktion`, `tags-dateityp`, `tags-datenart`: Schlagwörter zur Einordnung der App im ODAS (Funktion, Dateityp, Datenart).
- `screenshots`: Array mit Screenshot-Pfaden; die Dateien müssen unter `assets/` liegen und werden relativ ohne führenden Slash angegeben.
- `daten`: Beschreibung der erwarteten Daten (siehe [Daten und Assets](#daten-und-assets)).
- `branding-css`: optionaler Pfad zu einer mitgelieferten Branding-CSS-Datei (siehe [ODA-Styling](#oda-styling)).
- `api-version`: Version der ODAS-Config-API, aktuell `"1"`.
- `instanz-config`: Formularspezifikation für die Konfiguration der App-Instanz durch den Portalbetreiber (siehe unten).

Apps mit `api-version: "1"` werden in dieser Doku als **ODAS-v1-Apps** bezeichnet; das ist die aktuelle Version der ODAS-Config-API.

## `instanz-config`

`instanz-config` beschreibt die Formularfelder, die ein Portalbetreiber im ODAS pro App-Instanz ausfüllt. Jeder Key muss eindeutig sein und sollte genau den Namen verwenden, den die App im `configdata` liest.

Ein Feld enthält typischerweise:

- `label`: sichtbare Feldbezeichnung.
- `hilfe`: kurzer Hilfetext.
- `format`: Datentyp und optionale Zusatzangaben.
- `default`: Vorbelegung.
- `beispiel`: optionales Beispiel.
- `erforderlich`: `"ja"` oder `"nein"`.

Robust unterstützte und empfohlene `format.typ`-Werte für ODAS-v1 sind:

- `string`
- `url`
- `dropdown`
- `markdown`
- `image`

Zahlen sollten in `instanz-config` als Strings gespeichert und in der App bei Bedarf mit `Number(...)`, `parseInt(...)` oder ähnlichen Funktionen umgewandelt werden. Das vermeidet Probleme in Import- und Editorpfaden.

Die Typnamen werden exakt kleingeschrieben verwendet: `string`, nicht `String`; Typen wie `Zahl` oder `number` gehören nicht zu den oben genannten, für ODAS-v1 robusten Typen. Auch die Key-Namen sind kanonisch: Der Datenendpunkt heißt `apiurl` (komplett klein), die Datensatzseite `urlDaten`. Parallele Legacy-Schreibweisen wie `apiUrl` oder doppelte `urldaten`/`urlDaten`-Definitionen sollen nicht neu eingeführt werden.

`dropdown`-Felder schreiben ihre Vorbelegung als Feld-`default`; `format` enthält den Typ und die Optionen:

```json
{
  "default": "nein",
  "format": {
    "typ": "dropdown",
    "optionen": ["nein", "ja"]
  }
}
```

### Multiline und Rich Text

Längere Werte können als String-Array mit dem Marker `_multiline_` gespeichert werden: Jeder Wert in der `app-package.json`, der ein Array von Strings ist und dessen erster String `"_multiline_"` lautet, wird als Multiline-String interpretiert. Der Marker wird entfernt und die übrigen Zeilen werden verbunden.

Beispiel für einen Wert, der als Multiline-String definiert ist:

```json
{
  "mehrzeiliger-string": [
    "_multiline_",
    "erste Zeile ",
    "zweite Zeile\n",
    "dritte Zeile"
  ]
}
```

Dieser Wert wird im ODAS interpretiert als:

```json
{
  "mehrzeiliger-string": "erste Zeile zweite Zeile\ndritte Zeile"
}
```

`format.typ: "markdown"` ist der ODAS-v1-Feldtyp für Rich Text. Felder dieses Typs werden in Markdown verfasst: Überschriften mit `##`, Listen mit `-`, Links mit `[Text](URL)`. ODAS wandelt das Markdown vor der Auslieferung der Konfiguration an die App in HTML um; die App rendert das Ergebnis unverändert (z.B. per `innerHTML`). Portalbetreiber können Felder wie Impressum, Kontakt oder Beschreibung dadurch ohne HTML-Kenntnisse pflegen.

**Hinweis zur Übergangsphase:** Bestehende Apps und Konfigurationen enthalten in Markdown-Feldern teilweise direkt HTML (z.B. `<h2>`, `<p>`, `<ul>`). Das bleibt während der Übergangsphase gültig; für neue und überarbeitete Inhalte ist Markdown der Standard. Das Beispiel-`app-package.json` weiter unten zeigt den aktuellen Stand der Vorlage `oda-generic` und enthält deshalb noch HTML-Defaults.

Statische Beschreibungstexte bleiben im bestehenden Feld `beschreibung`; dafür werden keine zusätzlichen Config-Keys angelegt. Die Datenquellenbeschreibung verlinkt, soweit Werte vorhanden sind, vom Open Data Portal über die Datensatzseite bis zur tatsächlich verwendeten Ressource oder API.

## Konfiguration

Zur Laufzeit erhält die App ein `configdata`-Objekt aus dem ODAS. Bei `oda-generic` werden unter anderem diese Basisfelder verwendet:

- `titel`
- `seitentitel`
- `icon`
- `fusszeile`
- `kontakt`
- `impressum`
- `datenschutz`
- `beschreibung`
- `brandingCSS`
- `brandingCSSFile`

Empfohlen ist: Jeder app-spezifische Key, den `app/app.js` liest, ist auch in `app-package.json` unter `instanz-config` dokumentiert. Für lokale Tests spiegelt `odas-config/config.json` diese Werte. `odas-config/config.json` ist dabei eine lokale Entwicklungsdatei; im ODAS wird die produktive Konfiguration aus den App-Metadaten und der jeweiligen Instanz erzeugt. Beim Anlegen einer App-Instanz übernimmt der ODAS zunächst die `default`-Werte und die in der `app-package.json` eingetragenen Werte; der Portalbetreiber passt sie anschließend über das Konfigurationsformular an.

## Daten und Assets

Der Abschnitt `daten` in `app-package.json` beschreibt die erwarteten Daten:

- `beschreibung`: kurze Erklärung der Quelle und des Formats.
- `schema`: Pfad zu einem [Frictionless Table Schema](https://specs.frictionlessdata.io/table-schema/), bei neuen Apps `assets/schema.json`.
- `beispiel`: optionaler Hinweis auf Beispieldaten.
- `beispiel-url`: Beispiel-URL zu einer echten Datenquelle oder Ressource.

Assets liegen üblicherweise unter `assets/`. Screenshotpfade werden relativ und ohne führenden Slash angegeben. Das Store- und Runtime-Icon zeigen im Normalfall beide auf `assets/odas-app-icon.svg`. Alle referenzierten Dateien müssen im aktiven ZIP-Lieferumfang vorhanden sein.

Der Dateiname `_odp-logo.png` ist in `assets/` nicht zulässig; er ist für den ODAS reserviert.

Neue Apps liefern ein echtes Frictionless Table Schema mit einem flachen `fields`-Array. Ein leeres Objekt, Nutzdaten anstelle eines Schemas oder ein allgemeines JSON Schema erfüllen diese Konvention nicht.

## ODA-Styling

ODAs sollen gleiche CSS-Grundlagen verwenden, damit sie sich in jedes Open Data Portal einheitlich einfügen. Die Basis bilden Bootstrap 5.3.8 und die Vorlagen-Datei [`app/app-base.css`](https://github.com/open-data-apps/oda-generic/blob/master/app/app-base.css) aus `oda-generic`; app-spezifisches Styling kommt zusätzlich in `app/app.css`.

Portalbetreiber können das Corporate Design pro App-Instanz anpassen, sodass die App nach dem Betreiber, der Stadt oder dem Open Data Portal aussieht. Dafür gibt es zwei Laufzeit-Konfigurationsfelder:

- `brandingCSS`: eigener CSS-Code für die konkrete Instanz.
- `brandingCSSFile`: URL zu einer eigenen CSS-Datei.

Die Vorlage liefert außerdem `assets/branding.css` als mitgelieferte Branding-Basisdatei mit; das Top-Level-Feld `branding-css` in `app-package.json` kann auf eine solche Datei zeigen.

## CORS und ODAS-Proxy

Viele offene Datenquellen lassen sich direkt im Browser laden. Wenn eine Quelle CORS blockiert oder aus Sicherheitsgründen nicht direkt angesprochen werden soll, kann der ODAS-Proxy verwendet werden.

Für proxy-fähige Apps wird der Konfigurationsschalter `proxyAktiv` empfohlen:

```json
"proxyAktiv": {
  "label": "ODAS-Proxy aktivieren",
  "hilfe": "Mit ja werden Datenabrufe über den ODAS-Proxy gesendet. Echte Proxy-Aufrufe sind nur im ODAS-Live-System prüfbar.",
  "default": "nein",
  "format": {
    "typ": "dropdown",
    "optionen": ["nein", "ja"]
  },
  "erforderlich": "ja"
}
```

Der Schalter ist keine allgemeine Pflicht. Er ist sinnvoll, wenn Portalbetreiber bewusst zwischen Direktabruf und Proxy-Betrieb wählen sollen.

Der Proxy-Aufruf verwendet `POST`. Im URL-kodierten Parameter `path` wird nur der Pfad einschließlich Query der Zielressource übertragen, nicht die vollständige externe URL. Die Proxy-Antwort enthält die Nutzdaten als String im Feld `content`. Echte Proxy-Antworten werden im ODAS-Live-System getestet; lokal werden Konfiguration, Statusanzeige und Direktmodus geprüft.

## Auslieferung

Der Lieferumfang wird durch das jeweilige `Makefile` bestimmt. In der aktuellen `oda-generic`-Vorlage packt `make zip`:

- `app/`
- `assets/`
- `app-package.json`
- `CHANGELOG.md`

Dateien wie `odas-config/`, lokale Tests, Tools oder Demo-Daten sind nur dann online verfügbar, wenn das aktive `Makefile` sie ausdrücklich in die ZIP-Datei aufnimmt.

Die Paketversion muss mit dem obersten versionierten Eintrag in `CHANGELOG.md` übereinstimmen. Für neue Einträge wird das Format `## X.Y.Z - YYYY-MM-DD` verwendet.

## Gekürztes Beispiel `app-package.json`

{% raw %}
```json
{
  "app-entwickler-id": "12343",
  "app-entwickler-name": "ondics-gmbh",
  "name-in-url": "generic",
  "name": "Generic Open Data App",
  "version": "1.1.0",
  "odas-app-icon": "assets/odas-app-icon.svg",
  "app-icon": "assets/odas-app-icon.svg",
  "kurzbeschreibung": "Technische ODAS-Vorlage mit Routing, Instanz-Konfiguration und Proxy-Muster.",
  "beschreibung": [
    "_multiline_",
    "Die Generic Open Data App ist die technische Ausgangsbasis fuer neue ODAS-Apps.",
    "Vor einer Veroeffentlichung werden Fachlogik, Metadaten und Datenmodell ersetzt."
  ],
  "tags-funktion": ["Vorlage", "Konfigurationsvorschau", "Proxy"],
  "tags-dateityp": ["JSON"],
  "tags-datenart": ["Konfigurationsdaten"],
  "screenshots": [
    "assets/Desktop_Screenshot.png",
    "assets/Mobile_Screenshot.png"
  ],
  "daten": {
    "beschreibung": "Die Vorlage enthaelt ein gueltiges Frictionless-Beispielschema.",
    "schema": "assets/schema.json",
    "beispiel": "Vor der Veroeffentlichung durch app-spezifische Angaben ersetzen.",
    "beispiel-url": ""
  },
  "branding-css": "",
  "api-version": "1",
  "instanz-config": {
    "seitentitel": {
      "label": "Seitentitel",
      "hilfe": "Der Seitentitel wird im Browser-Tab der App angezeigt.",
      "default": "Generic Open Data App",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "titel": {
      "label": "Titel",
      "hilfe": "Der Titel wird in der Titelzeile der App angezeigt.",
      "default": "Generic Open Data App",
      "format": {
        "typ": "string",
        "laenge": 50
      },
      "erforderlich": "ja"
    },
    "icon": {
      "label": "App-Icon",
      "hilfe": "Das Icon wird links oben in der Titelzeile angezeigt.",
      "default": "{{{odp.logo}}}",
      "format": {
        "typ": "image",
        "hoehe": 100,
        "breite": 300
      },
      "erforderlich": "ja"
    },
    "kontakt": {
      "label": "Kontakt",
      "hilfe": "Der Text wird im Menuepunkt Kontakt angezeigt.",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "<p>Bei Fragen zur App wenden Sie sich bitte an die im Open Data Portal hinterlegte Kontaktstelle.</p>"
      ],
      "erforderlich": "ja"
    },
    "beschreibung": {
      "label": "Beschreibung",
      "hilfe": "Der Text wird im Menuepunkt Ueber diese App angezeigt.",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "<h2>Ueber diese Vorlage</h2>",
        "<p>Diese App zeigt die wirksame Instanz-Konfiguration.</p>",
        "<h2>Datenquelle</h2>",
        "<p>Die Generic-App laedt absichtlich keinen Fachdatenbestand.</p>"
      ],
      "erforderlich": "ja"
    },
    "impressum": {
      "label": "Impressum",
      "hilfe": "Der Text wird im Menuepunkt Impressum angezeigt.",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "<h2>Anbieter</h2>",
        "<p>{{{odp.anbieter.orgName}}}<br>{{odp.anbieter.strasse}}<br>{{odp.anbieter.plzort}}</p>"
      ],
      "erforderlich": "ja"
    },
    "datenschutz": {
      "label": "Datenschutz",
      "hilfe": "Der Text wird im Menuepunkt Datenschutz angezeigt.",
      "format": {
        "typ": "markdown"
      },
      "default": [
        "_multiline_",
        "<p>Massgeblich sind die Datenschutzangaben des jeweiligen Portalbetreibers.</p>"
      ],
      "erforderlich": "ja"
    },
    "fusszeile": {
      "label": "Fusszeile",
      "hilfe": "Wird unten in der App angezeigt.",
      "format": {
        "typ": "string",
        "laenge": 180
      },
      "default": "© {{jahr}} | App und Daten: {{odp.anbieter.name}} | Entwicklung: {{app.developer.name}} | ODAS: {{odas.betreiber.name}}",
      "erforderlich": "ja"
    },
    "brandingCSS": {
      "label": "Zusaetzliches Branding-CSS",
      "hilfe": "Optionaler CSS-Code fuer die konkrete Instanz.",
      "default": "",
      "format": {
        "typ": "string",
        "laenge": 10000
      },
      "erforderlich": "nein"
    },
    "brandingCSSFile": {
      "label": "Branding-CSS-Datei",
      "hilfe": "Optionale URL zu einer Branding-CSS-Datei.",
      "default": "",
      "format": {
        "typ": "url",
        "laenge": 500
      },
      "erforderlich": "nein"
    },
    "urlDaten": {
      "label": "URL zum Datensatz",
      "hilfe": "Katalogseite des tatsaechlich verwendeten Datensatzes.",
      "default": "",
      "format": {
        "typ": "url",
        "laenge": 500
      },
      "erforderlich": "nein"
    },
    "apiurl": {
      "label": "URL zu den Daten",
      "hilfe": "Von dieser URL werden die Daten der App bezogen.",
      "format": {
        "typ": "url",
        "laenge": 255
      },
      "default": "",
      "beispiel": "",
      "erforderlich": "nein"
    },
    "proxyAktiv": {
      "label": "ODAS-Proxy aktivieren",
      "hilfe": "Steuert direkte oder proxygestuetzte Datenabrufe.",
      "default": "nein",
      "format": {
        "typ": "dropdown",
        "optionen": ["nein", "ja"]
      },
      "erforderlich": "ja"
    }
  }
}
```
{% endraw %}

## Schale 4: Verständlichkeitsebene

Schale 4 ist eine optionale Ebene, die Apps für Bürgerinnen und Bürger ohne Datenfachwissen verständlicher macht: Kontext zu Kennzahlen, Methodik-Transparenz, Datenaktualität und weiterführende Links. Sie ist keine ODAS-Pflicht, sondern Teil des Empfohlen-/Ondics-Standards und in nahezu allen Ondics-Apps umgesetzt.

Grundprinzip: Jede konfigurierbare Komponente rendert nur, wenn ihr Feld gefüllt ist; ein leerer Wert blendet die Komponente aus. Alle Felder verwenden die [für ODAS-v1 robusten `format.typ`-Werte](#instanz-config) und sind mit `erforderlich: "nein"` deklariert. Die Komponenten sind Opt-in und werden nur aufgenommen, wenn sie für die konkrete App sinnvoll sind.

### KPI-Kontexttexte

Für jeden KPI-Slot kann ein Feld `kpiKontext1`, `kpiKontext2`, ... (Typ `string`) einen kurzen Erklärtext liefern. Die Referenz-Apps zeigen ihn über ein kleines ⓘ-Icon zum Aufklappen direkt an der Kennzahl, damit KPI-Reihen kompakt bleiben.

### Methodikbox

Eine ausklappbare Sektion im Hauptinhalt erklärt Herkunft, Erhebungsmethode und Limitierungen der Daten. Sie wird aus zwei Feldern gespeist: `datenquelleHinweis` (Typ `markdown`) für den Erklärtext und `datenStand` (Typ `string`) für eine Freitext-Angabe wie "Stand: Januar 2026". `datenStand` ist bewusst ein `string` und kein Datumstyp.

### Datenaktualitäts-Indikator

Wenn die Datenquelle einen Zeitstempel liefert (z.B. CKAN `metadata_modified` oder der neueste Datensatz), zeigt die App ihn prominent im Inhaltsbereich an. Dafür gibt es **bewusst kein Konfigurationsfeld**: Der Wert wird quellspezifisch aus der API-Antwort abgeleitet. Liefert die Quelle keinen Zeitstempel, entfällt die Anzeige.

### Weiterführende Links

Das Feld `weiterfuehrendeLinks` (Typ `markdown`) enthält eine vom Portalbetreiber gepflegte Liste verwandter Datensätze oder Hintergrundquellen. Die App rendert daraus einen Abschnitt "Weitere Informationen" am Ende der Seite.

### "Für wen ist diese App?"

Ein kurzer Absatz innerhalb des bestehenden `beschreibung`-Felds benennt die Zielgruppe und stellt klar, dass kein besonderes Datenfachwissen nötig ist. Dafür wird **kein eigener Config-Key** angelegt.

### Datenquellen-Linkliste

Die Datenquellenbeschreibung in `beschreibung` verlinkt dreistufig: Open Data Portal → Datensatz → Ressource(n). Die URLs werden aus den Datenquellen-Configwerten der App abgeleitet (`urlDaten` für die Datensatzseite, `apiurl` für die Ressource), damit verlinkte und tatsächlich geladene Quelle identisch sind. Ebenen ohne Wert werden weggelassen, nicht mit Platzhaltern verlinkt.

### Beispiel

```json
"kpiKontext1": {
  "label": "KPI-Kontext 1",
  "hilfe": "Optionaler Erklärtext zum ersten KPI-Wert. Leer = kein Kontext.",
  "format": { "typ": "string", "laenge": 160 },
  "default": "",
  "erforderlich": "nein"
},
"datenquelleHinweis": {
  "label": "Methodik / Datenquelle-Hinweis",
  "hilfe": "Herkunft, Erhebungsmethode, Limitierungen (Markdown). Leer = ausgeblendet.",
  "format": { "typ": "markdown" },
  "default": "Die Werte stammen aus dem **amtlichen Datenbestand** und werden monatlich aktualisiert.",
  "erforderlich": "nein"
},
"datenStand": {
  "label": "Datenstand",
  "hilfe": "Freitext-Datum, z.B. 'Stand: Januar 2026'. Leer = ausgeblendet.",
  "format": { "typ": "string", "laenge": 60 },
  "default": "",
  "erforderlich": "nein"
},
"weiterfuehrendeLinks": {
  "label": "Weiterführende Links",
  "hilfe": "Verwandte Datensätze / Hintergrundquellen (Markdown). Leer = ausgeblendet.",
  "format": { "typ": "markdown" },
  "default": "- [Open Data Portal](https://opendata.example.org)\n- [Datensatz im Katalog](https://opendata.example.org/dataset/beispiel)",
  "erforderlich": "nein"
}
```

Die `markdown`-Defaults sind hier bereits in Markdown verfasst, konform zum oben beschriebenen Standard. Bestands-Apps verwenden in diesen Feldern noch HTML.

## Ondics-Standard

Für Apps, die als Referenz, Showcase oder produktiver Ondics-Beitrag entstehen, gelten zusätzliche Qualitätsziele. Dazu gehören:

- App-Logik und Styling klar in `app/app.js` und `app/app.css` trennen.
- README, `assets/schema.json`, Icon, Screenshots und `CHANGELOG.md` app-spezifisch pflegen.
- Konfigurationsfelder minimal halten und nur aufnehmen, wenn Portalbetreiber sie wirklich ändern sollen.
- Schale-4-Komponenten (Verständlichkeitsebene) umsetzen, wo sie für die App sinnvoll sind.
- Keine Platzhalter-Metadaten in gelieferten Apps, z.B. Template-Entwicklername oder generische Tags.
- Datenabrufe, Fehlerzustände, leere Daten, Filter, Tabellen, Karten und responsive Darstellung lokal testen.
- Den ODAS-Proxy lokal nur auf Verdrahtung prüfen; echte Proxy-Antworten werden im ODAS-Live-System geprüft.

Diese Punkte sind nicht automatisch harte Anforderungen an alle externen Entwickler. Sie beschreiben unseren Qualitätsanspruch für vorzeigbare ODAS-Apps.

---

[zurück zum Index]({{ site.baseurl }}/index)
