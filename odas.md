# Der Open Data App Store (ODAS)

Im ODAS können App-Entwickler ODAs veröffentlichen.
Zudem können Open Data Portalbetreiber diese ODAs ihrem Portal zuordnen.

Für einfache Interaktionen steht im ODAS eine Email-API zur Verfügung.

Emails dürfen nur an die im ODAS konfigurierte Email-Adresse versendet werden.
----------------> können wir das sicherstellen oder die API ganz ohne Empfänger angeben?

Der ODAS ist aktuell erreichbar unter https://open-data-app-store.ckan.de/

## ODAS für Open Data Portalbetreiber

Im ODAS können Open Data Portalbetreiber

- ihr Open Data Portal (ODP) registrieren
- für das ODP ein oder mehrere ODAs "buchen"/zuordnen
- die ODA für das eigene ODP konfigurieren (z. B. einen Open Data Datensatz zuordnen)
- die zugeordneten ODAs mit den Daten aus dem ODP testen

## ODAS für ODA-Entwickler

Im ODAS können ODA-Entwickler

- eine oder mehrere ODAs veröffentlichen
- ODAs testen (Todo: wie???)
- ODA-Updates veröffentlichen
- ODA-Statistiken ansehen (wie oft und von wem wurde die ODA "gebucht"?)

## Veröffentlichung einer ODA im ODAS

Eine ODA muss eine Datei `/app_package.json` haben. In der sind
alle Metadaten zur App (Lizenz, Version, Konfiguration, ...) enthalten.

Die Datei `/app_package.json`

- muss folgende Top-Level Elemente enthalten:
  - app-entwickler: siehe ODAS
  - app-entwickler-name: siehe ODAS
  - name-in-url: Teil der URL
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
