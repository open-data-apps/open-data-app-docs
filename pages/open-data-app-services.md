---
layout: default
title: Open Data App Services
permalink: /open-data-app-services/
---

# Open Data App Services

ODAS kann Apps Dienste bereitstellen, die nicht direkt in einer statischen Browser-App liegen sollten. Dadurch bleiben Apps klein, Schlüssel und Tokens müssen nicht im App-Code stehen und sicherheitskritische Abläufe können zentral kontrolliert werden.

Aktuell relevante Dienste:

- E-Mail-Service
- ODAS-Proxy
- KI-Service

## E-Mail-Service

Servicename: `mail`

Mit dem E-Mail-Service kann eine App Feedback oder Formularinhalte an die vom Portalbetreiber konfigurierte Adresse senden.

URL:

```text
https://open-data-app-store.de/view/<odp-name>/<app-name>/<instanz-id>/mail
```

Parameter:

- `content`: Inhalt der Nachricht.
- `emailCC`: optionale CC-Adresse.
- `userIP`: optionale IP-Information.

Apps sollten nur die im ODAS konfigurierte Empfängeradresse verwenden.

## ODAS-Proxy

Servicename: `proxy`

Der ODAS-Proxy hilft, wenn eine offene Datenquelle im Browser wegen CORS nicht direkt geladen werden kann oder wenn ein zentraler Abrufweg gewünscht ist.

Eine App ruft den Proxy relativ zur aktuellen App-Instanz auf:

```text
POST /odp-data?path=<url-encodierter-pfad-mit-query>
```

Wichtig:

- `path` enthält nur Pfad und Query der Ziel-URL, nicht die komplette Domain.
- Der `path`-Wert wird URL-encodiert.
- Die Proxy-Antwort enthält `content` als String.
- Die App parst `content` anschließend passend zur Quelle als JSON, CSV oder Text.

Für Apps mit steuerbarem Proxy-Modus ist `proxyAktiv` als `dropdown` mit `nein` und `ja` empfohlen. Der Schalter ist optional und sollte nur verwendet werden, wenn Portalbetreiber den Datenabruf bewusst steuern sollen.

## KI-Service

Servicename: `ki`

Ein KI-Service kann Apps zentralen Zugriff auf KI-Funktionen ermöglichen, ohne dass API-Schlüssel im App-Code stehen. Die öffentliche Schnittstelle ist noch nicht abschließend dokumentiert.

## Touristik-Daten

Servicename: `tourismus-odg`

Mit diesem Service können Touristikdaten aus unterschiedlichen deutschen Touristikhubs bezogen werden.
Der Service bezieht die Daten über zentrale API-Keys und erspart das individuelle anlegen der API-Keys bei den Anbietern.
Aktuell sind verfügbar:

- Tourismus-Hub Hessen
- Tourismus Open Data Baden-Württemberg

Verfügbarkeit ab 10/2026

### Nutzung

Die Nutzung erfolgt mit den gleichen Parametern die von den jeweiligen Anbietern beschrieben werden.
Zusätzlich ist die Angabe als Header Daten erforderlich:

```text
ANBIETER=BW
```

## Authentifikation über Email

Servicename: `email-auth`

In der App wird eine E-Mailadresse verifiziert durch Bestätigung über eine Email die an die Adresse gesendet wird.

Verfügbarkeit ab 12/2026

## Datenspeicher

Servicename: `storage`

Mit diesem Service können Apps vom Benutzer eingegebene Informationen im ODAS speichern. Hierzu legt der Service Daten der App in einer strukturierten Form in einer Datenbank ab:

- Emailadresse des Benutzers
- Zeitstempel
- IP-Adresse
- App
- Daten im Format JSON

Der ODP-Betreiber kann die Daten im Open Data Appstore die gespeicherten Daten einsehen und zur weiterverarbeitung exportieren.

Verfügbarkeit ab 12/2026

---

[zurück zum Index]({{ site.baseurl }}/index)
