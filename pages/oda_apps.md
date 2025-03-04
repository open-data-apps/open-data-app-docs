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
