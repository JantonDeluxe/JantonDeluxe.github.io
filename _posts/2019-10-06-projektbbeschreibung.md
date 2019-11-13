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
>    - [Arduino-Sketch](#8)
>    - [Website](#9)
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

   
## Softwarea <name="7"></a>
Die Software besteht aus zwei Teilen. Dem [Arduino-Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/WebserverTest3/WebserverTest3.ino) und der [Website](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/WebserverTest3/index.h).

### Arduino-Sketch <a name="8"></a>
Der Arduino-Sketch ist das "Hauptprogramm" des Höhenmessers. Hierüber läuft alles bis auf die Darstellung der Werte auf der Website.

#### Libraries
Libraries enthalten Code-Teile, wie z.B. vorgefertigte Funktionen oder Maschinencode, auf die dann im eigenen Programm einfach zugegriffen werden kann, ohne sie dort selbst schreiben oder einfügen zu müssen.

Für unser Projekt benötigen wir folgende Libraries:
```c++
#include <SFE_BMP180.h>        
#include <Wire.h>
#include <SSD1306Ascii.h>
#include <SSD1306AsciiWire.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
#include <FS.h>
```
- **SFE_BMP180** ist die Library, die den Maschinencode für den Höhenmesser enthält. Damit ermöglicht sie das auslesen und ansteuern des Höhenmessers. 
- **Wire**  sorgt dafür, dass der Arduino mit dem Höhenmesser kommunizieren kann, da dieser zur Kommunikation das I²C-Protokoll benutzt.
- **SSD1306Ascii** ermöglicht die einfache Darstellung von Buchstaben und Zahlen auf dem Display
- **SSD1306AsciiWire** ermöglicht die Kommuniktion mit dem Dispay via I²C.
- **ESP8266WiFi** ermöglicht Netzwerkverbindungen
- **ESP8266WebServer** ist eine einfache Webserver-Library, die HTTP-Verbindungen mit maximal einem Client unterstützt
- **FS** ist die Library für das SPIFFS-Dateisystem

#### Instanzen
Die Funktionen der Libraries sind jeweils in einer Klasse (z.B. `SFE_BMP180`) gespeichert. Um diese Funktionen abrufen zu können, muss man eine Instanz (hier: `pressure`) der jeweiligen Klasse erstellen.
```c++
SFE_BMP180 pressure;
SSD1306AsciiWire oled;
ESP8266WebServer server(80);
```
Der Parameter 80 für die Instanz `server()` gibt in diesem Fall zusätzlich den Port an auf dem der Webserver erreichbar ist. 80 ist der Standard-Port für HTTP-Verbindungen.

#### Funktionen
##### Druckmessung
Für die Druckmessung haben wir die Funktion `getPressure()` aus dem 
Höhenmesser Example-Sketch übernommen. Die Variablen verwenden wir jedoch 
auch an anderen Stellen des Codes, weshalb wir sie als globale statt 
lokale Variablen deklarieren.

Zuerst muss eine Temperatur-Messung durchgeführt werden. Mit 
`pressure.startTemperature()` wird die Messung begonnen. Bei einem Fehler 
gibt die Funktion den Wert 0, sonst die Zeit, die für die Messung gebraucht
 wird in Millisekunden aus. Solange wird gewartet. 
 `pressure.getTemperature(T)` verändert dann den Wert der Variable T, 
 sodass er dem Temperaturwert entspricht. Ist das erfolgreich gibt die 
 Funktion den Wert 1, sonst den Wert 0 aus.

Jetzt kann der Druck gemessen werden: `pressure.startPressure` beginnt 
die Messung (in diesem Fall auf Genauigkeitsstufe 3 von 3) und gibt 
entweder 0 als Fehler oder die Wartezeit in Millisekunden aus. 
`pressure.getPressure` berechnet dann auf Basis von `T` den Druck `P` in 
Milibar. Die Temperatur muss eingerechnet werden, da sich nach dem 
allgemeinen Gasgesetz der Druck bei ändernder Temperatur und 
gleichbleibendem Volumen ebenfalls verändert.  Ist das erfolgreich gibt 
die Funktion den Wert 1, sonst den Wert 0 aus. Wird 0 ausgegeben, sendet 
die Funktion die entsprechende Fehlermeldung seriell an einen 
angeschlossenen Computer.

```c++
double getPressure()
{ 
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
##### Website senden
`handleRoot` schickt den HTTP-Code 200 ("OK") und den Code für die
 Homepage als String `s`. 
```c++
void handleRoot() {
  String s = MAIN_page;
  server.send(200, "text/html", s);
}
```
Den String `s` könnte man auch als den Code der Homepage in einer Zeile 
definieren. Das ist aber sehr unübersichtlich. 
Eleganter ist der Weg aus dem Tutorial "ESP8266 data logging with real time 
graphs" auf [circuits4you.com](https://circuits4you.com/2019/01/11/esp8266-data-logging-with-real-time-graphs/). 
Dabei enthält die Konstante `MAIN_page` den Code der Website. Diese Konstante ist 
in der Datei `index.h` definiert, das wir zu Beginn des Programm hinzugefügt
 haben. Solche Dateien nennt man "header".
```c++
#include "index.h"
```
Damit die Arduino IDE das header file einem Programm zuordnen kann, muss 
die Datei im selben Ordner wie die .ino Datei, die den "Hauptcode" enthält
 gespeichert werden.
 
Den Code enthält das header file in Form einer Konstaten mit dem Namen `MAIN_page`. Da diese Datei zu groß für den Arbeitsspeicher des Boards ist (80 kb), muss diese Datei im Flash-Speicher (16 mb) gespeichert werden. Dafür gibt es den Variablenmodifikator [PROGMEN](https://www.arduino.cc/reference/de/language/variables/utilities/progmem/), der dafür sorgt, dass Daten im Flash gespeichert werden.
Die Trennzeichen (delimiter) `R"=====()====="` sorgen dafür, dass Sonderzeichen, Absätze usw. keine Syntaxfehler verursachen.
```c++
const char MAIN_page[] PROGMEM = R"=====(xyz)=====";
```
   
##### Messwerte senden
`handleData` sendet die Daten, die auf der Website graphisch dargestellt
 werden in Form eines Strings. Dieser enthät die aktuelle Zeit 
 (in Sekunden seit dem Booten) und den Höhenwert. Diese beiden Werte werden 
 durch ein Semicolon getrennt.
```c++
void handleData(){
  double d = *pointerzwei;
  double t = millis() / 1000;
  String teil = String(String(t) + ";");
  String kombi = String(teil + String(d));
  server.send(200, "text/plain", kombi);
}
```
   
##### Unbekannte URLs 
Wird eine URL aufgerufen, die vorher nicht definiert wurde greift die 
Funktion `handleWebRequests`. In diesem Fall gibt es zwei Möglichkeiten: 
* die URL existiert wirklich nicht. In diesem Fall wird der HTTP-Code 404 und eine nachricht, dass nichts gefunden wurde versendet
* es soll eine Datei aus dem Speicher abgerufen werden. Wenn die URL zu einer im Speicher vorhandenen Datei passt, wird 
die Funktion `loadFromSpiffs` aufgerufen, die den [MIME-Typ](https://wiki.selfhtml.org/wiki/MIME-Type/%C3%9Cbersicht) der 
Datei feststellt, sie öffnet, verschickt und dann wieder schließt.

Diese Funktionen sind aus dem Tutorial [ESP8266 jQuery and AJAX Web Server](https://circuits4you.com/2018/03/10/esp8266-jquery-and-ajax-web-server/) übernommen.
```c++
void handleWebRequests(){
  if(loadFromSpiffs(server.uri())) return;
  String message = "File Not Detected\n\n";
  message += "URI: ";
  message += server.uri();
  message += "\nMethod: ";
  message += (server.method() == HTTP_GET)?"GET":"POST";
  message += "\nArguments: ";
  message += server.args();
  message += "\n";
  for (uint8_t i=0; i<server.args(); i++){
    message += " NAME:"+server.argName(i) + "\n VALUE:" + server.arg(i) + "\n";
  }
  server.send(404, "text/plain", message);
  Serial.println(message);
}
}
```
```c++
bool loadFromSpiffs(String path){
  String dataType = "text/plain";
  if(path.endsWith("/")) path += "index.htm";
 
  if(path.endsWith(".src")) path = path.substring(0, path.lastIndexOf("."));
  else if(path.endsWith(".html")) dataType = "text/html";
  else if(path.endsWith(".htm")) dataType = "text/html";
  else if(path.endsWith(".css")) dataType = "text/css";
  else if(path.endsWith(".js")) dataType = "application/javascript";
  else if(path.endsWith(".png")) dataType = "image/png";
  else if(path.endsWith(".gif")) dataType = "image/gif";
  else if(path.endsWith(".jpg")) dataType = "image/jpeg";
  else if(path.endsWith(".ico")) dataType = "image/x-icon";
  else if(path.endsWith(".xml")) dataType = "text/xml";
  else if(path.endsWith(".pdf")) dataType = "application/pdf";
  else if(path.endsWith(".zip")) dataType = "application/zip";
  File dataFile = SPIFFS.open(path.c_str(), "r");
  if (server.hasArg("download")) dataType = "application/octet-stream";
  if (server.streamFile(dataFile, dataType) != dataFile.size()) {
  }
 
  dataFile.close();
  return true;
}
```

#### Setup
Im Setup werden alle Softwarekomponenten einmalig gestartet, sodass sie 
später im Betrieb funktionieren.

##### Serielle Schnittstelle
Als erstes starten wir die Übertragung via serieller Schnittstelle, 
die wir für die Übertragung von Daten an den PC nutzen. Diese Überträgt
 per Micro-USB-Kabel Daten an den PC, die mit dem seriellen Monitor in 
 der Arduino IDE ausgelesen werden können.

`Serial.begin()` startet die Übertragung, in diesem Falle mit einer
 Übertragungsgeschwindigkeit von 115200 Bits pro Sekunde.

`Serial.println("")` schreibt Daten als ASCII-Text und beginnt danach 
im Gegensatz zu `Serial.Print()` eine neue Zeile. Hier verhindert eine
 leere Ausgabe Darstellungsfehler zu Beginn der Übertragung.

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

##### I²C-Setup
```c++
Wire.begin();
Serial.println("I2C gestartet!");
```

##### Display-Setup
Zum Ansteuern des Displays muss noch vor dem Setup die I²C-Adresse definiert werden.
 Beim Höhenmesser tut die Library das für uns, beim Display muss die 
 Adresse manuell ermittelt werden. Dafür gibt es den 
 [I²C-Scanner](https://playground.arduino.cc/Main/I2cScanner/), der nach 
 I²C-Adressen der angeschlossenen Komponenten sucht. Dabei geholfen hat 
 uns dieses [Tutorial](https://www.instructables.com/id/Monochrome-096-i2c-OLED-display-with-arduino-SSD13/). 
 Im Falle des Display ist die Adresse 0x3C.

```c++
#define I2C_ADDRESS 0x3C 
```
Dann kann das Display mit `oled.begin()` initialisiert werden. 
`&Adafruit128x64` ist der DevType also der allgemeine Typ des Geräts und 
`I2C_ADDRESS` übergibt die Adresse des Displays. `oled.set400kHZ()` legt 
den Takt der I²C-Übertragung fest und `oled.setFont(font5x8)` wählt die 
Größe der Schriftart. `oled.clear` leert den Bildschirm, um 
Darstellungsfehler zu verhindern. Dann vergrößern wir die Schriftgröße 
auf das doppelte und geben "Start" aus. Um den Setupprozess abzuschließen 
setzten wir die Schriftgröße wieder auf den Standard zurück und leeren 
nach einer Verzögerung das Display.
```c++
  oled.begin(&Adafruit128x64, I2C_ADDRESS);
  oled.set400kHz();
  oled.setFont(font5x7);
  
  Serial.println("Display gestartet!"); 

  oled.clear();
  oled.set2X();
  oled.println("START");
  oled.set1X();
  delay(500);
  oled.clear();
```

##### Senor-Setup
Der nächste Schritt ist es, den Sensor zu starten: Das tut die 
Funktion `pressure.begin()` aus der Höhenmesser-Library. 
Wenn dabei ein Fehler auftreten sollte wird immer wieder eine Schleife 
wiederholt und das Programm damit gestoppt.

```c++
 if (pressure.begin())
    Serial.println("BMP180 gestartet!");
  else
  {
    Serial.println("BMP180 fehlt!");
    while (1);
  }
```

##### Access Point-Setup
Bevor der Access Point des Höhenmessers gestartet werden kann, muss das Netzwerk konfiguriert werden.
Wir tun dies, damit die Seite, auf der der Graph der Höhe angezeigt wird eine statische 
IP-Adresse hat. Dafür muss man ebenfalls man ebenfalls die IP-Adresse des
Gateways und die Adresse der Subnetzmaske manuell definieren.
```c++
  IPAddress ip(192,168,4,22);
  IPAddress gateway(192,168,4,9);
  IPAddress subnet(255,255,255,0);

  WiFi.softAPConfig(local_IP, gateway, subnet);
  WiFi.softAP(ssid, password);

  Serial.println("Access Point getstartet!");
```

##### Dateisystem-Setup
Als nächstes wird das Dateisystem SPIFFS gestartet. SPIFFS ist ein einfaches Dateisystem
, dass speziell für kleine Microcontrollerboards mit wenig Arbeitsspeicher 
(RAM) entwickelt wurde. Um etwas im über SPIFFS speichern zu können, muss man das
[ESP8266 filesystem uploader](https://github.com/esp8266/arduino-esp8266fs-plugin)-Plugin installieren. Diese fügt ind der Arduino IDE unter Werkzeuge das
Tool "ESP8266 Sketch Data Upload" hinzu, dass Datei aus dem im Arduino_Projektordner zu erstellenden Ordner "data" in den SPIFFS-Speicher lädt.
Tritt dabei ein Fehler auf wird wie beim Sensor-Setup verfahren.
```c++
 if(SPIFFS.begin())
   {
     Serial.println("SPIFFS gestartet!");
   }
   else
   {
     Serial.println("SPIFFS nicht gestartet!");
     while (1);
    }
```

##### Webserver-Setup
Zunächst wird definiert, bei welcher [URI](https://wiki.selfhtml.org/wiki/URI), welche Funktion abgerufen werden
 soll. Wird keine URI gefunden greift die Funktion `handleWebRequests`.
 Dann kann der Server gestartet und seine IP-Adresse ausgegeben werden. 
 ```c++
  server.on("/", handleRoot);
  server.on("/readData", handleData);
  server.onNotFound(handleWebRequests);
  server.begin();

  Serial.println("HTTP-Server gestartet!"); 
  Serial.print("IP: ");
  Serial.println(WiFi.softAPIP()); 
  ```
  
##### Basisdruck
Der Höhenmesser berechnet seine Höhenangaben aus der Differenz des
 aktuell gemessenen Drucks und dem Ausgangsdruck. Für diesen 
 Ausgangsdruck deklarieren wir vor dem Setup eine eigene Variable `ausgangsdruck`, das
 Array `Array` und die Konstante `messungen`.

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

##### statische Teile der Höhenanzeigen
Die Teile der Höhenanzeigen, die sich nicht verändern lassen wir schon 
während des Setups anzeigen. Das bedeutet, dass sie im laufenden Betrieb nicht andauernd neu
geladen werden müssen, was Übertragungskapazitäten einspart.
```c++
  oled.setCursor(0, 3);
  oled.print("Hoehe:");
  oled.setCursor(0,5);
  oled.print("Max:");
```
Mit `oled.SetCursor` bestimmt man den Ort auf dem Display, auf dem der Text angezeigt 
werden soll.

#### Hauptcode

##### Webserver
Die funktion `server.handleClient` wartet auf HTTP-Anfragen und ruft dann je nach URI eine
vorher definierte Funktion auf.

##### Höhenberechnung
Die Methode `pressure.altitude()` berechnet mit Hilfe der Barometrischen 
Höhenformel den zurückgelegten Höhenunterschied basierend auf dem 
aktuellen Druck, der durch die Funktion `getPressure()` ausgegeben wird,
und dem Ausgangsdruck. Dieses Ergebnis setzten wir dann der Höhenvariable
 `a` gleich. Beide Variablen wurden vor dem Setup deklariert.
```c++
double P;
double a;
```

```c++
P = getPressure();

a = pressure.altitude(P, ausgangsdruck);
```
Wie gesagt liegt der Berechnung die internationale barometrische Höhenformel zu Grunde:
![Barometrische Höhenformel](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Druck.jpg?raw=true)
_P_  ist hierbei der aktuelle Druck in hPa und P0 der Basisdruck hPa. 
Die Konstante 44330 kommt durch die Annahme zu Stande, dass die Temperatur um 6,5°C pro Höhenkilometer sinkt und eine Druckänderung von 1 hPa 
einen Höhenunterschied von etwa 8 Metern ausmacht.
In der Theorie sollte der BMP180 mit dieser Methode eine Genauigkeit
 von 25 cm erreichen. In der Praxis liegt die Ungenauigkeit bei eitwas 
 über 1 Meter. Die Genauigkeit verschlechtert sich mit der Zeit unter 
 anderem auf grund von Wetterveränderungen, die eine Veränderung des 
 Luftdrucks mit sich bringen. So kann die Ungenauigkeit nach einem Tag 
 30 Meter betragen. Der Höhenmesser sollte also vor dem Benutzen 
 neugestartet werden, damit ein aktueller Ausgangsdruck gemessen werden 
 kann.

##### Maximalwerte berechnen
Wenn die Höhe `a` größer als die Variable `highest` bzw. kleiner als `lowest`
 ist, wird dieser Höhenwert übernommen und damit zum neuen Maximum 
 oder Minimum. Das Minimum benötigen wir nicht wirklich, berechnen es 
 aber trotzdem für den Fall, dass es doch gebraucht wird.
```c++
if (a < lowest) lowest = a;

if (a > highest) highest = a;
```

##### Anzeige auf Display
Die Anzeigen der einzelnen Informationen sind dann alle ähnlich 
aufgebaut: Erst wird die Fontgröße und dann der Ort, an dem der 
Text dargestellt werden festgelegt. Die Parameter sind dabei die 
Spalten und Zeilen.
Dabei gilt es zu beachten, dass die angegebenen Zahlen negativ 
sein könnten. Das sieht dann in der Darstellung sehr komisch aus, 
wenn die Zahlen hin und her springen. Deshalb fügen wir ein Leerzeichen
 hinzu, wenn die zahl positiv ist.

```c++
oled.set2X();
oled.setCursor(40, 2);               
if (a >= 0.0) oled.print(" ");      
oled.print(a);
oled.print("m");
```

## Website <a name="9"></a>
Basis für den Website-Code ist Tutorial "ESP8266 data logging with real
 time graphs" auf [circuits4you.com](https://circuits4you.com/2019/01/11/esp8266-data-logging-with-real-time-graphs/)
Bei der Anpassung an unsere Daten hat uns Nick Lamprecht mit seinen 
Java Skript-Kenntnissen geholfen. Alle Beiträge sind 
[hier](https://github.com/JantonDeluxe/luft-waffle/pulls?q=is%3Apr+is%3Aclosed) zu finden.*

Der Seitentitel, der z.B. in der Titelleiste eines Browser-Tabs angezeigt wird lautet "Höhenmesser".
```html
<!doctype html>
<html>
<head>
<title>H&ouml;henmesser</title>
```
Danach wird das JavaSkript abgerufen, dass für die graphische Darstellung der Messwerte verantwortlich ist. 
Dafür verwenden wir [chart.js](https://www.chartjs.org/) eine 
"Open Source-JavaScript-Diagrammbibliothek für Designer und Entwickler". 
Die Datei hat den Namen "Chart.min.js" und befindet sich im Flash-Speicher des D1 mini Pro.
```html
<script src = "Chart.min.js"></script>
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
  </head>
```
Dann wird die Überschrift "Höhenmesser" definiert. Der div "chart-container" definiert die Größe und den Canvas des Diagramms, sodass der Ort an dem 
Chart.js das Diagramm erstellen soll definiert ist. Unter dem Diagramm wird dann die Tabelle eingefügt.
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
```

Für die Folgenden JavaScript-Funktionen benötigen wir folgende Variablen. Bei `data` handelt es sich hierbei um ein Array.
```html
<script>
  var data = [];
  var time = "";
  
  let graph;
  ```
  
### Funktion addGraph()
Die Funktion `addGraph` erstellt einen neuen Chart.js-Linien-Graphen mit der Höhe auf der x- und der Zeit auf der y-Achse.
Größe, Farben, Beschriftungen usw. werden hier ebenfalls definiert. Die Variablen hierfür sind vom Chart.js-Skript vorgegeben.
```js
function addGraph()
  {
    var ctx = document.getElementById("Chart").getContext('2d');
    graph = new Chart(ctx, {
      type: 'line',
        data: {
        labels: [time],  //x-Achse
        datasets: [{
          label: "H\u00f6he",
          fill: false, 
          backgroundColor: 'rgba( 243, 156, 18 , 1)', // Punkt-farbe
          borderColor: 'rgba( 243, 156, 18 , 1)', // Graphlinien-Farbe
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
  ```
  ### Funktion updateGraph
  Die Funktion `updateGraph` aktualisiert den Graphen.
  `push(time)` fügt dabei den neuen Zeitstempel und `push(data)` den neuen Höhenwert hinzu.
 
  ```js
   function updateGraph() {
    graph.data.labels.push(time);
    graph.data.datasets.forEach((dataset) => {
        dataset.data.push(data);
    });
    graph.update();
  }
  ```
  
  Beim Starten der Website wird nun der Graph unter `addGraph()` definierte Graph angezeigt.
  ```js
   window.onload = function() {
    addGraph();
  };
  ```
  
  ### Update-Intervall
  Die Update-Geschwindigkeit für das abrufen neuer Daten haben wir auf 333 Millisekunden gesetzt, um auch bei schnellen 
  Bewegungen noch eine relativ genaue Darstellung zu haben.
   ```js
   setInterval(function() {
    getData();
  }, 333); 
    ```
	
### Funktion getData
Die Funktion `getData` sendet einen HTTP-GET-Request an die URI "readData" des Webservers, der dann einen kombinierten String
zurück schickt. Dieser kann nun dank des ";" als Trennzeichen geteilt werden. Die Variablen `data` und `time` werden dann
aktualisiert und in die Tabelle geschrieben.
```js
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
```

## Quellen <a name="13"></a>
[BMP180-Datenblatt]:https://ae-bst.resource.bosch.com/media/_tech/media/datasheets/BST-BMP180-DS000.pdf
[I²C]:https://www.ipd.kit.edu/mitarbeiter/buchmann/microcontroller/i2c.htm
[Sparkfun]:https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all

