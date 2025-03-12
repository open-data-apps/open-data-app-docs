---
layout: default
title: Open Data App "generic"
permalink: /oda-generic/
---

Die Open Data App "generic" ist als Startpunkt für eigene ODAs konzipiert.

Sie ist Vorlage und Musterbeispiel einer ODA und entspricht den Vorgaben der [ODA Spezifikation](open-data-app-spezifikation).

## Inhalt

- Dateien und Verzeichnisaufbau
- Grundfunktionen
- Wie und wohin schreibe ich meinen Code?
- Tipps & Tricks

## Dateien und Verzeichnisaufbau

Die Applikation ist klar strukturiert. Nachfolgend die Übersicht der wichtigsten Dateien und Verzeichnisse:

```
odas-app-generic/
├── app/                    # Frontend-Anwendung
│   ├── app-base.js        # Basis-JavaScript-Funktionen
│   ├── app.js             # Haupt-JavaScript Datei (siehe Hinweis zur Wartung)
│   ├── app.css            # Haupt-CSS für die App
│   ├── favicon.png        # Favicon der Anwendung
│   ├── index.html         # Hier wird der Content dynamisch eingefügt durch die .js Dateien
│   ├── logo_ondics.png    # Logo der Anwendung
│   └── app-base.css       # Basis-CSS für generelle Styles (siehe Hinweis zur Wartung)
└── assets/                # Zusätzliche Assets
    ├── odas-app-icon.svg  # Icon der App
    ├── Desktop_Screenshot.png # Screenshot der Desktop-Ansicht
    └── Mobile_Screenshot.png  # Screenshot der Mobile-Ansicht
├── odas-config/            # Konfigurationsverzeichnis
│   └── config.json        # Zentrale Konfigurationsdatei der App zum lokalen Entwickeln
├── .gitignore              # Dateien, die von Git ignoriert werden
├── app-package.json        # App-spezifische Definitionen woraus die config.json erstellt wird beim Upload zum Open Data Appstore
├── CHANGELOG.md            # Änderungsprotokoll der App
├── docker-compose.yml      # Konfiguration für Docker Compose
├── Dockerfile              # Docker Build Datei zur Erstellung des App-Images
├── Makefile                # Skripte und Befehle für Build- und Wartungsprozesse
├── nginx.conf              # Nginx Konfigurationsdatei
├── odas-app-generic.zip    # Zip-Datei zur Auslieferung der App an das ODAS Portal
├── README.md               # Allgemeine Dokumentation und Einführung zur App
├── .vscode/               # VSCode-spezifische Einstellungen
│   └── settings.json      # Konfiguration des Ports für das Live Server Plugin von VS Code (Siehe Tipps & Tricks)
```

## Grundfunktionen

Die Open Data App "generic" bietet folgende Basisfunktionen:

- **Grundgerüst**: Die Generic App bietet ein Menü mit allen vorgeschriebenen Seiten: Beschreibung der App, Kontakt, usw. die über die `config.json` angepasst werden können.
- Alles ist fertig konfiguriert nach den ODAS Spezifikationen. Theoretisch muss nur in der `app.js` Code ergänzt werden, die `app-package.json` Datei dementsprechend angepasst werden sowie die `README.md` Datei.
- **app.js**: Enthält folgende Basisvariablen `configdata` und `enclosingHtmlDivElement`. Über `configdata` sind alle Variablen abrufbar aus dem `config.json`. Mit dem `enclosingHtmlDivElement` kann der Hauptinhalt der Startseite dynamisch angepasst werden.
- Zudem enthält die Datei eine Funktion `addToHead` dort können mit JavaScript Stylesheets oder Skripte im Head der `index.html` hinzugefügt werden.
- **Styling**: Als CSS Framnework wird Bootstrap 5.3 verwendet.
- **Containerisierung & Deployment**: Mit Hilfe von `Dockerfile` und `docker-compose.yml` kann die App einfach in Containerumgebungen betrieben werden.
- **Konfiguration**: Einstellungen können in `odas-config/config.json` vorgenommen werden, z.B.: Titel, Footer, Link zu Daten aus Open Data Portal, usw.
- **Frontend-Rendering**: Die Benutzeroberfläche wird über `app/index.html` bereitgestellt und mit den JavaScript-Dateien (`app/app.js` und `app/app-base.js`) sowie den CSS-Dateien (`app/app.css` und `app/app-base.css`) dynamisch und responsiv umgesetzt.
- **Modularität**: Der Code für die Inhalte ist in `app/app.js` vorgesehen – dies dient der einfachen Wartung, ist aber nicht zwingend vorgeschrieben (Siehe Tipps & Tricks).

## Wie und wohin schreibe ich meinen Code?

- **HTML**: Die Basisstruktur der Benutzeroberfläche befindet sich in `app/index.html`.
- **CSS**: Anpassungen am Design erfolgen in `app/app.css` und `app/app-base.css`.
- **JavaScript**: Der inhaltliche Code der App wird in `app/app.js` oder `app/app-base.js` geschrieben.  
  **Hinweis**: Es ist vorgesehen, dass der Hauptinhalt der App in `app/app.js` gepflegt wird. Dies ist keine Pflicht, jedoch wird es empfohlen, um eine einfache Wartung und Erweiterbarkeit sicherzustellen.
- **Konfiguration**: Einstellungen können in `odas-config/config.json` vorgenommen werden.

## Tipps & Tricks

- **Docker-Umgebung nutzen**: Mit den bereitgestellten `Dockerfile` und `docker-compose.yml` Dateien kannst du eine konsistente Entwicklungs- und Produktionsumgebung aufbauen.
- **Live Server Extension** Die Live Server Extension ermöglicht ebenfalls eine gute alternative zu Docker zum entwicklen [Live Server Extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- **KI-Prompt für App.js**: Nutze KI-Prompts, um den Code in `app/app.js` zu optimieren und zu erweitern. Detaillierte Anleitungen findest du in der [KI-Prompt Dokumentation](oda-ki).
- **Modularer Aufbau**: ODAS Tools ermöglichen einen Austausch aller Base-Dateien in allen Apps gleichzeitig – mithilfe eines Makefiles, das auch weitere Entwicklerhilfen enthält. Weitere Informationen dazu findest du unter [ODAS Tools](#placeholder).

---

[zurück zum Index](index)
