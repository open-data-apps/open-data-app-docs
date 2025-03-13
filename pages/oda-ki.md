---
layout: default
title: Automatisierte Erstellung einer ODA mit KI
permalink: /oda-ki/
---

Die ODA sind strukturell so aufgebaut, dass eine
Automatisierung der Inhaltserstellung möglich ist.

Das Ziel dabei ist, dass ODAs einfach erstellt werden können,
um dann in möglichst viele Open Data Portalen zum Einsatz
zu kommen. Motto: Je niedriger die Entwicklungskosten sind,
desto mehr ODAs entstehen und je größer ist die Auswahl
an ODAs wird, desto mehr bekommen Rohdaten neues Leben.

## Inhalt

- Was muss die KI tun?
- Welchen Prompt benöige ich?
- Beispiel: Ein automatsierte erstellte ODA

## Was muss die KI tun?

Die KI soll:

- Den bereitgestellten Code-Template analysieren und automatisiert den erforderlichen JavaScript-Code generieren.
- Die Funktion `app()` so implementieren, dass dynamischer Content basierend auf den Konfigurationsdaten (wie dem API-Link in `configdata`) erstellt wird.
- Sicherstellen, dass der generierte Content ausschließlich in das übergebene HTML-Element (`enclosingHtmlDivElement`) geladen wird.
- Falls benötigt, über die Funktion `addToHead()` externe Skripte und Stylesheets in den `<head>`-Bereich einfügen.
- Bei spezifischen Anforderungen (wie im Beispiel eines Ping Pong Spiels) den Code so anpassen, dass die jeweilige Funktionalität und Interaktivität umgesetzt wird.

## Welchen Prompt benöige ich?

Prompt:

Agiere als Softwareentwickler für eine Web-App.

Die technischen Rahmenbedingungen der App sind folgende:

- Die Web-App besteht aus einem Header, einem Footer und einem Inhaltsbereich.
  Header und Footer stehen bereits fest. Es muss nur der Inhaltsbereich erstellt werden.
- In dem Kommentar des Code-Template stehen die Übergabeparameter.
- Mit der app() Funktion soll der Content für die Seite generiert werden. Die übergebene
  Variable `configdata` enthählt die `apiurl`. Diese enthält einen Link
  zu einem Datensatz oder einer Datei aus einem Open Data Portal. Falls benötigt
  soll die App von dort die Daten beziehen.
- Die app() Funktion muss in Javascript geschrieben werden.
- Der generierte Content soll ausschließlich in das enclosingHtmlDivElement geladen werden.
- Mit der Funktion addToHead können Skripte oder Stylesheets per Javascript in den Head
  der index.html geladen werden.

Die App soll folgendes tun:

- ...

Aufgabe:
Erstelle die app. fülle dazu die funktion app() {} und ggf. addToHead() {}

hier ist der code-template:

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
-         "apiUrl": "https://open-data-musterstadt.ckan.de/dataset/db92da8e40f9/download/formular_multitemplate.json"
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
- Sie hängt die Skripte und Stylesheets in die Head Section an.

- @returns {string} - HTML mit script, link, etc. Tags
  \*/
  function addToHead() {

  }

## Beispiel: Ein automatsierte erstellte ODA

Folgend ist ein Beispiel für die Erstellung eines Ping Pong Spiels.

Prompt:

Agiere als Softwareentwickler für eine Web-App.

Die technischen Rahmenbedingungen der App sind folgende:

- Die Web-App besteht aus einem Header, einem Footer und einem Inhaltsbereich.
  Header und Footer stehen bereits fest. Es muss nur der Inhaltsbereich erstellt werden.
- In dem Kommentar des Code-Template stehen die Übergabeparameter.
- Mit der app() Funktion soll der Content für die Seite generiert werden. Die übergebene
  Variable `configdata` enthählt die `apiurl`. Diese enthält einen Link
  zu einem Datensatz oder einer Datei aus einem Open Data Portal. Falls benötigt
  soll die App von dort die Daten beziehen.
- Die app() Funktion muss in Javascript geschrieben werden.
- Der generierte Content soll ausschließlich in das enclosingHtmlDivElement geladen werden.
- Mit der Funktion addToHead können Skripte oder Stylesheets per Javascript in den Head
  der index.html geladen werden.

Die App soll folgendes tun:

- Anzeige eines Ping Pong Spiels
- Der Spieler (rechts) soll mit den Pfeiltasten auf und ab fahren
- Der Spieler (links) ist der Computer
- Das Spiel soll über die Ganze breite gehen

Aufgabe:
Erstelle die app. fülle dazu die funktion app() {} und ggf. addToHead() {}

hier ist der code-template:

/\*

- Diese Funktion ist für die Inhalte der Startseite zuständig.
-
- Der umschließebde HTML code ist:
-
-      <body>
-      <div class="container mt-4" id="main-content">
-          ...
-      </div>
-      </body>
-
- Als CSS Framnework wird Bootstrap 5.3 verwendet.
-
- ConfigData ist ein JSON enthält die Referenz
- auf die Daten im CKAN Open Data Portal:
-     {
-         "apiUrl": "https://open-data-musterstadt.ckan.de/dataset/db92da8e40f9/download/formular_multitemplate.json"
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
- Sie hängt die Skripte und Stylesheets in die Head Section an.

- @returns {string} - HTML mit script, link, etc. Tags
  \*/
  function addToHead() {

  }

---

[zurück zum Index]({{ site.baseurl }}/index)
