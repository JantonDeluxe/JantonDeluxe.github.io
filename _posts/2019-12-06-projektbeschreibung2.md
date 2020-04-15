---
layout: page
title: Projektbeschreibung 2
subtitle: Höhenmesser Reloaded
---
### Inhaltsverzeichnis
>* [Ladescreen](#1)
>* [Data-Logger-Shield](#2)
>* [Websiteerweiterung](#3)
>    - [Buttons](#4)
>    - [CSV-Export](#5)
>    - [Navbar](#6)
>    - [Weitere Daten](#7)
>    - [Timer](#8)
>    - [Anzeigefelder](#9)
>* [Reflexion](#10)


## Ladescreen
Der Ladescreen erscheint bei jedem Start des Arduino auf dem Bildschirm. Er besteht aus 11 einzelnen Bildern, die an Teile des Startcodes gebunden wurden. Für den Ladescreen wurde ein Bild mit Hilfe von Gimp auf die Größe von 128x64 Pixeln zurechtgeschnitten, anschließend wurden die Ladebalken eingefügt. Für den Code mussten die Bilder nun noch in ein Bitmap-Format umgewandelt werden. Dabei werden die einzelnen Pixel in Code umgewandelt. Die einzelnen Codes werden dann vom Arduino gelesen und von oben nach unten in den Ladescreen eingefügt. Für die Umwandlung vom mp4. in Bitmapformat haben wir einen Converter genutzt. 
Als Grundlagewurde die Libary Adafruit_GFX.h verwendet.

Die Größe der Bilder festzulegen, haben wir diesen Code benutzt
```
#define imageWidth 128
#define imageHeight 64
```
Das Bild loadingscreen1 (Bitmapcode nicht vollständig) wurde beispielsweise mit dieser Zeile definiert:
```
static const unsigned char PROGMEM loadingscreen1[] =
{
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, ..............., 0x00
};
```
00x0 steht für einen schwarzen Pixel.

Um diesen das nach dem Setup anzuzeigen, benutzten wird den Code
```
  drawLoadingscreen1();
```
Vor dem Starten eines Bildes des Loadingscreen wurde zuerst das Display gecleart
```
void drawLoadingscreen1()
{
  display.clearDisplay();
  display.drawBitmap(0, 0, loadingscreen1, imageWidth, imageHeight, 1);
  display.display();
}
```
![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/gifScreen.gif)

## Data-Logger-Shield
Auf Aliexpress gibt es für einen sehr sehr günstigen Preis viele Shields für den D1 mini Pro (teilweise direkt aus der Fabrik), also kleine Platinen, die man oben auf das Board draufstecken kann. Damit werden dann neue Funktionen möglich. 
Wir haben uns also so ein Shield besorgt, mit dem man Daten auf eine Micro-SD-Karte speichern und die genaue Uhrzeit anzeigen können soll. 

![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/mitShield.jpeg)

Das hat nur leider nicht geklappt. Wir haben mehrere Tage lang versucht, dieses Shield zum Laufen zu bringen: 
Der Code war bereits fertig (in mehreren Versionen, um alle möglichen Software-Fehler auszuschließen), wir haben die Pin-Belegungen des D1 mini Pro verändert, um Doppelbelegungen auszuschließen, 
SD-Karten getauscht und verschieden formatiert, die Spannung der benötigten Knopfzellen-Batterie überprüft, ein D1 mini statt dem D1 mini Pro verwendet und noch ein paar Dinge mehr, aber nichts hat die Fehler beheben können. Der SD-Karten-Slot wird gar nicht erkannt, die Real-Time-Clock (RTC) jedoch schon. Sie lässt sich aber nicht auslesen oder umstellen.

Letztendlich muss also die Hardware defekt sein. Über dieses Problem mit genau dem gleichen Board gibt es online auch einige Berichte - wir können also nicht empfehlen off-brand Elektronikteile aus China zu bestellen. Als Asuweichlösung zum Speichern der Daten haben wir deshalb später einen CSV-Export über die Website eingebaut (mehr dazu weiter unten). Immerhin haben wir im Zuge unserer vielen Versuche den Code aufgeräumt: Z.B. haben wir vieles in eigene Funktionen ausgelagert und die Variablen Englisch benannt.

## Website-Erweiterung
Die meisten Änderungen haben wir an der Website des Höhenmessers vorgenommen. 

Auf der alten Website, die nur das Diagramm mit den Höhen-Messwerten angezeigt hat, wurde es schnell sehr unübersichtlich, da schnell sehr viele Messwerte auflaufen. Deshalb haben wir eine Startseite eingebaut: Solange keine Messung angezeigt werden soll, werden auch keine Daten abgerufen und auch nicht angezeigt. Das geschieht erst, wenn man den Start-Knopf betätigt und auf die entsprechende Seite weitergeleitet wird. Ebenfalls haben wir einen Kalibrierungs-Knopf eingebaut, um den Höhenmesser auf einen neuen Nulldruck kalibrieren zu können. Sonst kommt nach spätestens einer Stunde zu erheblichen Ungenauigkeiten (siehe Projektbeschreibung 1. Halbjahr). 

Betätigt man den Start-Konpf, wird man auf die "ChartPage", also die Seite mit dem Diagramm weitergeleitet. Dort wird weiterhin das Diagramm (jetzt mit zusätzlichen Messwerten) angezeigt. Hinzu kommen Boxen, die den Temperatur- und Höchstwert anzeigen, sowie ein Timer, der die Messung nach der jeweils eingestellten Zeit wieder stoppt und auf den Nutzer wieder die Startseite bringt. Die Messung kann man aber ebenfalls manuell stoppen. Wenn man die genauen Flugdaten als CSV-Dateien benötigt, sollte man vorher jedoch über den entsprechenden Button den Export der Daten starten. 

Zusätzlich haben wir noch eine Seite hinzugefügt, auf der unsere Projektseite, die verwendete Hardware und die Dislcaimer angegeben sind. 

Die Funktionsweise der gesamten Website haben wir hier noch einmal als gif zusammengestellt:

![alt text](https://raw.githubusercontent.com/JantonDeluxe/luft-waffle/master/Bilder/gifWebsite.gif)

Nun erläutern wir die Umsetzung der neuen Features:

### Buttons
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

Mit HTML bauen wir dann die Buttons ein und definieren die URI, die durch den Klick aufgerufen werden soll (beim erstten Beispiel ´/stopp´). Darunter sind die Funktionen aufgeführt, die beim Aufrufen dieser URI ausgeführt werden.

#### Stopp-Button
```html
<form action="/stopp" class="inline">
      <button class="button button2">Stopp</button>
    </form>
``` 
---
```c++
void handleStopp() {
  timer = 200; // Reset des Timers
  startstop = false;
  Serial.println("Messung gestoppt!");
  server.sendHeader("Location", "/"); // Weiterleitung auf die Startseite
  server.send(303);
}
```

#### Start-Button
```html
<form action="/start" class="inline">
     <button class="button button1">Start</button>
    </form>
```
---
```c++
void handleStart() {
  startstop = true;
  highest = 0; // Um nicht einen noch gespeicherten Maximalwert anzuzeigen
  Serial.println("Messung gestartet!");
  server.sendHeader("Location", "/chart"); // Weiterleitung auf die ChartPage
  server.send(303);
}
```
 
#### Kalibrierungs-Button
```html
<form action="/calibrate" class="inline">
     <button class="button button2">Kalibrieren</button>
    </form>
```
---
```c++
void handleCalibration() {
  calculateBasePressure(); // Funktion zur Nulldruckberechnung
  Serial.println("Ausgangsdruck neu berechnet!");
  server.sendHeader("Location", "/");
  server.send(303);
}
```


### CSV-Export
Zum Speichern der Flugdaten ist der Export der Daten unerlässlich. Das Dateiformat, was sich für diese Daten am einfachen erstellen lässt, ist [CSV](https://de.wikipedia.org/wiki/CSV_(Dateiformat)). Das direkte Erstellen einer CSV-Datei mit JavaScript aus den unter '/readData' abrufbaren Daten stellte sich als kompliziert heraus, weshalb wir als ersten Schritt eine unsichtbare HTML-Tabelle erstellen, aus der die Daten für die CSV-Datei abgerufen werden, wenn der entsprechende Export-Button gedrückt wird. Dieses Feature funktioniert nicht in Internet Explorer, da es neuere JavaScript-Versionen nicht unterstützt. Wie bereits im Projekttagebuch erwähnt, haben wir den CSV-Export mit Hilfe dieses [Tutorials](https://www.youtube.com/watch?v=cpHCv3gbPuk) erstellt und den Code für unsere Zwecke angepasst und vereinfacht. Im folgenden werden die einzelnen Komponenten dieses Features kurz erklärt:


Export-Button einbauen:

```html
<button id="btnExportToCsv" type="button" class="button button3">CSV-Export</button>
```

Definieren der HTML-Tabellen-Kopfzeile:

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
            }, 500); // wenn der Export zu lange dauert, wird er abgebrochen
        });
        
```

Zunächst wird die HTML-Tabelle ausgelesen. Dabei wird auch darauf geachtet, ob die Tabelle eine Kopfzeile hat. Dann
Die folgenden Funktionen befinden sich alle in der Klasse `TableCSVExporter`:


```js
    constructor(table, includeHeaders = true) {
        this.table = table; 
        this.rows = Array.from(table.querySelectorAll("tr"));  // Array aus allen Tabelleninhalten
    }
```

Für jede Tabellenzeile (line) wird ein String erstellt und diese lines werden dann mit Absatz (line break) zusammengefügt 
```js
convertToCSV() {
    const lines = [];  // unetrschiedliche viele Zeilen
    const numCols = 4; // immer 4 Spalten
        
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
        
Diese Funktion liest die einzelnen Tabellen-Zellen aus und formatiert sie korrekt und gibt sie dann aus. Dies ist nötig, da z.B. Kommata, als Trennzeichen für CSV-Dateien fungieren und Zahlenwerte mit Komma folglich nicht einfach übernommen werden können. Auch Zeilenumbrüche müssen in der CSV-Synatx besonders behalndelt werden.

```js
        
static parseCell(tableCell) {
    let parsedValue = tableCell.textContent; // Inhalt aus der HTML-Tabelle auslesen (noch als Roh-Text)
    parsedValue = /[",\n]/.test(parsedValue) ? `"${parsedValue}"` : parsedValue;
        
    return parsedValue;
    }
} 
```     

Eine exportierte CSV-Datei sieht dann etwa so aus (die Zeit sind die Millisekunden nach dem Booten):

| Zeit      | Hoehe | Geschwindigkeit | Beschleunigung |
|-----------|-------|-----------------|----------------|
| 145738.00 | 0.99  | 7.41            | 137.26         |
| 145405.00 | 0.19  | 2.74            | 50.36          |
| 145178.00 | -0.08 | -2.81           | -52.56         |
| 144955.00 | -0.26 | -7.05           | -132.97        |



### Navigationsleiste
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

### Weitere Daten im Diagramm
Zusätzlich zur Höhe in Metern  berechnen wir neue Werte und zeigen Sie im Diagramm an.

Für die Berechnung der Geschwindigkeit und Beschleunigung benötigen wir jeweils die Zeit eines Messzyklus und die darin zurückgelegte Strecke. Dabei legen wir folgende Formeln zu grunde: v = Δs/Δt und a = 2Δs/Δt^2. Wir berechnen also nur die Durchschnittsgeschwindigkeit während eines Messzyklus und nehmen an, dass es sich innerhalb eines Messzyklus um eine gleichmäßig beschleunigte, geradlinige Bewegung handelt. Dazu kommt, dass die berechneten Werte aufgrund der schankenden Höhen-Werte sehr ungenau sind. Sie bieten also nicht mehr als eine grobe Einschätzung, in welchem Bereich sich die wirklichen Werte bewegen.
```c++
v = deltaS / deltaT;
a = (2 * deltaS) / (deltaT * deltaT);
```

Zum Bestimmen der Dauer eines Messzyklus starten wir mit jeder Wiederholung des loops im Hauptcode einen Timer: Es wird die Anzahl der Milisekunden, die seit dem Booten des Boards vergangen sind `millis()` als `T0` gespeichert. Dieser Wert wird dann gegen Ende des Messzyklus vom dann neuen `millis()`-Wert abgezogen. Die Differenz wird dann durch 1000 geteilt, um Δt in Sekunden zu erhalten.
```c++
// Zeitmessung starten
T0 = millis();

// Zeitmessung stoppen
deltaT = millis() - T0;
deltaT = deltaT / 1000;
```

Das Berechnen von Δs läuft sehr ähnlich: Vor dem Berechnen des nächsten Höhenwerts setzten wird den vorherigen Höhenwert (die bis dahin zurückgelegte Strecke) als Variable `S1`. Nach der Berechnung des neuen Höhenwertes setzten wir diesen als Variable `S2`. Δs ist nun wie der Name schon sagt die Differenz der beiden Werte.
```c++
// Ausgangsstrecke
S1 = h;

// Höhenunterschied
h = pressure.altitude(P, P0);

// Streckenberechnung
S2 = h;
deltaS = S2 - S1; 
 ```
 
 Nun wo die Zwischenwerte vorliegen, können Geschwindigkeit und Beschleunigung berechnet werden:
 
```c++
// Geschwindigkeit und Beschleunigung ausrechnen
v = deltaS / deltaT;
a = (2 * deltaS) / (deltaT * deltaT);
```

Diese beiden Werte und die Temperatur geben wir nun auch an die Website weiter: Dafür werden Sie nun durch `handleData`auch zum Übertragungs-String `kombi` hinzugefügt.
```c++
// Datenübertragung
void handleData() {
  double t = millis();
  String teil1 = String(String(t) + ";");
  String teil2 = String(teil1 + String(h));
  String teil3 = String(teil2 + ";");
  String teil4 = String(teil3 + String(v));
  String teil5 = String(teil4 + ";");
  String teil6 = String(teil5 + String(a));
  String teil7 = String(teil6 + ";");
  String teil8 = String(teil7 + String(highest));
  String teil9 = String(teil8 + ";");
  String teil10 = String(teil9 + String(Temp));
  String teil11 = String(teil10 + ";");
  String kombi = String(teil11 + String(timer / 100 / 6));
  server.send(200, "text/plain", kombi);
}
```

Dieser String wird dann von der Funktion `getData` wie im [Projekttagebuch vom 1. Halbjahr](https://jantondeluxe.github.io/2019-10-06-projektbbeschreibung/#9) beschrieben abgerufen und wieder in die einzelnen Werte aufgeteilt. Diese werden dann wie die Höhe im Diagramm angezeigt. Standardmäßig sind jedoch alle Werte außer der Höhe ausgeblendet. So wird das Diagramm nicht unübersichtlich.

### Timer
Um nicht endlos Daten zu speichern ist ein Stoppmechanismus notwendig. Wir haben uns nach einigen Versuchen für einen Timer entschieden. Dieser Timer richtet sich nach der Anzahl der Messzyklen und lässt sich variabel einstellen. Gestartet wird er durch drücken des Start-Knopfes (also aufrufen der URI /start): Dadurch wird unsere "Schaltervariable" (Boolean) `startstop` von false auf true gesetzt und damit der Timer (im loop) freigegeben. Wenn der Timer abgelaufen ist, wird `startstop` wieder auf false gesetzt.
```c++
 // Messung gestartet
  if (startstop == true) {
    timer = timer - 1;
    if (timer == 0) {
      startstop = false;
    }
```

Wenn der Timer abgelaufen ist, wird die URI /stopp aufgerufen, um die Übertragung zu stoppen. Der Timer wird dabei kurzzeitig auf 1 gesetzt, um den redirect wirklich nur einmal und nicht mehrfach auszuführen:
```js
// Beenden einer Messuung
if (timer == 0) {
    window.location.replace("stopp");
    timer = 1;
}
```

Durch aufrufen von /stopp wird der Timer zurückgesetzt und der Nutzer auf die Startseite weitergeleitet. Von dort aus kann eine neue Messung erfolgen. Der Höhenmesser selbst misst permanent, speichert und überträgt diese Werte jedoch nur, wenn die ChartPage geöffnet ist.
```c++
// Messung stoppen, Redirect auf StartPage
void handleStopp() {
  timer = 200;
  startstop = false;
  Serial.println("Messung gestoppt!");
  server.sendHeader("Location", "/");
  server.send(303);
}
```

### Anzeige-Felder
Die maximale Höhe die aktuelle Temperatur und der Timer lassen sich im Diagramm nicht optimal auslesen. Deshalb haben wir Anzeige-Felder erstellt, die immer den aktuellen Wert groß anzeigen. Zunächst haben wir mit CSS die drei Boxen erstellt:

```css
        .box {
            height: 65px;
            width: 175px;
            line-height: 65px;  
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
            background-color: #4CAF50;
            color: white;
            text-align: center;
            vertical-align: baseline;
        }

        .box1{
          background-color: #f39c12;
        }
        
        .box2{
          background-color: grey;
        }
        
```
Diese sollen neben- und nicht untereinander linksbündig sein:

```css
        .floated {
             float:left;
             margin-right:15px;
        }
```

Die Werte werden ja bereits übertragen über *innerHTML* wird für diese Werte eine ID erstellt. Über diese können dann die Werte abgerufen und angezeigt werden. 
```js
document.getElementById("highest").innerHTML = response[4];
document.getElementById("temp").innerHTML = response[5];
document.getElementById("timer").innerHTML = response[6];
```

Mit HTML werden die Boxen dann eingebaut: 

```html
     <div style="font-family:verdana;text-align:left;padding-top:10px">
      <div class="box box1 floated">
      Max.     <b style="font-size:150%;"><span id="highest">0</span> m</b>
      </div>

      <div class="box box2 floated">
      Temp.     <b style="font-size:150%;"><span id="temp">0</span> C</b>
      </div>
  
      <div class="box floated">
      Timer     <b style="font-size:150%;"><span id="timer">0</span></b>
      </div>
    </div>
```
	 

## Reflexion des Projekts
Insgesamt sind wir sehr zufrieden mit dem Ergebnis des Informatikprojekts. Die Temperatur kann zuverlässig angezeigt werden und die Höhe mit einigen Abweichungen auch. Legendlich Geschindigkeit und Beschleunigung sind ungenau, was aber vor allem an der Technik liegt. Vor allem die Website funktioniert dank der vielen Fehlerbehebungen sehr gut.
Allerdings ist der Mikrocontroller D1 mini Pro 1.0 nicht zu empfehlen, wegen der vielen Fehlermöglichkeiten. Dazu ist die Qualität der etwas älteren chinesischen Elektronik nicht die Beste, gerade der Arduino hatte oft Abweichungen bei Messen des Luftdrucks. Leider ist auch noch nicht dazu gekommen, dass wir den Arduino in einer Wasserrakete testen konnten, denn wir hatten Angst, dass das Gerät kaputt geht. Hinzu wäre es schwierig, den Arduino mit Storm zu versorgen.
- D1 mini Pro 1.0 nicht zu empfehlen wegen der vielen Fehlermöglichkeiten
- auf Qualität bei chinesischer Elektronik achten
- leider keinen Praxistest
