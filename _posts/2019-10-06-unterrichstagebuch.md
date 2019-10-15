---
layout: page
title: Unterrichtstagebuch
subtitle: Was wir an welchem Tag gemacht haben
---

### Inhaltsverzeichnis
>* [13. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#13-august)
>* [14. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#14-august)
>* [15. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#15-august)
>* [20. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#20-august)
>* [21. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#21-august)
>* [22. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#22-august)
>* [27. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#27-august)
>* [28. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#28-august)
>* [29. August](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#29-august)
>* [11. September](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#11-september)
>* [12. September](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#12-september)
>* [24. September](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#24-september)
>* [25. September](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#25-september)
>* [26. September](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#26-september)
>* [1. Oktober](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#1-oktober)
>* [2. Oktober](https://github.com/JantonDeluxe/luft-waffle/wiki/Unterrichtstagebuch#2-oktober)
>* []()
>* []()
>* []()

## 13. August
In unserer ersten Informatikstunde haben wir uns überlegt, was für Projekte wir gerne machen würden und auf [code.org](https://code.org/) mit einem Einsteiger-Kurs angefangen.
Unsere Vorkenntnisse sind auf einfache Programmieraufgaben mit [Scratch](https://de.wikipedia.org/wiki/Scratch_(Programmiersprache)) im Rahmen des Jugendwettbewerbs Informatik beschränkt.

## 14. August
Heute haben wir einen [GitHub-Account](https://github.com/JantonDeluxe), ein [Repository](https://github.com/JantonDeluxe/luft-waffle) und unsere [Website](https://jantondeluxe.github.io/luft-waffle/) erstellt, die wir über den [editor](https://github.com/JantonDeluxe/luft-waffle/edit/master/README.md) bearbeiten.
Für die Website benutzen wir vorläufig das [Jekyll](https://jekyllrb.com/)-Theme Minimal, sind aber noch auf der Suche nach einem besseren Theme.

## 15. August
Heute haben wir uns für ein Arduino-Projekt entschieden und dafür angefangen, uns mit dem [Arduino](https://arduino.cc) und der Programmiersprache [C](https://de.wikipedia.org/wiki/C_(Programmiersprache)) auseinanderzusetzten. Ebenfalls haben wir Links in unsere Website eingefügt und weiterüberlegt, wie wir am besten lernen und was für ein Projekt genau wir umsetzen wollen.

## 20. August
Am Wochenende hatten wir uns dafür entschieden, einen Höhenmesser für Wasserraketen zu bauen. Diese Art von Höhenmesser kostet off-the-shelf mindestens 50 €, weshalb wir Geld sparen und den Sensor selbst bauen wollen. Basieren tut der Höhenmesser auf einem Versuch, den ich, Anton, vor vier Jahren mit meinem Vater unternommen habe. 

Dieser [alte Höhenmesser](http://jan.krummrey.de/2015/09/13/hoehenmesser-fur-unsere-wasserrakete/) hatte jedoch einige Schwachstellen, weshalb wir ihn nie zuende gebaut haben: 
- durch die zwei 7-Segment-Anzeigen konnte immer nur eine Information mit maximal zwei Ziffern angezeigt werden 
- es konnten keine Daten ausgelesen werden, da der Arduino nano sich bei USB Verbindungen resettet
- es konnten keine längeren Messreihen wie Flugverläufe gespeichert werden, da der Speicher dafür nicht ausgereicht hat
- der Arduino nano hat kein WLAN, weshalb man sich immer per USB-Kabel verbinden muss, was schwer möglich ist, wenn der Sensor in einer Rakete eingebaut ist

![Alter Höhenmesser](https://i1.wp.com/jan.krummrey.de/wp-content/uploads/2015/09/11951281_10153345040958153_9145140276330392700_n.jpg)

Wir haben uns also dafür entschieden, ein hochauflösenderes Display statt den 7-Segmentanzeigen und ein Board mit WLAN und mehr Speicherplatz zu verwenden. Das Display ist nun ein 128×64 Pixel GM009605 OLED-Display mit SSD1306-Controller aus China und das neue Board ein [D1 mini Pro V1.0](https://wiki.wemos.cc/products:retired:d1_mini_pro_v1.1.0) mit ESP8266-Mikrocontroller von WEMOS Electronics.

## 21. August
Zuhause haben wir die Aufgaben 1 bis 4 von StarlogoTNG erfolgreich abgeschlossen. 

In der Stunde haben wir angefangen den Laptop für unser Projekt einzurichten:
- Laptop in iSurf registriert
- [Arduino IDE](https://www.arduino.cc/en/Guide/Windows) installiert
- [D1 mini Pro Hardware Package](https://github.com/esp8266/Arduino) über die Arduino IDE installiert 

  (_Voreinstellungen_ -> _zusätzliche Boardverwalter-URLs_ -> Link einfügen)

- [D1 mini Pro-Treiber](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) installiert

Der alte Höhenmesser sollte über das zum Sensor gehörende [Example-Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/BMP180_altitude_example/BMP180_altitude_example.ino) die aktuelle Höhe messen und auf den Anzeigen darstellen. Zusätzlich konnte die maximale Höhe berechnet werden und durch das Blinken der im Arduino eingebauten LED ausgegeben werden. Das funktioniert, ist aber nicht sehr "nutzerfreundlich". Der Code ist leider in den vier Jahren verloren gegangen, basierte aber bis auf das Berechnen des Maximalwerts und dem Blinken der LED auf dem Example Sketch.

Wenn möglich soll der neue Höhenmesser mehr können:
1. aktuelle Höhe auf Display anzeigen
2. maximale Höhe möglichst auf unter 1 Meter genau auf Display anzeigen
3. Temperatur und Druckwerte auf Display anzeigen
4. Daten auf Webserver anzeigen
5. Geschwindigkeit und Beschleunigung ausrechnen
6. Flugverlauf grafisch darstellen

## 22. August
Heute haben wir den Höhenmsser (ein [Bosch BMP 180-Sensor](https://www.sparkfun.com/products/retired/11824)) mit Hilfe des [Sparkfun-Tutorials](https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all) mit dem Mikrocontroller auf einem Breadboard verkabelt.

Übersicht; welcher Pin wohin ;)

Dann wollten wir zum Testen das Example-Sketch auf den Mikrocontroller laden, was leider nicht funktioniert hat:
```
esptool.FatalError: Timed out waiting for packet headed

```
Deshalb haben wir statt der neuen Universal Windows Platform App aus dem Microsoft Store die Arduino IDE diesmal als normales Windows-Programm (über den .exe Installer) installiert, was das Problem aber nicht behoben hat. Sicherheitshalber haben wir deshalb den D1-Treiber ebenfalls neu installiert und weiter nach dem Fehler gesucht.

Parallel haben wir dieses Unterrichtstagebuch fortgeführt.

## 27. August
Heute haben wir endlich den Fehler gefunden, warum unser Board nicht mit der Arduino IDE kommunizieren konnte: Wir hatten vergessen, das Board engültig zu installieren. Das geht in der Arduino IDE unter

_Werkzeuge_ -> _Board_ -> _Boardverwalter_ -> Suche “ESP8266” (der ESP8266 ist der auf unserem Board verbaute Mikrocontroller) -> _Installieren_. 

Zusätzlich muss man für unser spezielles Board weitere Zusatzeinstellungen in den prefrences.txt hinzufügen, wofür es glücklicherweise eine [Vorlage](https://arduino-projekte.info/wp-content/uploads/2017/07/boards.txt) gibt. Dann muss man die Arduino IDE nur noch neustarten und unter 

_Werkzeuge_ -> _Board_ -> "LOLIN(WEMOS) D1 mini Pro" auswählen.

Sehr geholfen haben uns bei der Installation diesese beiden Tutorials: 
- (https://arduino-projekte.info/installation-eps8266-modul-wie-z-b-wemos/)
- (https://arduino-projekte.info/installation-wemos-d1-mini-lite-wemos-d1-mini-pro/)

Wenn wir diesen Schritt nicht vergessen hätten, hätte das Hochladen des Programms trotzdem nicht funktioniert, da auf der Hersteller-Website des D1 eine veraltete Boardverwalter-URL angegeben war, was uns nur durch Zufall aufgefallen ist. 

## 28. August
Heute konnten wir endlich testen, ob wir den Sensor richtig verkabelt haben. Dafür haben wir das oben erwähnte Example Sketch auf den D1 hochgeladen und die Daten ausgelsen. Das funktioniert so:

In Text-Form: _Werkzeuge_ -> _Serieller Monitor_

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Serieller%20Monitor.png?raw=true)

Als Graph: _Werkzeuge_ -> _Serieller Plotter_

(Diese Funktion ist neu, vor vier Jahren brauchten wir für die graphische Darstellung noch ein extra Processing-Skript.)

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Serieller%20Plotter.png?raw=true)

_Blau: Höhe in Meter_
_Rot: Höhe in feet_

Die Verkabelung ist also korrekt. Aus Spaß haben wir dann noch das zweite [Example Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/SFE_BMP180_example/SFE_BMP180_example.ino), das eigentlich für Wettermessungen gedacht ist ausprobiert.
Diesen Beispiel-Code findet man in der Arduino IDE unter: 

_Datei_ -> _Beispiele_ -> _BMP180_ -> _Examples_ -> _SFE_BMP180_example_

Dabei haben wir die vorgegebene Höhe (Boulder, Colorado: 1655 Meter) auf 0 Meter gesetzt, um die gesuchte relative Höhe zu erhalten:
```c
#define ALTITUDE 0.0
```
Danach wurde uns allerdings erstmal trotz Bewegung dauerhaft die Höhe 0 angezeigt:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Example%20Sketch%202.png?raw=true)

Letzlich lag das daran, dass die Höhe nur in ganzen Zahlen ausgegeben wurde und die Bewegungen kleiner als 1 Meter waren, weshalb keine neue Höhe angezeigt wurde.

Mit dem ersten Example Skecth haben wir dann den Zeitraum gemessen, in dem die Messwerte eine Ungenauigkeit von 2 Metern übersteigen. Der Zeitraum beträgt 18 Minuten. Um sicher zugehen, dass die Abweichung nicht zu groß wird, sollte also etwa alle 10 Minuten eine Rekalibrierung vorgenommen werden, um einen neuen Nullwert zu errechnen.

## 29. August
Heute haben wir mit Hilfe dieses  das OLED-Display in Betrieb genommen.

Übersicht welcher Pin wohin

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Zusammengesteckt%20auf%20Breadboard1.jpeg?raw=true)

Nachdem das Display verkabelt war mussten wir für unser testprogramm die nun die benötigten Libraries installieren:
```c
#include <Wire.h>             // Arduino-Standard-Library für die Datenübertragung (I2C)
#include <SSD1306Ascii.h>     // Library für die Buchstabendarstellung auf dem Display
#include <SSD1306AsciiWire.h> // Library für die Übertragung auf das OLED-Display 
```
Damit die Library SSD1306AsciiWire funktioniert muss man ein Objekt mit dem Namen "oled" initialisieren:
```c
SSD1306AsciiWire oled;    
```
Um das Display später auch ansteuern zu können, muss man die I2C-Adresse des Displays herausfinden. Dabei geholfen hat uns dieses [Tutorial](https://www.instructables.com/id/Monochrome-096-i2c-OLED-display-with-arduino-SSD13/). Für diese Aufgabe gibt es nämlich den [I2C-Scanner](https://playground.arduino.cc/Main/I2cScanner/), den man auf das Board lädt und der einem dann die Adresse des Displays ausgibt. In unserem Fall ist das die Adresse 0x3C (vorher hatten wir den Sensor abgesteckt, damit uns nur die Adresse des Displays angezeigt wird). 

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/I2c-Scan.png?raw=true)

Also haben wir die Adresse gesetzt:
```c
#define I2C_ADDRESS 0x3C 
```
Jetzt kann das Setup beginnen:
```c
void setup() {
Wire.begin();                                // Wire-Protokoll initialisieren (Beginn der Datenübertragung)
  oled.begin(&Adafruit128x64, I2C_ADDRESS);  // Display initialisieren
  oled.set400kHz();                          // Datenübertragungsrate setzen
  oled.setFont(font5x7);                     // Fontgröße setzen
  oled.clear();                              // Display leeren (sicherheitshalber)
  oled.set2X();                              // Größere Buchstaben
  oled.println("START");                     // START schreiben
  oled.set1X();                              // Buchstaben wieder auf Normalgröße
}
```
Das eigentliche Programm besteht dann nur darin, "Hallo!" in großen Buchstaben anzuzeigen:
```c
void loop() {
 oled.set2X();                              // Größere Buchstaben
  oled.println("Hallo!");                   // "Hallo!" schreiben
}
```
Und das hat funktioniert:
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/hallo!.jpeg?raw=true)

[Hier](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/OLEDtest/OLEDtest.ino) findet man unser ganzes Test-Programm.

## 11. September
Heute haben wir angefangen, Daten vom Höhenmesser auf dem Display anzuzeigen. Dafür haben wir die beiden Testprogramme erst einmal kombiniert übersetzt und etwas kommentiert:

[Kombiniertes Programm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/Kombi/Kombi.ino)

Das sieht dann so aus:
  
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/45DB887A-5F1C-4270-9310-210C16FA2DC9.jpeg?raw=true)


## 12. September
Heute haben wir zunächst die Fehlermeldungen getestet und dann Fehlercodes aufgestellt, da die angezeigten Fehlermeldungen zu lang für das Display waren. Dabei haben wir gleich die Beschreibung unseres Codes oben erweitert:
  
```c
/* Höhenmesser

Hardware:
---------
Board: WEMOS D1 mini pro V1.0
Altimeter: Bosch BMP180
Display: GM009605 OLED

Fehlercodes:
------------
1 - Fehler beim Erhalten des Drucks
2 - Fehler beim Starten der Druckmessung
3 - Fehler beim Erhalten der Temperatur
4 - Fehler beim Starten der Temperaturmessung
  
Basiert teilweise auf dem BMP180_altitude_example des Höhenmessers:
V10 Mike Grusin, SparkFun Electronics 10/24/2013
V1.1.2 Updates for Arduino 1.6.4 5/2015
*/
```
Danach haben wir angefangen, die maximale Höhe zu berechnen. Das tun wir wie beim alten Höhenmesser mit Hilfe zweier neuer Variablen:
```c
double highest;     // Max-Druck
double lowest;      // Min-Druck
```
Wenn der gemessene Höhenunterschied (Variable a) kleiner als der kleinste bzw. größer als der größte Wert ist, wird dieser Wert als höchster (highest) bzw. kleinster (lowest) Wert gesetzt:
```c
  if (a < lowest) lowest = a;

  if (a > highest) highest = a;
```
Den Minimumwert wollen wir nur aus Interesse haben, der Maximumwert soll jedoch angezeigt werden:
```c
oled.setCursor(0, 4);                  // Cursor unter die Höhenanzeige bewegen
  if (highest >= 0.0) oled.print(" "); // Leerzeichen, wenn positive Zahl
oled.print(highest);                   
oled.print("m");
```
Das ganze funktioniert sehr gut, sieht aber nicht gut aus. Deshalb haben wir angefangen, uns zu überlegen, wie ein besseres Layout aussehen könnte.

## 24. September
Heute haben wir für das neue Layout für die Temperatur-Anzeige angefangen. Dafür haben wir eine neue Variable deklariert:
```c
double T;           // Temperatur
```
In unseren ersten Versuchen wurde erstmal nur die Temperatur zum Start-Zeitpunkt angezeigt. Das wäre akzeptabel, aber nicht das, was wir eigentlich wollen.
Dann haben wir versucht mit Hilfe von Pointern die Temperatur aus der Definition von getPressure() zu verwenden, was elegant, aber für uns auch sehr zeitaufwändig war: Kurz vor Ende der Stunde waren wir fertig, das ganze hat jedoch nicht funktioniert hat, weil die Library dafür nicht ausgelegt ist.

## 25. September
Heute haben wir das Temperatur-Problem auf eine relativ einfache Weise gelöst. Statt T aus der Definition von getPressure() direkt zu verwenden, haben wir die Temperaturmessung aus getPressure() kopiert und vereinfacht. Zunächst haben wir die Variable status als char deklariert:
```c
char status;
```
Und dann die Temperaturmessung von unten kopiert und auf's nötigste reduziert:
```c
status = pressure.startTemperature();
delay(100);
status = pressure.getTemperature(T);
```
Die Temperatur wird jetzt provisorisch unter dem Maximum angezeigt:
```c
// baseline-Druck anzeigen
oled.setCursor(0, 5);          // Cursor unter die Maximumanzeige bewegen
    if (T >= 0.0) oled.print(" "); // Leerzeichen, wenn positive Zahl
    oled.print(T);
    oled.print(" C");
```

## 26. September
Heute haben wir das neue Layout gebaut. Der baseline-Druck steht weiterhin oben rechts, jetzt aber in einer Reihe und mit der Einheit mbar statt mb:
```c
oled.clear();
oled.print(baseline);
oled.print(" mbar");
```
Darunter kommen, immernoch im setup, die Teile der Anzeige, die sich nicht verändern - also "Hoehe:", "Max:" und Platz für sonstigen Text:
```c
// statische Teile der Höhenanzeigen
 oled.setCursor(0, 3);
 oled.print("Hoehe:");
 oled.setCursor(0,5);
 oled.print("Max:");

 // Sonstiger Text
 oled.setCursor(0, 7);
 oled.print("blablabla");
```

Die veränderlichen Teile der Anzeige stehen im Hauptteil des Codes:
```c
    // Höhenunterschied anzeigen
    oled.set2X();
    oled.setCursor(40, 2);               // Cursor bewegen
    if (a >= 0.0) oled.print(" ");       // Leerzeichen, wenn positive Zahl
    oled.print(a);
    oled.print("m");

    // Maximum anzeigen
    oled.setCursor(40, 4);               // Cursor unter die Höhenanzeige bewegen
    if (highest >= 0.0) oled.print(" "); // Leerzeichen, wenn positive Zahl
    oled.print(highest);
    oled.print("m");
    oled.set1X();

    // Temperaturanzeige
    oled.setCursor(80, 0);
    oled.print(T);
    oled.println(" C");
```
Die Anzeige sieht jetzt so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/neues%20layout2.jpg?raw=true)

Damit haben wir die ersten drei unserer Ziele erreicht.

## 1. Oktober
Heute haben wir mit der Erstellung des Webservers angefangen.
Zuerst mussten wir die Macadresse des Boards im seriellen Monitor anzeigen lassen, um es dann in iSurf freizuschalten ([Quelle](https://techtutorialsx.com/2017/04/09/esp8266-get-mac-address/)):
```c
#include <ESP8266WiFi.h>
 
void setup(){
 
   Serial.begin(115200);
   delay(500);
 
   Serial.println();
   Serial.print("MAC: ");
   Serial.println(WiFi.macAddress());
 
}
 
void loop(){}
```

Verbindung herstellen mit dem Sketch, der in der [Dokumentation der ESP8266WiFi-Library](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html) vorgeschlagen wird:
```c
#include <ESP8266WiFi.h>

void setup() {
  
  Serial.begin(115200);
  Serial.println();

  WiFi.begin("MyNetwork", "----------");  // aus Sicherheitsgründen zensiert

  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();

  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
}

void loop() {

}
```
Das ist natürlich nur eine temporäre Lösung das Board in einem existierenden Netzwerk anzusteuern. Zum einen kann man den Höhenmesser dann nur an Orten mit funktionierender WLAN-Verbindung nutzen (also nicht draußen auf dem Feld!), zum anderen ist das ESP2866-Modul nicht sehr sicher, bietet also ein potentielles Einfallstor an der Firewall vorbei. Das Ziel ist also, dass der Höhenmesser sein eigenes Netzwerk erstellt, mit dem sich der Client dann verbinden kann. Erstmal haben wir dann aber einen Webserver mit Hilfe des Beispiels der Library aufgesetzt:
```c
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>
#include <ESP8266mDNS.h>

#ifndef STASSID
#define STASSID "MyNetwork"
#define STAPSK  "------."
#endif

const char* ssid = STASSID;
const char* password = STAPSK;

ESP8266WebServer server(80);

const int led = 13;

void handleRoot() {
  digitalWrite(led, 1);
  server.send(200, "text/plain", "Hoehenmesser");
  digitalWrite(led, 0);
}

void handleNotFound() {
  digitalWrite(led, 1);
  String message = "File Not Found\n\n";
  message += "URI: ";
  message += server.uri();
  message += "\nMethod: ";
  message += (server.method() == HTTP_GET) ? "GET" : "POST";
  message += "\nArguments: ";
  message += server.args();
  message += "\n";
  for (uint8_t i = 0; i < server.args(); i++) {
    message += " " + server.argName(i) + ": " + server.arg(i) + "\n";
  }
  server.send(404, "text/plain", message);
  digitalWrite(led, 0);
}

void setup(void) {
  pinMode(led, OUTPUT);
  digitalWrite(led, 0);
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.println("");

  // Wait for connection
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Connected to ");
  Serial.println(ssid);
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());

  if (MDNS.begin("esp8266")) {
    Serial.println("MDNS responder started");
  }

  server.on("/", handleRoot);

  server.on("/inline", []() {
    server.send(200, "text/plain", "this works as well");
  });

  server.onNotFound(handleNotFound);

  server.begin();
  Serial.println("HTTP server started");
}

void loop(void) {
  server.handleClient();
  MDNS.update();
}
```
Das Ergebnis sieht dann so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/WebServer1.png?raw=true)

Diesen einfachen Webserver haben wir dann in den Code integriert und getestet, ob Kompatibilitätsprobleme auftreten. Das war nicht der Fall. 

## 2. Oktober
Heute haben wir den Code aus dem Webserver-Beispiel auf das nötigste reduziert. Das Board erstellt jetzt ebenfals sein eigenes Netzwerk und ist nicht mehr auf ein bestehendes Netzwerk angewiesen. Außerdem haben wir ein Darstellungsproblem der IP-Adresse auf dem OLED-Display behoben.

## FERIEN
Webseite mit template
Webserver: 
Variablen über knöpfe ändern
Dateien speichern https://42project.net/esp8266-flash-dateisystem-spiffs-beispielhaft-benutzen/
