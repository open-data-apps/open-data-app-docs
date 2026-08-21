---
layout: default
title: Open Data App Generic
permalink: /oda-generic/
---

# Open Data App Generic

Die Open Data App **generic** ist als Startpunkt für eigene ODAs konzipiert. Sie ist Vorlage und Musterbeispiel einer ODA und erfüllt die [ODA-Spezifikation]({{ site.baseurl }}/open-data-app-spezifikation). Der aktuelle Referenzstand ist **Version 1.1.0 vom 10.07.2026**.

`oda-generic` ist eine Empfehlung, nicht die einzige denkbare ODAS-Implementierung. Wer die Vorlage nutzt, bekommt ein fertiges Grundgerüst mit Header, Navigation, Inhaltsbereich, Footer und Konfigurationsladung und profitiert von einem einheitlichen Aufbau und leichterer Prüfung.

## Inhalt

- Dateien und Verzeichnisaufbau
- Grundfunktionen
- Wie und wohin schreibe ich meinen Code?
- Konfiguration
- Lokale Entwicklung
- CORS und Proxy
- Tipps & Tricks
- Prüfung und Auslieferung

## Dateien und Verzeichnisaufbau

Die Vorlage ist klar strukturiert. Nachfolgend die Übersicht der wichtigsten Dateien und Verzeichnisse:

```text
oda-generic/
├── app/                        # Frontend-Anwendung
│   ├── app-base.js             # Basis-JavaScript: Routing, Konfigurationsladung, Fehleranzeige (nicht ändern)
│   ├── app-base.css            # Basis-CSS für generelle Styles (nicht ändern)
│   ├── app.js                  # Haupt-JavaScript: hier kommt der eigene App-Code hinein
│   ├── app.css                 # App-spezifisches CSS
│   ├── index.html              # Grundgerüst; der Inhalt wird dynamisch eingefügt (nicht ändern)
│   ├── favicon.png             # Favicon der Anwendung
│   └── logo_ondics.png         # Beispiel-Logo
├── assets/                     # Zusätzliche Assets
│   ├── odas-app-icon.svg       # Icon der App für den ODAS
│   ├── Desktop_Screenshot.png  # Screenshot der Desktop-Ansicht
│   ├── Mobile_Screenshot.png   # Screenshot der Mobile-Ansicht
│   ├── branding.css            # Branding-Basisdatei
│   └── schema.json             # Frictionless-Beispielschema; pro App durch das echte Datenmodell ersetzen
├── odas-config/                # Lokale Konfiguration
│   └── config.json             # Testkonfiguration für die lokale Entwicklung
├── .vscode/                    # VS-Code-Einstellungen
│   └── settings.json           # u. a. Port für die Live Server Extension
├── app-package.json            # ODAS-Metadaten; hieraus erzeugt der ODAS die Instanz-Konfiguration
├── CHANGELOG.md                # Änderungsprotokoll der App
├── README.md                   # Dokumentation und Einführung zur App
├── Dockerfile                  # Docker-Build der App (Nginx)
├── docker-compose.yml          # Konfiguration für Docker Compose
├── nginx.conf                  # Nginx-Konfigurationsdatei
└── Makefile                    # Build- und Wartungsbefehle, u. a. make zip
```

Nach `make zip` entsteht zusätzlich eine ZIP-Datei für die Auslieferung an den ODAS. Sie enthält `app/`, `assets/`, `app-package.json` und `CHANGELOG.md`.

## Grundfunktionen

Die Open Data App "generic" bietet in Version 1.1.0 folgende Basisfunktionen:

- **Fertiges Grundgerüst**: Menü mit allen vorgeschriebenen Seiten (Beschreibung, Kontakt, Datenschutz, Impressum), Header, Footer und Inhaltsbereich. Alles ist über die Konfiguration anpassbar.
- **Alles vorkonfiguriert**: Die Vorlage erfüllt die ODAS-Spezifikation. Theoretisch müssen nur `app/app.js` mit eigenem Code gefüllt sowie `app-package.json` und `README.md` angepasst werden.
- **Hash-Routing**: Unterseiten wie `#startseite`, `#beschreibung` oder `#kontakt` sind teilbar und funktionieren mit den Browser-Tasten Vor/Zurück; das Portal-Logo führt zur Startseite.
- **Konfigurationsanbindung**: Über `configdata` sind alle konfigurierten Werte abrufbar; mit `enclosingHtmlDivElement` wird der Hauptinhalt der Startseite dynamisch gefüllt.
- **Fehleranzeige**: Laufzeitfehler werden sichtbar und HTML-maskiert im Inhaltsbereich ausgegeben statt still zu scheitern.
- **Styling**: Als CSS-Framework wird Bootstrap 5.3.8 verwendet.
- **Proxy-Hilfsfunktionen**: Fertige Funktionen für direkten Datenabruf und den ODAS-Proxy (siehe unten).
- **Containerisierung**: Mit `Dockerfile` und `docker-compose.yml` läuft die App auch in Containerumgebungen.

## Wie und wohin schreibe ich meinen Code?

- **JavaScript**: Der inhaltliche Code der App wird in `app/app.js` geschrieben. Die Vorlage ruft dort die Funktion `app(configdata, enclosingHtmlDivElement)` auf und wartet auch eine zurückgegebene Promise ab.
- **CSS**: App-spezifisches Styling kommt in `app/app.css`.
- **Nicht anfassen**: `app/app-base.js`, `app/app-base.css` und `app/index.html` bilden die Laufzeitvorlage und müssen für normale App-Entwicklung nicht geändert werden. Gleiches gilt normalerweise für `Dockerfile`, `docker-compose.yml`, `nginx.conf` und `Makefile`.
- **Bibliotheken**: Wenn Leaflet, Chart.js oder Ähnliches benötigt wird, werden sie über dedizierte Loader-Funktionen in `app/app.js` geladen. Die Funktion `addToHead()` steht außerhalb von `app()` und kann zusätzliche Stylesheets oder Skripte in den `<head>` einfügen.

Empfohlenes Grundmuster:

```javascript
function app(configdata = {}, enclosingHtmlDivElement) {
  enclosingHtmlDivElement.innerHTML = `
    <section class="container py-3">
      <h2>${configdata.titel || "Open Data App"}</h2>
      <p>Hier wird der App-Inhalt gerendert.</p>
    </section>
  `;
}

function addToHead() {}
```

## Konfiguration

Produktiv erzeugt der ODAS die Konfiguration aus `app-package.json` und den Angaben der App-Instanz. Beim Anlegen einer App-Instanz übernimmt der ODAS zunächst die `default`-Werte und die in der `app-package.json` eingetragenen Werte; der Portalbetreiber passt sie danach über das Konfigurationsformular an. Lokal wird diese Konfiguration über `odas-config/config.json` gespiegelt. Dort können z.B. Titel, Footer oder der Link zu den Daten aus dem Open Data Portal zum Testen eingetragen werden.

Empfohlen ist:

- Jeder Key, den `app/app.js` liest, steht in `app-package.json` unter `instanz-config`.
- `odas-config/config.json` enthält dieselben app-spezifischen Keys für lokale Tests.
- Statische Texte, interne Konstanten und ableitbare Werte bleiben im Code statt als unnötige Konfigurationsfelder im ODAS-Formular.
- Basisfelder wie `titel`, `seitentitel`, `icon`, `fusszeile`, `kontakt`, `impressum`, `datenschutz`, `beschreibung`, `brandingCSS` und `brandingCSSFile` bleiben erhalten, wenn die Vorlage sie nutzt.
- Der Datenendpunkt heißt kanonisch `apiurl` und die Datensatzseite `urlDaten`; parallele Legacy-Schreibweisen werden nicht neu eingeführt.
- `format.typ` verwendet für ODAS-v1 nur `string`, `url`, `dropdown`, `markdown` und `image`.

Felder mit `format.typ: "markdown"` (z.B. `beschreibung`, `kontakt`, `impressum`, `datenschutz`) werden in Markdown verfasst; ODAS wandelt das Markdown vor der Auslieferung der Konfiguration in HTML um, das die Vorlage unverändert rendert. Im lokalen `odas-config/config.json` stehen diese Inhalte als HTML. Details stehen in der [ODA-Spezifikation]({{ site.baseurl }}/open-data-app-spezifikation).

## Lokale Entwicklung

Für schnelle Frontend-Iteration wird VS Code Live Server aus der Projektwurzel empfohlen:

```json
{
  "liveServer.settings.host": "127.0.0.1",
  "liveServer.settings.root": "/",
  "liveServer.settings.file": "app/index.html"
}
```

Dann die App unter `http://127.0.0.1:<live-server-port>/app/` öffnen. Live Server nutzt standardmäßig Port `5500`; Projekte können in `.vscode/settings.json` einen anderen Port festlegen.

Läuft die App unter `127.0.0.1` oder `localhost`, lädt `getConfigUrl()` in `app/app-base.js` automatisch die lokale `../odas-config/config.json`. Ein manuelles Umschalten ist dafür nicht nötig.

Docker bleibt als Alternative verfügbar:

```bash
make build
make up
```

## CORS und Proxy

Wenn eine Datenquelle direkte Browserabrufe blockiert, kann eine App den ODAS-Proxy nutzen. Für neue proxy-fähige Apps ist ein Schalter `proxyAktiv` als Dropdown mit `nein` und `ja` empfehlenswert, damit Portalbetreiber zwischen Direktabruf und Proxy-Betrieb wählen können.

Proxy-Aufrufe verwenden `POST`. Als Query-Parameter `path` wird ausschließlich der URL-kodierte Pfad einschließlich Query der Zielressource übertragen, nicht die vollständige externe URL. Der Proxy-Endpunkt liegt am App-Basispfad und funktioniert deshalb auch bei Aufrufen über `/app/index.html`.

Lokale Live-Server-Tests prüfen dabei vor allem:

- Konfiguration wird geladen.
- `proxyAktiv` wird korrekt gelesen.
- Direktmodus funktioniert, wenn CORS es erlaubt.
- Die App zeigt sinnvolle Hinweise, wenn Daten lokal nicht geladen werden können.

Echte Proxy-Antworten werden im ODAS-Live-System geprüft.

## Tipps & Tricks

- **Live Server Extension**: Die [Live Server Extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) für VS Code ist die schnellste Variante für lokale Entwicklung: Projektwurzel öffnen, App unter `/app/` aufrufen, fertig. Die lokale Konfiguration wird automatisch geladen.
- **Docker-Umgebung nutzen**: Mit den bereitgestellten `Dockerfile`- und `docker-compose.yml`-Dateien lässt sich eine konsistente Entwicklungs- und Produktionsumgebung aufbauen.
- **KI nutzen**: Der Inhalt von `app/app.js` lässt sich gut KI-gestützt erstellen und erweitern. Eine Anleitung mit Prompt-Vorlage steht auf der Seite [Automatisierte Erstellung einer ODA mit KI]({{ site.baseurl }}/oda-ki).
- **Schnelle Prüfbefehle**: `node --check app/app.js` findet Syntaxfehler, `python3 -m json.tool app-package.json` und `python3 -m json.tool odas-config/config.json` prüfen die Konfigurationsdateien.

## Prüfung und Auslieferung

Vor `make zip` sollte mindestens geprüft werden:

```bash
node --check app/app.js
node --check app/app-base.js
python3 -m json.tool app-package.json >/dev/null
python3 -m json.tool odas-config/config.json >/dev/null
python3 -m json.tool assets/schema.json >/dev/null
```

Zusätzlich werden Startseite, Hash-Routing, Logo-Link, Informationsseiten, Direktmodus und responsive Desktop-/Mobile-Ansicht im Browser geprüft.

## Ondics-Standard

Für Ondics-Showcase-Apps gehen wir strenger vor:

- App-spezifische Dateien enthalten keine Generic-Platzhalter.
- README, Schema, Icon und Screenshots sind auf die konkrete App zugeschnitten.
- `instanz-config` bleibt minimal und ODAS-v1-sicher.
- Die Schale-4-Komponenten aus der [ODA-Spezifikation]({{ site.baseurl }}/open-data-app-spezifikation) werden umgesetzt, wo sie für die App sinnvoll sind.
- Datenstruktur, Fehlerzustände, leere Daten, Filter, Tabellen, Karten und responsive Darstellung werden lokal geprüft.
- Der Lieferumfang wird gegen das aktive `Makefile` kontrolliert.

Diese Punkte sind unser Qualitätsstandard. Für externe Entwickler sind sie eine hilfreiche Checkliste, aber nicht in jedem Punkt harte ODAS-Pflicht.

---

[zurück zum Index]({{ site.baseurl }}/index)
