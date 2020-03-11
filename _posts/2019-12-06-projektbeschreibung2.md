---
layout: page
title: Projektbeschreibung 2
subtitle: Höhenmesser Reloaded
---

## Ladescreen

## CSV-Export
Zum Speichern der Flugdaten ist der Export der Daten unerlässlich. Das Dateiformat, was sich für diese Daten am einfachen erstellen lässt, ist (CSV)[https://de.wikipedia.org/wiki/CSV_(Dateiformat)]. Das direkte Erstellen einer CSV-Datei mit JavaScript aus den unter '/readData' abrufbaren Daten stellte sich als kompliziert heraus, weshalb wir als ersten Schritt eine unsichtbare HTML-Tabelle erstellen, aus der die Daten für die CSV-Datei abgerufen werden, wenn der entsprechende Export-Button gedrückt wird.

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


        // CSV-Erstellung
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

## Buttons

## Weitere Daten im Diagramm

## Anzeige-Felder

## Reflexion
- D1 mini Pro 1.0 nicht zu emofehlen wegen der vielen Fehlermöglichkeiten
- 
