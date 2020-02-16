---
layout: page
title: Unterrichtstagebuch 2. Halbjahr
subtitle: Unterrichtstagebuch 2: Angriff der Programmierer
---

### Inhaltsverzeichnis
>* [5. Dezember](#1)
>* [10. Dezember](#2)
>* [11. Dezember](#3)
>* [12. Dezember](#4)
>* [18. Dezember](#5)
>* [19. Dezember](#6)
>* [Ferien](#7)
>* [15. Januar](#8)
>* [16. Januar](#9)
>* [Daten anpassen bis 31.1](#10)
>* [22. Januar](#11)
>* [23. Januar](#12)
>* [28. Januar](#13)
>* [4. Februar](#14)
>* [5. Februar](#15)
>* [6. Februar](#16)
>* [Ferien](#17)
>* [22. Oktober](#18)
>* [23. Oktober](#19)
>* [24. Oktober](#20)
>* [29. Oktober](#21)
>* [5. November](#22)
>* [6. November](#23)
>* [7. November](#24)
>* [12. November](#25)
>* [14. November](#26)


## 5. Dezember <a name="1"></a>
Heute haben wir die Geschwindigkeits- und Beschleunigungsberechnung im Arduino-Sketch hinzugefügt und zur Darstellung in Chart.js hinzugefügt. Dabei haben wir ebenfalls die Update-Frequenz erhöht (jetzt ~100ms) und die Glättung des Graphen reduiziert, damit die Darstellung 

## 10. Dezember <a name="2"></a>
Heute haben wir versucht, die Netzwerkverbindungen des Boards zu harmonisieren. Ziel ist es, Verbindungen nicht immer manuell eingeben zu müssen. Dabei gilt zu beachten, dass die [Wifi-Status-Codes](https://www.arduino.cc/en/Reference/WiFiStatus) sich bei [ESP8266-Chips](https://github.com/esp8266/Arduino/blob/4897e0006b5b0123a2fa31f67b14a3fff65ce561/libraries/ESP8266WiFi/src/include/wl_definitions.h) von denen normaler Arduinos unterscheiden.Da diese Netzwerkeinstellungen in einem gesonderten Flashbereich gespiechert werden, haben wir mithilfe des esptools den Flashspeicher formatiert. Dabei muss irgendetwas schief gelaufen sein. Vermutlich haben wir das Board gebrickt.

## 11. Dezember <a name="3"></a>
Heute haben wir versucht, das Board zu reparieren. Nach einer Reinstallation der Treiber etc. konnten wir die Ursache auf einen fehlerhaften Bootloader zurückführen. Um dies zu beheben, haben wir zuhause mehrere Stunden versucht, eine neue Firmware auf das Board zu flashen. Dafür muss man den Pin D3 (GPIO0) erden (also mit Ground/GND verbinden). Dann kann man den Strom anschließen. Das Blinken der OnBoard-LED signalisiert, die Flash-Bereitschaft des Boards. Zum Flashen haben wir das Tool [ESPeasy](https://github.com/letscontrolit/ESPEasy) verwendet. Nun läuft der Upload auf das Board wieder.

## 12. Dezember <a name="4"></a>
Heute haben wir die Libraries usw. neuinstalliert und die WLAN-Verbindung wieder auf seine alte Weise hergestellt. Der einzige jetzt noch auftretende Fehler liegt bei der Übertragung des index-Headers.

## 18. Dezember <a name="5"></a>
Heute haben wir mit der neuen Version des Höhenmessers begonnen. Wir haben eine neue, bessere Display-Library getestet und angefangen, die Anzeige darauf umzustellen.

## 19. Dezember <a name="6"></a>
Heute haben wir die Anzeige zu Ende geändert und angefangen, das Data-Logger-Shield einzurichten.

## Ferien <a name="7"></a>
englische Variablen, Splashscreen https://cdn-learn.adafruit.com/downloads/pdf/adafruit-gfx-graphics-library.pdf, http://javl.github.io/image2cpp/, Temparaturannzeigefehler behoben (- verschiebung entfernt)

## 15. Januar <a name="8"></a>
Heute haben wir angefangen, die SD-Karte einzzubinden und uns überlegt, wie wir Messreihen darauf spreichern, starten und stoppen wollen.

## 16. Januar<a name="9"></a>
Heute haben wir Stoppmechanismen getestet. Ergebnis: die Kopplung des Stoppmechanismus an die Geschwindigkeit (Vorzeichenwechsel bei Überschreiten des Scheitelpunkts der Flugbahn) funktioniert nicht, da die Messabweichung zu groß ist.
Des weiteren haben wir versucht die SD-Karte zum Laufen zu bringen, was jedoch nicht funktioniert hat. Wir vermuten, dass für den Betrieb des ganzen Shields die Batterie benötigt wird.

## Daten anpassen bis 31.1. <a name="10"></a>
Website überarbeitet: Navbar, Start-/Stopp-Knopf, About-Page, Start-Page, manuelle Kalibrierung, Übertragung von maximaler Höhe, Temperatur und Timer (Anzeige außerhalb des Graphen), Geschwindigkeit und Beschleunigung im Diagramm standardmäßig ausgeblentet (Übersicht)
Timer eingebaut, der nach Aktivierung runterzählt und die Messung stoppt.

## 22. Januar <a name="11"></a>
Heute haben wir unseren Webserver repariert. Das Abrufen des headers funktioniert wieder. Die Fehlerquelle war der Variablenmodifikator PROGMEM, der nach dem Flashen der neuen Firmware nicht mehr funktioniert. Eigentlich werden so Teile des Codes im Flashspeicher gespeichert, was den Arbeitsspeicher entlastet. Da der D1 mini Pro über vergleichsweise viel Arbeitsspeicher verfügt, funktioniert es auch ohne das Speichern im Flashspeicher.

## 23. Januar <a name="12"></a>

## 28. Januar <a name="13"></a>
Heute war Anton krank, weshalb ich mir Möglichkeiten angeguckt habe, eine animierte Bitmap für das Display beim Starten des Arduinos zu erstellen, allerdings würden vielen der Möglichkeiten den 16Mb Arbeitsspeicher des Arduinos überlasten.

## 4. Februar <a name="14"></a>
Heute haben wir das neue Layout gebaut. Der baseline-Druck steht weiterhin oben rechts, jetzt aber in einer Reihe und mit der Einheit mbar statt mb. Darunter kommen, immernoch im setup, die Teile der Anzeige, die sich nicht verändern - also "Hoehe:", "Max:" und Platz für sonstigen Text. Die veränderlichen Teile der Anzeige stehen im Hauptteil des Codes.

Die Anzeige sieht jetzt so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/neues%20layout2.jpg?raw=true)

## 5. Februar <a name="15"></a>
Heute haben wir mit der Erstellung des Webservers angefangen.
Zuerst mussten wir die Macadresse des Boards im seriellen Monitor anzeigen lassen, um es dann in iSurf freizuschalten ([Quelle](https://techtutorialsx.com/2017/04/09/esp8266-get-mac-address/)):

Dann haben wir eine erste Verbindung mit dem Sketch, der in der [Dokumentation der ESP8266WiFi-Library](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html) vorgeschlagen wird hergestellt.

Das ist natürlich nur eine temporäre Lösung das Board in einem existierenden Netzwerk anzusteuern. Zum einen kann man den Höhenmesser dann nur an Orten mit funktionierender WLAN-Verbindung nutzen (also nicht draußen auf dem Feld!), zum anderen ist das ESP2866-Modul nicht sehr sicher, bietet also ein potentielles Einfallstor an der Firewall vorbei. Das Ziel ist also, dass der Höhenmesser sein eigenes Netzwerk erstellt, mit dem sich der Client dann verbinden kann. Erstmal haben wir dann aber einen Webserver mit Hilfe des Beispiels der Library aufgesetzt.

Das Ergebnis sieht dann so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/WebServer1.png?raw=true)

Diesen einfachen Webserver haben wir dann in den Code integriert und getestet, ob Kompatibilitätsprobleme auftreten. Das war nicht der Fall. 

## 6. Februar <a name="16"></a>


## Ferien <a name="17"></a>
In den Herbstferien haben wir eine Webseite für das Projekt erstellt. Wir verwenden jetzt das GitHub-Template [beautiful jekyll](https://deanattali.com/beautiful-jekyll/) von Dean Attali. Die "Installation" wird in der [Read-Me-Datei](https://github.com/daattali/beautiful-jekyll/blob/master/README.md) des dazugehörigen GitHub-Repositorys beschrieben.

Ebenfalls haben wir weiter am Webserver herumgebastelt. So haben wir z.B. Variablen über Buttons auf einer Website geändert und Messwerte mit Hilfe von Poitern als Text ausgegeben.
Webserver: 
Variablen über knöpfe ändern
Dateien speichern https://42project.net/esp8266-flash-dateisystem-spiffs-beispielhaft-benutzen/
Realtime Graphics https://circuits4you.com/2019/01/11/esp8266-data-logging-with-real-time-graphs/
Variablen übergeben mit Pointern: https://www.w3schools.com/cpp/cpp_pointers.asp

## 22.10.2019 <a name="18"></a>
Heute haben wir den Baseline-Wert als Durchschnitt von 100 Messungen definiert. Damit sollte sich die Genauigkeit der Messungen erhöhen. Quelle: https://www.arduinoforum.de/arduino-Thread-Gewichteten-Durchschnitt-berechnen-20-Werte-in-fortlaufender-Variable-speichern
Zusammen mit Nick haben wir angefangen Probleme beim Anpassen des HTML-/Java Skript-Codes an unseren vorhandenen Code zu beheben.

## 23.10.2019 <a name="19"></a>
Heute sind wir mit der Anpassung fertig geworden und haben den Höhenmesser getestet. Dafür sind wir die Treppen hoch und runtergelaufen und haben die messwerte kontrolliert. 

## 24.10.2019 <a name="20"></a>
Heute haben wir den Code vereinfacht. Die Kombination der Strings in `handleData` haben wir z.B. von 8 auf 4 Zeilen vereinfacht und mehrfach vorkommende Variablen mit Hilfe von Pointern zu globalen Variablen zusammengefasst.

## 29.10.2019 <a name="21"></a>
Heute haben wir das SPIFFS-Dateisystem auf dem Arduino eingerichtet und das Java Script eingebaut, so dass es jetzt auch offline abgerufen werden kann. Dazu haben wir an der Projektbeschreibung und dem Projekttagebuch weitergeschrieben.

## 5.11.2019 <a name="22"></a>
Heute haben wir die Ladegeschwindigkeit des Java Scripts erhöht, indem wir eine andere Speicher- und Abrufmethode verwendet haben.

## 6.11.2019 <a name="23"></a>
Heute haben wir eine Übersicht zu den Komponenten und Anschlüssen des D1 mini Pro erstellt und an der Projektbeschreibung weitergeschrieben.

## 7.11.2019 <a name="24"></a>
Heute haben wir mit [Fritzing](https://fritzing.org/home/) ein Diagramm der Verkabelungen erstellt und an der Projektbeschreibung weitergeschrieben.

## 12.11.2019 <a name="25"></a>
Heute haben wir die Projektbeschreibung weitergeschreieben.

## 14.11.2019 <a name="26"></a>
Heute haben wir die Projektbeschreibung weitergeschreieben.
