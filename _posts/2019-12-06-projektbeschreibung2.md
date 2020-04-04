---
layout: page
title: Projektbeschreibung 2
subtitle: Höhenmesser Reloaded
---

## Ladescreen

![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/gifScreen.gif)

## Neue Seiten auf der Website
![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/gifWebsite.gif)

### Start-Seite

### About-Seite


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
Hier einmal ein Beispiel, wie Buttons aussehen, wenn man sie drückt, oder nicht:
![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/Home-Button%20aus.png)
![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/Home-Button%20an.PNG)

Mit HTML bauen wir dann die Buttons ein und definieren die URI, die durch den Klick aufgerufen werden soll (beim erstten Beispiel ´/stopp´). Darunter sind die Funktionen aufgeführt, die beim Aufrufen dieser URI ausgeführt werden.

### Stopp-Button
```html
<form action="/stopp" class="inline">
      <button class="button button2">Stopp</button>
    </form>
```

```c++
void handleStopp() {
  timer = 200;
  startstop = false;
  Serial.println("Messung gestoppt!");
  server.sendHeader("Location", "/");
  server.send(303);
}
```

### Start-Button
```html
<form action="/start" class="inline">
     <button class="button button1">Start</button>
    </form>
```

```c++
void handleStart() {
  startstop = true;
  highest = 0;
  Serial.println("Messung gestartet!");
  server.sendHeader("Location", "/chart");
  server.send(303);
}
```
 
### Kalibrierungs-Button
```html
<form action="/calibrate" class="inline">
     <button class="button button2">Kalibrieren</button>
    </form>
```

```c++
void handleCalibration() {
  calculateBasePressure();
  Serial.println("Ausgangsdruck neu berechnet!");
  server.sendHeader("Location", "/");
  server.send(303);
}
```


## CSV-Export
Zum Speichern der Flugdaten ist der Export der Daten unerlässlich. Das Dateiformat, was sich für diese Daten am einfachen erstellen lässt, ist [CSV](https://de.wikipedia.org/wiki/CSV_(Dateiformat)). Das direkte Erstellen einer CSV-Datei mit JavaScript aus den unter '/readData' abrufbaren Daten stellte sich als kompliziert heraus, weshalb wir als ersten Schritt eine unsichtbare HTML-Tabelle erstellen, aus der die Daten für die CSV-Datei abgerufen werden, wenn der entsprechende Export-Button gedrückt wird. Dieses Feature funktioniert nicht in Internet Explorer, da es neuere JavaScript-Versionen nicht unterstützt. Wie bereits im Projekttagebuch erwähnt, haben wir den CSV-Export mit hilfe dieses [Tutorials](https://www.youtube.com/watch?v=cpHCv3gbPuk) erstellt. Im folgenden werden die einzelnen Komponenten dieses Features kurz erklärt:


Export-Button einbauen:

```html
<button id="btnExportToCsv" type="button" class="button button3">CSV-Export</button>
```

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

Mit jedem Datenübertragungszyklus (innerhalb von `getData`) wird eine neue "Tabellen-Zelle" erstellt, in die die Daten via *innerHTML* jeweils eingetragen werden:

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

Dies ist ein Listener, der "darauf wartet", dass der Button geklickt wird. Dann werden die Funktionen zur CSV-Erstellung und zum Export/Download ausgefhrt:
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

Zunächst wird die HTML-Tabelle ausgelesen. Dabei wird auch darauf geachtet, ob die Tabelle eine Kopfzeile hat. Dann
```js
class TableCSVExporter {
    constructor(table, includeHeaders = true) {
        this.table = table;
        this.rows = Array.from(table.querySelectorAll("tr"));
        
            if (!includeHeaders && this.rows[0].querySelectorAll("th").length) {
                this.rows.shift();
            }
    }
```

```js
convertToCSV() {
    const lines = [];
    const numCols = this._findLongestRowLength();
        
        for (const row of this.rows) {
            let line = "";
        
            for (let i = 0; i < numCols; i++) {
                if (row.children[i] !== undefined) {
                    ine += TableCSVExporter.parseCell(row.children[i]);
                }
        
                line += (i !== (numCols - 1)) ? "," : "";
            }
        
            lines.push(line);
        }
        
    return lines.join("\n");
	}
	```
        
Diese Funktion liest die einzelnen Tabellen-Zellen aus und formatiert sie korrekt und gibt sie dann aus. Dies ist nötig, da z.B. Kommata, als Trennzeichen für CSV-Dateien fungieren und Zahlenwerte mit Komma folglich nicht einfach übernommen werden können. Auch Anführungszeichen oder Zeilenumbrüche müssen in der CSV-Synatx besonders behalndelt werden. Hier schließt sich auch die Klasse `TableCSVExporter` - deshalb die zweite geschlossene Klammer.
```js
        
static parseCell(tableCell) {
    let parsedValue = tableCell.textContent;   
    parsedValue = parsedValue.replace(/"/g, `""`)
    parsedValue = /[",\n]/.test(parsedValue) ? `"${parsedValue}"` : parsedValue;
        
    return parsedValue;
    }
} 
```           

## Navigationsleiste
Um nicht immer den jeweiligen Link in die Adresszeile des Browsers eintippen zu müssen, um auf der Website zu navigieren, haben wir mit CSS und HTML eine Navigationsleiste (Navbar) eingebaut. Die Basis dafür bildet [dieses Tutorial](https://www.w3schools.com/css/css_navbar.asp) von W3Schools. Dies ist der Code der Navbar auf der Startseite.

Mit CSS wird das "Aussehen" der Navbar definiert. Dafür werden die Abmessungen der einzelnen Boxen, die Schatten und andere Parameter angegeben. Für die Farben muss anders, als beim Chart.js-Diagramm ein HEX-Code angegeben werden (statt rgba).

```css
 ul {
            list-style-type: none;
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #14264f;
            position: fixed;
            top: 0;
            width: 100%;
        }
        
        li {
            float: left;
        }
        
        li a {
            display: block;
            color: white;
            text-align: center;
            padding: 14px 16px;
            text-decoration: none;
            font-family: verdana;
        }
        
        li a:hover:not(.active) {
            background-color: #183754;
        }
        
        .active {
            background-color: #f39c12;
        }
		
```

Über HTML wird die Navbar in die Seite eingebaut, sowie festgelegt, welcher Reiter "active", also farblich hervorgehoben ist. Ebenfalls werden die jeweils aufzurufenden URIs der einzelnen Reiter festgelegt.
```html
 <ul>
        <li><a class="active" href="/">Home</a></li>
        <li><a href="/chart">Vorschau</a></li>
        <li style="float:right"><a href="/about">About</a></li>
    </ul>
```

## Weitere Daten im Diagramm


## Anzeige-Felder


## Timer
Redirect, wenn Timer abgelaufen

## Reflexion
- D1 mini Pro 1.0 nicht zu emofehlen wegen der vielen Fehlermöglichkeiten
- 
