---
layout: page
title: Projektbeschreibung 2
subtitle: Höhenmesser Reloaded
---

## Ladescreen

## Buttons
Um die neuen Funktionen der Website aufrufen zu können, benötigen wir Buttons. Um diese zu verschönern (Farben, abgerundete Ecken, Schatten usw.) definieren wir Klassen für die Buttons mit CSS (hier die Buttons auf der ChartPage):

```css
        .button {
            background-color: #4CAF50;
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
            border: none;
            color: white;
            padding: 16px 32px;
            width: 150px;
            text-align: center;
            text-decoration: none;
            font-size: 16px;
            margin: 4px 2px;
            -webkit-transition-duration: 0.4s;
            transition-duration: 0.4s;
            cursor: pointer;
        }

         .button2 {
            border-radius: 8px;
            background-color: #f44336;
            color: white;
            border: 2px solid #f44336;
        }
        
        .button2:hover {
            border-radius: 8px;
            background-color: white;
            color: black;
            border: 2px solid #f44336;
        }

        .button3 {
            border-radius: 8px;
            display: inline-block;
            margin-right: 5px;
            background-color: #0080ff;
            color: white;
            border: 2px solid #0080ff;
        }       

       .button3:hover {
            border-radius: 8px;
            display: inline-block;
            margin-right: 5px;
            background-color: white;
            color: black;
            border: 2px solid #0080ff;
        }
        
```

Mit HTML bauen wir dann die Buttons ein. In diesem Fall wird beim klicken des Buttons die `/Stopp`-URI aufgerufen:
```html
<form action="/stopp" class="inline">
      <button class="button button2">Stopp</button>
    </form>
```

void handleStopp() {
  timer = 200;
  startstop = false;
  Serial.println("Messung gestoppt!");
  server.sendHeader("Location", "/");
  server.send(303);
}

```html
<form action="/start" class="inline">
     <button class="button button1">Start</button>
    </form>
```

void handleStart() {
  startstop = true;
  highest = 0;
  Serial.println("Messung gestartet!");
  server.sendHeader("Location", "/chart");
  server.send(303);
}
 
```html
<form action="/calibrate" class="inline">
     <button class="button button2">Kalibrieren</button>
    </form>
```

void handleCalibration() {
  calculateBasePressure();
  Serial.println("Ausgangsdruck neu berechnet!");
  server.sendHeader("Location", "/");
  server.send(303);
}


## CSV-Export
Zum Speichern der Flugdaten ist der Export der Daten unerlässlich. Das Dateiformat, was sich für diese Daten am einfachen erstellen lässt, ist [CSV](https://de.wikipedia.org/wiki/CSV_(Dateiformat)). Das direkte Erstellen einer CSV-Datei mit JavaScript aus den unter '/readData' abrufbaren Daten stellte sich als kompliziert heraus, weshalb wir als ersten Schritt eine unsichtbare HTML-Tabelle erstellen, aus der die Daten für die CSV-Datei abgerufen werden, wenn der entsprechende Export-Button gedrückt wird.

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

Export-Button:

```html
<button id="btnExportToCsv" type="button" class="button button3">CSV-Export</button>
```


Mit jedem Datenübertragungszyklus (innerhalb von `getData`) werden die Daten via *innerHTML* in die Tabelle eingetragen:

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

Listener für den Fall, dass der Button geklickt wird: CSV-Erstellung starten und Export/Download starten:
```js
        btnExportToCsv.addEventListener("click", () => {
            const exporter = new TableCSVExporter(dataTable);
            const csvOutput = exporter.convertToCSV();
            const csvBlob = new Blob([csvOutput], {
                type: "text/csv"
            });
            const blobUrl = URL.createObjectURL(csvBlob);
            const anchorElement = document.createElement("a");
        
            anchorElement.href = blobUrl;
            anchorElement.download = "table-export.csv";
            anchorElement.click();
        
            setTimeout(() => {
                URL.revokeObjectURL(blobUrl);
            }, 500);
        });
        
```

HTML-Tabelle auslesen und CSV erstellen:
```js
        class TableCSVExporter {
            constructor(table, includeHeaders = true) {
                this.table = table;
                this.rows = Array.from(table.querySelectorAll("tr"));
        
                if (!includeHeaders && this.rows[0].querySelectorAll("th").length) {
                    this.rows.shift();
                }
            }
        
            convertToCSV() {
                const lines = [];
                const numCols = this._findLongestRowLength();
        
                for (const row of this.rows) {
                    let line = "";
        
                    for (let i = 0; i < numCols; i++) {
                        if (row.children[i] !== undefined) {
                            line += TableCSVExporter.parseCell(row.children[i]);
                        }
        
                        line += (i !== (numCols - 1)) ? "," : "";
                    }
        
                    lines.push(line);
                }
        
                return lines.join("\n");
            }
        
            _findLongestRowLength() {
                return this.rows.reduce((l, row) => row.childElementCount > l ? row.childElementCount : l, 0);
            }
        
            static parseCell(tableCell) {
                let parsedValue = tableCell.textContent;
        
                // Replace all double quotes with two double quotes
                parsedValue = parsedValue.replace(/"/g, `""`);
        
                // If value contains comma, new-line or double-quote, enclose in double quotes
                parsedValue = /[",\n]/.test(parsedValue) ? `"${parsedValue}"` : parsedValue;
        
                return parsedValue;
            }
        }

## Navbar

## Weitere Daten im Diagramm

## Anzeige-Felder

## Reflexion
- D1 mini Pro 1.0 nicht zu emofehlen wegen der vielen Fehlermöglichkeiten
- 
