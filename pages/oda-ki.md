---
layout: default
title: Automatisierte Erstellung einer ODA mit KI
permalink: /oda-ki/
---

# Automatisierte Erstellung einer ODA mit KI

ODAs sind strukturell so aufgebaut, dass eine Automatisierung der Inhaltserstellung möglich ist: statische Dateien, eine ODAS-Konfiguration und ein begrenzter Inhaltsbereich.

Das Ziel dabei ist, dass ODAs einfach erstellt werden können, um dann in möglichst vielen Open Data Portalen zum Einsatz zu kommen. Motto: Je niedriger die Entwicklungskosten sind, desto mehr ODAs entstehen. Und je größer die Auswahl an ODAs wird, desto mehr bekommen Rohdaten neues Leben.

## Was muss die KI tun?

Die KI soll:

- Die Vorlage `oda-generic` analysieren und den erforderlichen JavaScript-Code generieren.
- Die Funktion `app(configdata, enclosingHtmlDivElement)` so implementieren, dass der Inhalt basierend auf den Konfigurationsdaten (z.B. der `apiurl`) erstellt wird.
- Sicherstellen, dass der generierte Inhalt ausschließlich in das übergebene HTML-Element (`enclosingHtmlDivElement`) geladen wird.
- Falls benötigt, über die Funktion `addToHead()` oder dedizierte Loader-Funktionen externe Skripte und Stylesheets einbinden.
- Die app-spezifischen Metadaten (`app-package.json`, `odas-config/config.json`, bei Showcase-Qualität auch Schema, Icon, README und Changelog) konsistent pflegen.

## Vor dem Start

Wichtigste Grundlage: Vor der Ausführung des Prompts sollte die Vorlage [`oda-generic`](https://github.com/open-data-apps/oda-generic) bereits als lokale Kopie vorliegen (z.B. per `git clone`, danach den `.git`-Ordner der Kopie entfernen). Der Prompt setzt voraus, dass die Template-Dateien im Arbeitsverzeichnis vorhanden sind. Ohne diese Grundlage funktioniert der Prompt nicht wie gewollt.

Außerdem sollten diese Informationen vor der Codegenerierung bekannt sein:

- App-Name und kurze Beschreibung.
- Datenquelle: API-URL, direkte Datei-URL, CKAN-Ressource oder andere Quelle.
- Datenformat: JSON, GeoJSON, CSV oder ein anderes Format.
- Relevante Felder, Filter, Kennzahlen, Tabellen, Karten oder Diagramme.
- Ob die Datenquelle direkt im Browser ladbar ist oder voraussichtlich den ODAS-Proxy braucht.
- Zielgruppe der App und gewünschte Schale-4-Komponenten: KPI-Kontexttexte, Methodikbox, weiterführende Links (siehe [ODA-Spezifikation]({{ site.baseurl }}/open-data-app-spezifikation)).

Die KI sollte nicht automatisch neue Konfigurationsfelder erfinden. Konfigurationsfelder sind sinnvoll, wenn Portalbetreiber sie pro App-Instanz wirklich ändern sollen.

## Prompt-Vorlage

Dieser Prompt deckt die wichtigsten Konventionen ab. Der vollständige Ondics-Standard steht in der [ODA-Spezifikation]({{ site.baseurl }}/open-data-app-spezifikation).

```text
Agiere als Softwareentwickler fuer eine Open Data App im Open Data App Store.

Voraussetzung: Die Vorlage oda-generic liegt bereits als Kopie im Projektverzeichnis vor.
Arbeite direkt in dieser Kopie und erfinde keine eigene Projektstruktur.

Die App basiert auf der Vorlage oda-generic:
- Die App ist statisch und nutzt HTML, CSS und Vanilla JavaScript.
- Bootstrap 5.3 ist ueber die Vorlage verfuegbar.
- Header, Footer, Navigation und ODAS-Konfiguration werden von der Vorlage geladen.
- Der app-spezifische Inhalt wird in app/app.js in der Funktion app(configdata, enclosingHtmlDivElement) umgesetzt.
- App-spezifisches Styling kommt in app/app.css.
- Die Funktion addToHead() steht ausserhalb und nach app().
- Template-Dateien wie app/app-base.js, app/app-base.css und app/index.html werden nicht geaendert.
- Leaflet, Chart.js und aehnliche Bibliotheken werden dynamisch ueber Loader-Funktionen geladen.

Die App soll folgendes tun:
[BESCHREIBUNG DER FUNKTION]

Die Datenquelle ist:
[URL ODER BESCHREIBUNG DER DATENQUELLE]

Das Datenformat ist:
[JSON, GeoJSON, CSV, CKAN Datastore, ...]

Erwartete Felder:
[FELDER UND BEDEUTUNG]

Erstelle oder aktualisiere:
- app/app.js
- app/app.css, falls app-spezifisches Styling noetig ist
- app-package.json mit passenden Metadaten und instanz-config
- odas-config/config.json als lokale Testkonfiguration
- assets/schema.json, README.md und CHANGELOG.md bei Showcase-Qualitaet

Konfigurationsregeln:
- Jeder in app/app.js gelesene configdata-Key steht in app-package.json unter instanz-config.
- odas-config/config.json spiegelt diese app-spezifischen Keys fuer lokale Tests.
- Fuer ODAS-v1 nur die format.typ-Werte string, url, dropdown, markdown und image verwenden, exakt kleingeschrieben.
- Zahlen in instanz-config als Strings speichern und im Code umwandeln.
- Felder mit format.typ markdown in echtem Markdown verfassen (Ueberschriften mit ##, Listen mit -, Links mit [Text](URL)); kein HTML in neuen Defaults.
- Nutze nur Konfigurationsfelder, die fuer Portalbetreiber wirklich relevant sind.

Schale 4 (Verstaendlichkeit, optional):
- Konfigurierbare Komponenten nur aufnehmen, wenn sie umgesetzt werden; leere Felder blenden die Komponente aus.
- KPI-Kontexte als kpiKontext1, kpiKontext2, ... (string); Methodikbox aus datenquelleHinweis (markdown) und datenStand (string); weiterfuehrende Links als weiterfuehrendeLinks (markdown).
- Der Datenaktualitaets-Indikator wird ohne Config-Feld aus der Antwort der Datenquelle abgeleitet.
- Ein Absatz "Fuer wen ist diese App?" steht innerhalb des Felds beschreibung, ohne eigenen Config-Key.
- Die Datenquellenbeschreibung verlinkt dreistufig: Open Data Portal, Datensatz, Ressource(n).

CORS/Proxy:
- Wenn Direktabrufe scheitern koennen, nutze den ODAS-Proxy.
- Empfohlen ist proxyAktiv als dropdown mit nein/ja, wenn Portalbetreiber den Modus steuern sollen.
- Proxy-Aufrufe gehen per POST an /odp-data?path=<url-encodierter-pfad-mit-query>.
- Der Proxy liefert content als String; diesen danach als JSON, CSV oder Text parsen.
- Lokale Tests pruefen nur Verdrahtung und Direktmodus. Echte Proxy-Responses werden im ODAS-Live-System geprueft.

Lokales Testen:
- Variante Live Server: Projektwurzel oeffnen und die App unter http://127.0.0.1:<port>/app/ aufrufen.
- Variante Docker: make build und make up ausfuehren; die App laeuft dann unter dem in docker-compose.yml konfigurierten Port. Stoppen mit make down.
- Unter 127.0.0.1 oder localhost laedt die App automatisch die lokale ../odas-config/config.json.
- Vor der Abgabe pruefen: node --check app/app.js sowie python3 -m json.tool fuer app-package.json und odas-config/config.json.

Qualitaetsgate:
- Keine Generic-Platzhalter in gelieferten Dateien.
- Fehlerzustand, leere Daten, Ladezustand und responsive Darstellung beruecksichtigen.
- Lieferumfang gegen das aktive Makefile pruefen.
```

## App-Icon mit KI erstellen

Auch das App-Icon lässt sich mit KI erzeugen. Eine getestete Prompt-Vorlage dafür liegt im Repo [odas-app-icon-prompt](https://github.com/open-data-apps/odas-app-icon-prompt/blob/main/odas_icon_prompt.md) auf GitHub.

## CORS und `proxyAktiv`

`proxyAktiv` ist ein empfohlenes Muster, keine allgemeine Pflicht. Es ist besonders nützlich, wenn eine App je nach Portal oder Datenquelle im Direktmodus oder über den ODAS-Proxy laufen soll.

Beispiel für `instanz-config`:

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

## Checkliste für KI-Ergebnisse

- Ist `app-package.json` gültiges JSON?
- Passen `app-package.json` und `odas-config/config.json` bei den app-spezifischen Keys zusammen?
- Liest `app/app.js` nur dokumentierte Konfigurationswerte?
- Sind Felder mit `format.typ: "markdown"` in echtem Markdown verfasst?
- Sind Schale-4-Felder nur vorhanden, wenn die Komponente umgesetzt ist, und blendet ein leerer Wert sie aus?
- Wird der Datenaktualitäts-Indikator ohne Config-Feld aus der Datenquelle abgeleitet?
- Werden echte Datenstruktur und Fehlerfälle verarbeitet?
- Ist `app/app.css` nur für app-spezifisches Styling zuständig?
- Ist der ODAS-Proxy nur dort eingebaut, wo er nötig oder sinnvoll ist?
- Sind README, Schema und Icon app-spezifisch, wenn es eine Showcase-App ist?
- Läuft die lokale Vorschau mit Live Server (`http://127.0.0.1:<live-server-port>/app/`) oder Docker (`make up`)?

---

[zurück zum Index]({{ site.baseurl }}/index)
