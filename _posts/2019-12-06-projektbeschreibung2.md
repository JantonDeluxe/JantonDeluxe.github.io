---
layout: page
title: Projektbeschreibung 2
subtitle: Wie funktioniert der Höhenmesser?
---

## Ladescreen

## CSV-Export
Zum Speichern der Flugdaten ist der Export der Daten unerlässlich. Das Dateiformat, was sich für diese Daten am einfachen erstellen lässt, ist [CSV](https://de.wikipedia.org/wiki/CSV_(Dateiformat)]. Das direkte Erstellen einer CSV-Datei mit JavaScript aus den unter '/readData' abrufbaren Daten stellte sich als kompliziert heraus, weshalb wir als ersten Schritte eine unsichtbare HTML-Tabelle erstellen, aus der die Daten für die CSV-Datei abgerufen werden, wenn

Definieren der Tabellen-Kopfzeile:

```html
<table id="dataTable" class="table"  style="display: none">
        <thead>
            <tr>
                <th>Zeit</th>
                <th>Hoehe</th>
                <th>Geschwindigkeit</th>
                <th>Beschleunigung</th>
                <th>Temperatur</th>
            </tr>
        </thead>
    </table>
```

Mit jedem Datenübertragungszyklus (innerhalb von 'getData') werden die Daten in die Tabelle eingetragen:

```js
var table = document.getElementById("dataTable");
var row = table.insertRow(1);
var cell1 = row.insertCell(0);
var cell2 = row.insertCell(1);
var cell3 = row.insertCell(2);
var cell4 = row.insertCell(3);
cell1.innerHTML = time;
cell2.innerHTML = height;
cell3.innerHTML = speed;
cell4.innerHTML = acceleration;
```

## Navbar

## Buttons

## Weitere Daten im Diagramm

## Anzeige-Felder

## Reflexion
- D1 mini Pro 1.0 nicht zu emofehlen wegen der vielen Fehlermöglichkeiten
- 
