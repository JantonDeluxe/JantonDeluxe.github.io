---
layout: page
title: Projektbeschreibung
subtitle: Wie funktioniert der Höhenmesser?
---

### Inhaltsverzeichnis
>* [Ziel](#1)
>* [Funktionen](#2)
>* [Hardware](#3)
>    - [Mikrocontroller](#4)
>    - [Höhenmesser](#5)
>    - [Display](#6)
>* [Software](#7)
>    - [Serielle Schnittstelle](#8)
>    - [Höhenmessung](#9)
>    - [Anzeige auf Display](#10)
>    - [WLAN-Verbindung oder Access Point](#11)
>    - [Webserver](#12)
>* [Quellen](#13)

## Ziel<a name="1"></a>
Wasserraketen können ziemlich hoch fliegen. Aber wie hoch genau?

Das haben wir uns schon vor vier Jahren gefragt und uns nach einem Höhenmesser umgegeuckt. Off-the-shelf kostet so ein geigneter Höhenmesser mindesten 50 Euro und das mit einem sehr beschränkten Funktionsumfang. Deshalb haben wir angefangen, einen Höhenmesser auf Basis eines Arduino nano selber zu bauen. 

![Alter Höhenmesser](https://i1.wp.com/jan.krummrey.de/wp-content/uploads/2015/09/11951281_10153345040958153_9145140276330392700_n.jpg)

Der [alte Höhenmesser](http://jan.krummrey.de/2015/09/13/hoehenmesser-fur-unsere-wasserrakete/) konnte über das zum Sensor gehörende [Example-Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/BMP180_altitude_example/BMP180_altitude_example.ino) die aktuelle Höhe messen und auf zwei 7-Segment-Anzeigen darstellen. Zusätzlich konnte er die maximale Höhe berechnen und durch das Blinken der im Arduino eingebauten LED ausgeben. Das funktioniert, ist aber nicht sehr "nutzerfreundlich". 

Dazu kamen noch einige konstruktionsbedingte Schwachstellen, weshalb wir ihn nie zuende gebaut haben: 
* durch die zwei 7-Segment-Anzeigen konnte immer nur eine Information mit maximal zwei Ziffern angezeigt werden 
* der Arduino nano hat kein WLAN, weshalb man sich immer per USB-Kabel verbinden muss, was unmöglich ist, wenn der Sensor in einer Rakete eingebaut ist
* es konnten keine gespeicherten Daten ausgelesen werden, da der Arduino nano sich bei USB Verbindungen resettet
* es konnten keine längeren Messreihen wie Flugverläufe gespeichert werden, da der Speicher dafür nicht ausgereicht hat

Der Code ist in den vier Jahren verloren gegangen, basierte aber bis auf das Berechnen des Maximalwerts und dem Blinken der LED auf dem Example Sketch.

## Funktionen <a name="2"></a>
Der neue Höhenmesser behebt die alten Probleme und kann noch mehr:
1. aktuelle Höhe auf Display anzeigen
2. maximale Höhe möglichst auf unter 1 Meter genau auf Display anzeigen
3. Temperatur und Druckwerte auf Display anzeigen
4. Daten drahtlos auf Webserver anzeigen
5. Flugverlauf grafisch darstellen

## Hardware <a name="3"></a>
Viele der Probleme mit dem alten Höhenmesser lassen sich auf die unzureichende Hardware zurück führen. Deshalb haben wir einige Änderungen vorgenommen.
### Mikrocontroller <a name="4"></a>
Als Mikrocontroller-Board benutzen wir einen [Wemos D1 mini Pro V1.0.0](https://wiki.wemos.cc/products:retired:d1_mini_pro_v1.0.0) von Aliexpress.

![Board-Zeichnung](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/board.JPG?raw=true)

Der D1 mini Pro verwendet einen [ESP8266EX](https://www.espressif.com/en/products/hardware/esp8266ex/overview)-Mikrochip von espressif Systems. Das ist ein 32 bit-Mikroprozessor (Tensilica L106) mit integrierter WLAN-Schnittstelle (802.11 b/g/n Unterstützung). Eingebaut ist eine Keramik-Antenne, man könnte aber auch eine externe Antenne anschließen. Damit lassen sich Daten nun auch kabellos per WLAN übertragen.

Dazu besitzt das Board 16 Megabyte Flash-Speicher statt den 32 Kilobyte beim Arduino nano. Das Speichern von längeren Datenreihen ist jetzt also möglich.
Das Board ist Arduino-kompatibel, das heißt man kann es wie jeden beliebigen Arduino mit der Arduino IDE programmieren. Dafür müssen jedoch einige Einstellungen verändert werden. Wie das funktioniert beschreiben wir im Unterrichtstagebuch vom 21. bis 27. August.
Zum Verbinden mit einem PC verwendetet man den eingebauten microUSB-Port. Das USB-Signal wird dann von der USB-to-UART-Bridge in ein serielles Signal umgewandelt, das der Prozessor verarbeiten kann.

### Höhenmesser <a name="5"></a>
Als Höhenmesser verwenden wir dem Bosch BMP180, einen günstigen Drucksensor mit relativ hoher Genauigkeit (theoretisch 0,25 m). Eingesetzt wird dieser Sensor auch in Smartphones oder einfachen Wetterstationen.

Er basiert auf dem piezoresistiven Effekt: Bei Druck oder Zug (in diesem Fall durch den Luftdruck) verändert sich der elektrische Widerstand eines Materials (hier: Silizium). Durch diese Änderung lässt sich dann der ausgeübte Druck bestimmen.
Der Höhenmesser besitzt ebenfalls ein Thermometer.
Die so gemessenen Werte werden von einem digital-to-analog converter in ein digitales Signal umgewandelt. Dieses wird dann von einer Kontrolleinheit mit Hilfe der auf dem Board gespeicherten Kalibrierungswerte ausgeglichen, sodass die Werte über das I²C-Interface ausgegeben werden können.

I²C steht für "Inter-Integrated Circuit bus" und ist, wie der Name schon sagt, dafür da, integrierte Schaltkreise (wie den Höhenmesser und den Arduino) zu verbinden. Der größte Vorteil dieses Busses ist, dass mehrere Schaltkreise über die gleiche Leitung verbunden werden können, man also nur eine Doppelleitung benötigt. Die erste dieser Leitungen überträgt den Takt, die zweite die Daten.[[²]][I²C]

#### Verkabelung

|BMP180|D1 mini Pro|Funktion |
|------|:---------:|--------:|
|GND   |GND        |Masse    |
|VIN   |3V3        |3,3 Volt |
|SDA   |D2         |I²C-Data |   
|SCL   |D1         |I²C-Clock|

#### Fehlerquellen:
* Wind kann momentane Druckunterschiede erzeugen, die nicht dem eigentlichen Umgebungsdruck entsprechen
* Wetterlagen kommen mit unterschiedlichen Umgebungsdrücken einher (Hoch-/ Tiefdruckgebiet)
* Feuchtigkeit kann die Messungen verfälschen.
* Das Silizium im Drucksensor ist lichtempfindlich, sollte also vor allzu starker Sonneneinstrahlung geschützt werden. [[1]][Sparkfun]

### OLED-Display <a name="6"></a>
Unser Display ist ein monochromes GM009605 OLED-Display:

*0,96′ Bildschirmdiagonale
*Auflösung von 128×64 Pixel
*Monochrom
*SSD1306-Controller
*3-5V Gleichstrom
*Modulgröße: 27 mm x 27 mm x 4,5 mm
*Anschlüsse: GND, VDD, SCK, SDA
*Gemacht für Arduino—Mikrocontroller

Das Display verfügt über einen SSD1306—Controller und muss an eine 3—5V Gleichstromquelle angeschlossen werden (in diesem Fall 3,3Volt).  Daten und Strom werden mithilfe von 4 Anschlüssen vermittelt. 

#### Verkabelung
Das Display benötigt die gleichen Verbindungen wie der Höhenmesser. Deshalb haben wir die beiden Schaltkreise in Reihe geschaltet. Das funktioniert, da das I²C-Protokoll mehrere Geräte über eine Doppelleitung ansteuern kann.

|Display|D1 mini Pro|Funktion |
|-------|:---------:|--------:|
|GND    |GND        |Masse    |
|VDD    |3V3        |3,3 Volt |
|SDA    |D2         |I²C-Data |   
|SCK    |D1         |I²C-Clock|

### Diagramm
![Diagramm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Z-Uno%20bmp180-oled_Steckplatine.png?raw=true)#

Anstelle des D1 mini Pro haben wir hier einen D1 mini genommen, der die gleichen Anschlüsse hat.

   
## Software <a name="7"></a>
Hier beschreiben wir die einzelnen Komponenten des Programms getrennt, damit die Erklärung nicht zu kompliziert wird.

***
### Serielle Schnittstelle <a name="8"></a>
Für die Übertragung von Daten an den PC nutzen wir die serielle Schnittstelle des Arduino. Diese Überträgt per Micro-USB-Kabel Daten an den PC, die mit dem seriellen Monitor in der Arduino IDE ausgelesen werden können.

`Serial.begin()` startet die Übertragung, in diesem Falle mit einer Übertragungsgeschwindigkeit von 115200 Bits pro Sekunde.

`Serial.println("")` schreibt Daten als ASCII-Text und beginnt danach im Gegensatz zu `Serial.Print()` eine neue Zeile. Hier verhindert eine leere Ausgabe Darstellungsfehler zu Beginn der Übertragung.

```c++
 Serial.begin(115200);
 delay(100);
 Serial.println("");
 Serial.println("Datenübertragung gestartet!");
```
Die serielle Schnittstelle des Arduino benutzen wir hauptsächlich, um Fehlermeldungen und Zwischenwerte anzuzeigen. Alle anderen Daten werden entweder über das Display oder die Website angezeigt.

Man kann Daten auch als Graph über den seriellen Plotter ausgeben lassen. Diese Funktion ist neu, vor vier Jahren brauchten wir für die graphische Darstellung noch ein extra [Processing](https://processing.org/)-Skript. Das ist jedoch unpraktisch, da man durchgehend eine USB-Verbindung benötigt.

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Serieller%20Plotter.png?raw=true)

_Blau: Höhe in Meter_
_Rot: Höhe in feet_

***

### Höhenmessung <a name="9"></a>
*Bei der Einbindung des Höhenmessers haben wir uns am [Example-Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/SFE_BMP180_example/SFE_BMP180_example.ino) von Mike Grusin aus der SFE_BMP180-Library orientiert.*

#### Bibliotheken einbinden
Für die Benutzung des BMP180 benötigt man zwei Libraries oder auf deutsch Programmbiliotheken:
* **SFE_BMP180** ist die Library, die den Maschinencode für den Höhenmesser enthält. Damit ermöglicht sie das auslesen und ansteuern des Höhenmessers. 

* **Wire** sorgt dafür, dass der Arduino mit dem Höhenmesser kommunizieren kann, da dieser zur Kommunikation das I²C-Protokoll benutzt.[[¹]][BMP180-Datenblatt] 

Libraries enthalten Code-Teile, wie z.B. vorgefertigte Funktionen oder Maschinencode, auf die dann im eigenen Programm einfach zugegriffen werden kann, ohne sie dort selbst schreiben oder einfügen zu müssen.


```c++
#include <SFE_BMP180.h>
#include <Wire.h>
```


#### Objekt erstellen
Um die Funktionen der SFE_BMP180-Library abrufen zukönnen muss als nächstes ein Objekt für diese Bibliothek erstellt werden. Der Name dieses Objektes ist egal, deshalb haben wir den Namen "pressure" übernommen.
Objekte sind......

```c++
SFE_BMP180 pressure;
```


#### Sensor starten
Der nächste Schritt ist es, den Sensor zu starten. Dafür wird die Funktion `begin()` aus der Höhenmesser-Library aufgerufen. Da diese Funktion einem Objekt, in diesem Falle `pressure`, zugeordnet ist, handelt es sich genau genommen um eine Methode.

`pressure.begin()` startet nun, falls noch nicht geschehen, die Wire-Library und kalibriert den Sensor. Wenn dabei ein Fehler auftreten sollte wird immer wieder eine Schleife wiederholt und das Programm damit gestoppt.

```c++
 if (pressure.begin())
    Serial.println("BMP180 gestartet!");
  else
  {
    Serial.println("BMP180 fehlt!");
    while (1);
  }
```


#### Druck messen
Zum Messen des Drucks verwenden wir die Funktion `getPressure()`. Nach dem Deklarieren der Variablen muss dazu zuerst eine Temperatur-Messung durchgeführt werden. 

Mit `pressure.startTemperature()` wird die Messung begonnen. Bei einem Fehler gibt die Funktion den Wert 0, sonst die Zeit, die für die Messung gebraucht wird in Millisekunden aus. Solange wird gewartet. `pressure.getTemperature(T)` verändert dann den Wert der Variable T, sodass er dem Temperaturwert entspricht. Ist das erfolgreich gibt die Funktion den Wert 1, sonst den Wert 0 aus.

Jetzt kann der Druck gemessen werden: `pressure.startPressure` beginnt die Messung (in diesem Fall auf Genauigkeitsstufe 3 von 3) und gibt entweder 0 als Fehler oder die Wartezeit in Millisekunden aus. `pressure.getPressure` berechnet dann auf Basis von `T` den Druck `P` in Milibar. Die Temperatur muss eingerechnet werden, da sich nach dem allgemeinen Gasgesetz der Druck bei ändernder Temperatur und gleichbleibendem Volumen ebenfalls verändert.  Ist das erfolgreich gibt die Funktion den Wert 1, sonst den Wert 0 aus. Wird 0 ausgegeben, sendet die Funktion die entsprechende Fehlermeldung seriell an einen Angeschlossenen Computer.

```c++
double getPressure()
{
  // Variablen
  char status;
  double T,P,p0,a;
  
  // Temperatur messen
  status = pressure.startTemperature();
  if (status != 0)
  {
    // Warten bis Messung fertig
    delay(status);

     // Temperatur erhalten
    status = pressure.getTemperature(T);
    if (status != 0)
    {
      
      // Druck messen
      status = pressure.startPressure(3);
      if (status != 0)
      {
        // Warten bis Messung fertig
        delay(status);

        //Druck erhalten
        status = pressure.getPressure(P,T);
        if (status != 0)
        {
          return(P);
        }
        else Serial.println("Fehler beim Erhalten des Drucks");
      }
      else Serial.println("Fehler beim Starten der Druckmessung");
    }
    else Serial.println("Fehler beim Erhalten der Temperatur");
  }
  else Serial.println("Fehler beim Starten der Temperaturmessung");
}
```
  
  
  
#### Ausgangsdruck berechnen 
Der Höhenmesser berechnet seine Höhenangaben aus der Differenz des aktuell gemessenen Drucks und dem Ausgangsdruck. Für diesen  Ausgangsdruck deklarieren wir eine eigene Variable `ausgangsdruck`, das Array `Array` und die Konstante `messungen`.
Ein Array ist.....KJHGKJHGKJ

```c++
const int messungen = 100;
float Array[messungen];
float ausgangsdruck = 0.0;
```
Im Array speichern wir dann solange Druckwerte, bis die vorgegebene Anzahl der Messungen erreicht ist. 
```c++
 for (int i = 0; i < messungen; i++)
  {
    Array[i] = getPressure();
  }
```

Solange wird ebenfalls zur Variable `ausgangsdruck` die jeweils neueste Messung addiert.

```c++
  for (int i = 0; i < messungen; i++)
  {
    ausgangsdruck = ausgangsdruck + Array[i];
  }
```

Wenn die Anzahl der Messungen erreicht ist, wird die Summe aller Messungen durch die Anzahl der Messungen geteilt und man erhält den Durchschnitt der Messungen. Die Berechnung dieses Durchschnitts ist wichtig, da einzelne Druckmessungen immer ein wenig schwanken und damit die Höhenberechnung ungenau machen würden.

```c++
ausgangsdruck = ausgangsdruck / messungen;
```

  
#### Höhe berechnen
Die Methode `pressure.altitude()` berechnet mit Hilfe der Barometrischen Höhenformel den zurückgelegten Höhenunterschied basierend auf dem aktuellen Druck und dem Ausgangsdruck. Dieses Ergebnis setzten wir dann der Höhenvariable `a` gleich

![Barometrische Höhenformel](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Druck.jpg?raw=true)

```c++
a = pressure.altitude(P, ausgangsdruck);
```

In der Theorie sollte der BMP180 mit dieser Methode eine Genauigkeit von 25 cm erreichen. In der Praxis liegt die Ungenauigkeit bei eitwas über 1 Meter. Die Genauigkeit verschlechtert sich mit der Zeit unter anderem auf grund von Wetterveränderungen, die eine Veränderung des Luftdrucks mit sich bringen. So kann die Ungenauigkeit nach einem Tag 30 Meter betragen. Der Höhenmesser sollte also vor dem Benutzen neugestartet werden, damit ein aktueller Ausgangsdruck gemessen werden kann.[[³]][Höhenformel]

#### Maximalwerte berechnen
Wenn die Höhe `a` größer als die Variable `highest` bzw. kleiner als `lowest` ist, wird dieser Höhenwert übernommen und damit zum neuen Maximum oder Minimum. Das Minimum benötigen wir nicht wirklich, berechnen es aber trotzdem für den Fall, dass es doch gebraucht wird.

```c++
if (a < lowest) lowest = a;

if (a > highest) highest = a;
```

***

### Anzeige auf Display <a name="10"></a>
Für das Betreiben des OLED-Displays werden neben der Wire-Library noch zwei weitere Libraries benötigt:
* **SSD1306Ascii** ermöglicht die einfache Darstellung von Buchstaben und Zahlen auf dem Display
* **SSD1306AsciiWire** ermöglicht die Kommuniktion mit dem Dispay via I²C.

Wie beim Höhenmesser muss wieder ein Objekt erstellt werden, damit die Funktionen der Library verwendet werden können. In diesem Fall haben wir es `oled` genannt.

```c++
SSD1306AsciiWire oled;
```

Zum Ansteuern des Displays muss nun die I²C-Adresse definiert werden. Beim Höhenmesser tut die Library das für uns, beim Display muss die Adresse manuell ermittelt werden. Dafür gibt es den [I²C-Scanner](https://playground.arduino.cc/Main/I2cScanner/), der nach I²C-Adressen der angeschlossenen Komponenten sucht. Dabei geholfen hat uns dieses [Tutorial](https://www.instructables.com/id/Monochrome-096-i2c-OLED-display-with-arduino-SSD13/). Im Falle des Display ist die Adresse 0x3C.

```c++
#define I2C_ADDRESS 0x3C 
```

Nun kann das Display-Setup beginnen: Zunächst wird mit `Wire.begin()` die Datenübertragung gestartet, dann kann das Display mit `oled.begin()` initialisiert werden. `&Adafruit128x64` ist der DevType also der allgemeine Typ des Geräts und `I2C_ADDRESS` übergibt die Adresse des Displays. `oled.set400kHZ()` legt den Takt der I²C-Übertragung fest und `oled.setFont(font5x8)` wählt die Größe der Schriftart. `oled.clear` leert den Bildschirm, um Darstellungsfehler zu verhindern. Dann vergrößern wir die Schriftgröße auf das doppelte und geben "Start" aus. Um den Setupprozess abzuschließen setzten wir die Schriftgröße wieder auf den Standard zurück und leeren nach einer Verzögerung das Display.

```c++
Wire.begin();
oled.begin(&Adafruit128x64, I2C_ADDRESS);
oled.set400kHz();
oled.setFont(font5x7);
oled.clear();
oled.set2X();
oled.println("START");
oled.set1X();
delay(500);
oled.clear();
```

Die Anzeigen der einzelnen Informationen sind dann alle ähnlich aufgebaut: Erst wird die Fontgröße und dann der Ort, an dem der Text dargestellt werden festgelegt. Die Parameter sind dabei die Spalten und Zeilen.
Dabei gilt es zu beachten, dass die angegebenen Zahlen negativ sein könnten. Das sieht dann in der Darstellung sehr komisch aus, wenn die Zahlen hin und her springen. Deshalb fügen wir ein Leerzeichen hinzu, wenn die zahl positiv ist.

```c++
oled.set2X();
oled.setCursor(40, 2);               
if (a >= 0.0) oled.print(" ");      
oled.print(a);
oled.print("m");
```
***

### WLAN-Verbindung oder Access Point <a name="11"></a>
Für das Erstellen eines eigenen WLAN-Access-Points oder das Verbinden mit bestehennden Netzwerken benötigt man die Library **ESP8266WiFi**.

Die Zugangsdaten zum jeweiligen Netzwerk definiert man als Konstanten:
```c++
const char *ssid = "xyz";  
const char *password = "xyz";
```
Im Setup können dann die benötigten IP-Adressen definiert werden. Das ist von Vorteil, damit das Gerät immer unter der gleichen IP-Adresse erreichbar ist.
```c++
IPAddress ip(192, 168, 178, 220);
IPAddress dns(192, 168, 178, 1);
IPAddress gateway(192, 168, 178, 1);
IPAddress subnet(255, 255, 255, 0);
WiFi.config(ip, dns, gateway, subnet);
```
Nach der Konfiguration kann die WLAN-Verbindung bzw. der Access-Point gestartet werden.

Verbindung zu bestehendem Netzwerk:
```c++
WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Verbunden mit:");
  Serial.println(ssid);
```

Starten eines eigenen Netzwerks:
```c++
 WiFi.softAP(ssid, password);         
  Serial.println("AccessPoint gestartet!");   
```
Quelle: https://www.xgadget.de/anleitung/esp8266-feste-ip-adresse-vergeben/

### Webserver <a name="12"></a>
Für ESP8266-Webserver gibt es die Library **ESP8266WebServer**.

#### Port setzen
Zuerst muss der HTTP-Port gesetzt werden unter dem der Webserver erreichbar ist. Der Standard ist 80.

```c++
ESP8266WebServer server(80);
```
#### Konfiguration
Wie beim WLAN konfiuriert man zunächst bei welcher URI, welche Funktion abgerufen werden soll. Dann kann der Server gestartet werden.
```c++
server.on("/", handleRoot);
server.on("/readData", handleData);
server.onNotFound(handleNotFound);
server.begin();
```

#### handleData
`handleData` sendet die Daten, die über chart.js graphisch dargestellt werden in Form eines Strings. Dieser enthät im die aktuelle Zeit (Sekunden seit dem booten) und den Höhenwert. Diese beiden Werte werden durch ein Semicolon getrennt.
```c++
void handleData(){
  double d = *pointerzwei;
  double t = millis() / 1000;
  String teil = String(String(t) + ";");
  String kombi = String(teil + String(d));
  server.send(200, "text/plain", kombi);
}
```
#### handleNotFound
Wird eine unbekannte URI aufgerufen greift die Funktion `handleNotFound` und sendet den HTTP-Fehlercode 404.
```c++
void handleNotFound(){              
  server.send(404, "text/plain", "404: Not found"); 
}
```

#### handleRoot
`handleRoot` schickt den HTTP-Code 200 ("OK") und den Code für die Homepage als String `s`. 
```c++
void handleRoot() {
  String s = MAIN_page;
  server.send(200, "text/html", s);
}
```

***
*Für diesen Teil haben wir uns am Tutorial "ESP8266 data logging with real time graphs" auf [circuits4you.com](https://circuits4you.com/2019/01/11/esp8266-data-logging-with-real-time-graphs/) orientiert. Bei der Anpassung an unsere Daten hat uns Nick Lamprecht mit seinen Java Skript-Kenntnissen geholfen. Alle Beiträge sind [hier](https://github.com/JantonDeluxe/luft-waffle/pulls?q=is%3Apr+is%3Aclosed) zu finden.*

***

#### Header
Den Code für diese Homepage könnte man auch in einer Zeile als String schicken, das ist aber sehr unübersichtlich. Deshalb haverwenden wir das header file `index`, dass den Code der Website enthält. Header sind ..... Damit die Arduino IDE das header file einem Programm zuordnen kann, muss die Datei im selben Ordner wie die .ino Datei, die den "Hauptcode" enthält gespeichert werden.
```c++
#include "index.h"
```
Den Code enthält das header file in Form einer Konstaten mit dem Namen `MAIN_page`. Da diese Datei zu groß für den Arbeitsspeicher des Boards ist (80 kb), muss diese Datei im Flash-Speicher (16 mb) gespeichert werden. Dafür gibt es den Variablenmodifikator [PROGMEN](https://www.arduino.cc/reference/de/language/variables/utilities/progmem/), der dafür sorgt, dass Daten im Flash gespeichert werden.
Die Trennzeichen (delimiter) `R"=====()====="` sorgen dafür, dass Sonderzeichen, Absätze usw. keine Syntaxfehler verursachen.
```c++
const char MAIN_page[] PROGMEM = R"=====(xyz)=====";
```
#### HTML/ JavaScript

Der Grundaufbau der Website ist wie folgt. Zur besseren Verständlichkeit gehen wir Schritt für Schritt vor.
```html
<!doctype html>
<html>
   
<head>  
</head>
   
<body>
</body>
   
</html>
```
##### Head
Der Seitentitel, der z.B. in der Titelleiste eines Browser-Tabs angezeigt wird lautet "Höhenmesser".
```html
<title>H&ouml;henmesser</title>
```
Danach wird das JavaSkript abgerufen, dass für die graphische Darstellung der Messwerte verantwortlich ist. Dafür verwenden wir [chart.js](https://www.chartjs.org/) eine "Open Source-JavaScript-Diagrammbibliothek für Designer und Entwickler". Das Skript befindet sich auf einem CDN-Server (CDN = *Content Delivery Network*) von dem aus es abgerufen wird.
```html
<script src = "https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.3/Chart.min.js"></script>
```

Die `user-select`-Eigenschaft von CSS verhindert hier, dass innerhalb der Grafik Text ausgewählt werden kann.
Als nächstes wird das Aussehen der Datentabelle unter der Grafik definiert. Dafür werden Funktionen der 
```html
<style>
  canvas{
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
  }

  // Aussehen der Tabelle
  #dataTable {
    font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  #dataTable td, #dataTable th {
    border: 1px solid #ddd;
    padding: 8px;
  }

  #dataTable tr:nth-child(even){background-color: #f2f2f2;}

  #dataTable tr:hover {background-color: #ddd;}

  #dataTable th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #4CAF50;
    color: white;
  }
  </style>
```
```html
<body>
    <div style="text-align:center;">
    <h1 style="font-family:verdana;color:#999999">H&ouml;henmesser</h1>
    </div>
    <div class="chart-container" style="position: relative; height:350px; width:100%">
        <canvas id="Chart" width="400" height="400"></canvas>
    </div>
<div>
  <table id="dataTable">
    <tr><th>Zeit</th><th>H&ouml;he</th></tr>
  </table>
</div>

<script>
  var data = [];
  var time = "";

  let graph;

  function addGraph()
  {
    var ctx = document.getElementById("Chart").getContext('2d');
    graph = new Chart(ctx, {
      type: 'line',
        data: {
        labels: [time],  //Bottom Labeling
        datasets: [{
          label: "H\u00f6he",
          fill: false,  //Try with true
          backgroundColor: 'rgba( 243, 156, 18 , 1)', //Dot marker color
          borderColor: 'rgba( 243, 156, 18 , 1)', //Graph Line Color
          data: null, // Wenn es noch keine Daten gibt wird 'null' angezeigt
        }],
        },
        options: {
          title: {
            display: true,
            text: "H\u00f6he"
          },
          maintainAspectRatio: false,
          elements: {
            line: {
              tension: 0.5 //Smoothening (Curved) of data lines
            }
          },
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero:true
              }
            }]
        }
      }
    })
  }

  // Graphen nach erhalt neuer Daten aktualisieren
  function updateGraph() {
    graph.data.labels.push(time);
    graph.data.datasets.forEach((dataset) => {
        dataset.data.push(data);
    });
    graph.update();
  }

  //Beim Starten der Seite Graphen anzeigen
  window.onload = function() {
    addGraph();
  };

  // Update-Geschwindigkeit (500 ms)
  setInterval(function() {
    getData();
  }, 333); 


  // Graph mit Daten anzeigen
  function showGraph(data) {
    graph.data.labels.push(timeStamp);
    graph.data.datasets.forEach((dataset) => {
        dataset.data.push(data);
    });
    graph.update();
  }

  // Daten unter /readData abrufen
  function getData() {
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
      if (this.readyState == 4 && this.status == 200) {
        var response = this.responseText.split(";"); // Erstellt ein array aus den Teilen des response-strings (';' ist das Trennzeichen)
        time = response[0]; // Zeit hat den ersten Index...
        data = response[1]; // ...und die Höhe den zweiten
        updateGraph();
        var table = document.getElementById("dataTable");
        var row = table.insertRow(1); //Add after headings
        var cell1 = row.insertCell(0);
        var cell2 = row.insertCell(1);
        cell1.innerHTML = time;
        cell2.innerHTML = data;
      }
    };
    xhttp.open("GET", "readData", true);
    xhttp.send();
  }
</script>
</body>

</html>




## Quellen <a name="13"></a>
[BMP180-Datenblatt]:https://ae-bst.resource.bosch.com/media/_tech/media/datasheets/BST-BMP180-DS000.pdf
[I²C]:https://www.ipd.kit.edu/mitarbeiter/buchmann/microcontroller/i2c.htm
[Sparkfun]:https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all

