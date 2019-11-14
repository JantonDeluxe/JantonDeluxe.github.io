---
layout: page
title: Unterrichtstagebuch
subtitle: Was wir an welchem Tag gemacht haben
---

### Inhaltsverzeichnis
>* [13. August](#1)
>* [14. August](#2)
>* [15. August](#3)
>* [20. August](#4)
>* [21. August](#5)
>* [22. August](#6)
>* [27. August](#7)
>* [28. August](#8)
>* [29. August](#9)
>* [11. September](#10)
>* [12. September](#11)
>* [24. September](#12)
>* [25. September](#13)
>* [26. September](#14)
>* [1. Oktober](#15)
>* [2. Oktober](#16)
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


## 13. August <a name="1"></a>
In unserer ersten Informatikstunde haben wir uns überlegt, was für Projekte wir gerne machen würden und auf [code.org](https://code.org/) mit einem Einsteiger-Kurs angefangen.
Unsere Vorkenntnisse sind auf einfache Programmieraufgaben mit [Scratch](https://de.wikipedia.org/wiki/Scratch_(Programmiersprache)) im Rahmen des Jugendwettbewerbs Informatik und Grundlagen der Arduino-Programmierung beschränkt.

## 14. August <a name="2"></a>
Heute haben wir einen [GitHub-Account](https://github.com/JantonDeluxe), ein [Repository](https://github.com/JantonDeluxe/luft-waffle) und unsere [Website](https://jantondeluxe.github.io/luft-waffle/), die das README anzeigt, erstellt.
Für die Website benutzen wir vorläufig das [Jekyll](https://jekyllrb.com/)-Theme Minimal, sind aber noch auf der Suche nach einem besseren Theme.

## 15. August <a name="3"></a>
Heute haben wir uns für ein Arduino-Projekt entschieden und dafür angefangen, uns mit dem [Arduino](https://arduino.cc) und der Programmiersprache [C++](https://www.w3schools.com/cpp/) auseinanderzusetzten. Ebenfalls haben wir erste Links in unsere Website eingefügt und weiterüberlegt, wie wir am besten lernen und was für ein Projekt genau wir umsetzen wollen.

## 20. August <a name="4"></a>
Heute haben wir uns dafür entschieden, einen Höhenmesser für Wasserraketen zu bauen. Dafür haben wir Ziele aufgestellt (siehe [Projektbeschreibung](https://jantondeluxe.github.io/2019-10-06-projektbbeschreibung/#ziel)). Dazu haben wir uns mit Pseudo-Code überlegt, wie wir diese Ziele grob umsetzen könnten.

## 21. August <a name="5"></a>
Zuhause haben wir die Aufgaben 1 bis 4 von StarlogoTNG erfolgreich abgeschlossen. 

In der Stunde haben wir angefangen den Laptop für unser Projekt einzurichten:
- Laptop in iSurf registriert
- [Arduino IDE](https://www.arduino.cc/en/Guide/Windows) installiert
- [D1 mini Pro Hardware Package](https://github.com/esp8266/Arduino) über die Arduino IDE installiert 
  (_Voreinstellungen_ -> _zusätzliche Boardverwalter-URLs_ -> Link einfügen)
- [D1 mini Pro-Treiber](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) installiert
- [BMP180-Library](https://github.com/LowPowerLab/SFE_BMP180) installiert [(Tutorial)](https://learn.adafruit.com/adafruit-all-about-arduino-libraries-install-use/installing-a-library)

## 22. August <a name="6"></a>
Heute haben wir den Höhenmsser (ein [Bosch BMP 180-Sensor](https://www.sparkfun.com/products/retired/11824)) mit Hilfe des [Sparkfun-Tutorials](https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all) mit dem Mikrocontroller auf einem Breadboard verkabelt.

Dann wollten wir zum Testen das Example-Sketch auf den Mikrocontroller laden, was leider nicht funktioniert hat:
```
esptool.FatalError: Timed out waiting for packet headed
```
Deshalb haben wir statt der neuen Universal Windows Platform App aus dem Microsoft Store die Arduino IDE diesmal als normales Windows-Programm (über den .exe Installer) installiert, was das Problem aber nicht behoben hat. Sicherheitshalber haben wir deshalb den D1-Treiber ebenfalls neu installiert und weiter nach dem Fehler gesucht.

## 27. August <a name="7"></a>
Heute haben wir endlich den Fehler gefunden, warum unser Board nicht mit der Arduino IDE kommunizieren konnte: Wir hatten vergessen, das Board engültig zu installieren. Das geht in der Arduino IDE unter

_Werkzeuge_ -> _Board_ -> _Boardverwalter_ -> Suche “ESP8266” (der ESP8266 ist der auf unserem Board verbaute Mikrocontroller) -> _Installieren_. 

Zusätzlich muss man für unser spezielles Board weitere Zusatzeinstellungen in den prefrences.txt hinzufügen, wofür es glücklicherweise eine [Vorlage](https://arduino-projekte.info/wp-content/uploads/2017/07/boards.txt) gibt. Dann muss man die Arduino IDE nur noch neustarten und unter 

_Werkzeuge_ -> _Board_ -> "LOLIN(WEMOS) D1 mini Pro" auswählen.

Sehr geholfen haben uns bei der Installation diesese beiden Tutorials: 
- https://arduino-projekte.info/installation-eps8266-modul-wie-z-b-wemos/
- https://arduino-projekte.info/installation-wemos-d1-mini-lite-wemos-d1-mini-pro/

Wenn wir diesen Schritt nicht vergessen hätten, hätte das Hochladen des Programms trotzdem nicht funktioniert, da auf der Hersteller-Website des D1 eine veraltete Boardverwalter-URL angegeben war, was uns nur durch Zufall aufgefallen ist. 

## 28. August <a name="8"></a>
Heute konnten wir testen, ob der Sensor richtig verkabelt ist. Dafür haben wir das oben erwähnte Example Sketch auf den D1 hochgeladen und die Daten ausgelesen. Das funktioniert so:

In Text-Form: _Werkzeuge_ -> _Serieller Monitor_

Als Graph: _Werkzeuge_ -> _Serieller Plotter_

Die Verkabelung war korrekt. Mit dem Example Sketch haben wir dann den Zeitraum gemessen, in dem die Messwerte eine Ungenauigkeit von 2 Metern übersteigen. Der Zeitraum beträgt 18 Minuten. Um sicher zugehen, dass die Abweichung nicht zu groß wird, sollte also etwa alle 10 Minuten eine Rekalibrierung vorgenommen werden, um einen neuen Nullwert zu errechnen.
 
Aus Spaß haben wir dann noch das zweite [Example Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/SFE_BMP180_example/SFE_BMP180_example.ino), das eigentlich für Wettermessungen gedacht ist ausprobiert.
Dabei haben wir die vorgegebene Höhe (Boulder, Colorado: 1655 Meter) auf 0 Meter gesetzt, um die gesuchte relative Höhe zu erhalten.
Danach wurde uns allerdings erstmal trotz Bewegung dauerhaft die Höhe 0 angezeigt. Letzlich lag das daran, dass die Höhe nur in ganzen Zahlen ausgegeben wurde und die Bewegungen kleiner als 1 Meter waren, weshalb keine neue Höhe angezeigt wurde.

## 29. August <a name="9"></a>
Heute haben wir das OLED-Display verkabelt und ein [Testprogramm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/OLEDtest/OLEDtest.ino) geschrieben.

Und das hat funktioniert:
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/hallo!.jpeg?raw=true)

## 11. September <a name="10"></a>
Heute haben wir angefangen, Daten vom Höhenmesser auf dem Display anzuzeigen. Dafür haben wir die beiden Testprogramme erst einmal kombiniert übersetzt und etwas kommentiert:

[Kombiniertes Programm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/Kombi/Kombi.ino)

Das sieht dann so aus:
  
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/45DB887A-5F1C-4270-9310-210C16FA2DC9.jpeg?raw=true)


## 12. September <a name="11"></a>
Heute haben wir zunächst die Fehlermeldungen getestet und dann Fehlercodes aufgestellt, da die angezeigten Fehlermeldungen zu lang für das Display waren. Dabei haben wir gleich die Beschreibung unseres Codes oben erweitert:
  
Anmerkung: Letztendlich lassen wir die Fehlermeldungen über die serielle Schnittstelle ausgeben. Da spielt die Länge keine Rolle mehr, weshalb wir die Fehlercodes gelöscht haben.

Danach haben wir angefangen, die maximale Höhe zu berechnen und anzuzeigen. Das ganze funktioniert sehr gut, sieht aber nicht so aus. Deshalb haben wir angefangen, uns zu überlegen, wie ein besseres Layout aussehen könnte.

## 24. September <a name="12"></a>
Heute haben wir für das neue Layout für die Temperatur-Anzeige angefangen. 
In unseren ersten Versuchen wurde erstmal nur die Temperatur zum Start-Zeitpunkt angezeigt. Das wäre akzeptabel, aber nicht das, was wir eigentlich wollen.
Dann haben wir versucht mit Hilfe von Pointern die Temperatur aus der Definition von getPressure() zu verwenden, was elegant, aber für uns auch sehr zeitaufwändig war: Kurz vor Ende der Stunde waren wir fertig, das ganze hat jedoch nicht funktioniert hat, weil die Library dafür nicht ausgelegt ist.

## 25. September <a name="13"></a>
Heute haben wir das Temperatur-Problem auf eine relativ einfache Weise gelöst. Statt `T` aus der Definition von `getPressure()` direkt zu verwenden, haben wir die Temperaturmessung aus `getPressure()` kopiert und vereinfacht. Die Temperatur wird jetzt provisorisch unter dem Maximum angezeigt.

## 26. September <a name="14"></a>
Heute haben wir das neue Layout gebaut. Der baseline-Druck steht weiterhin oben rechts, jetzt aber in einer Reihe und mit der Einheit mbar statt mb. Darunter kommen, immernoch im setup, die Teile der Anzeige, die sich nicht verändern - also "Hoehe:", "Max:" und Platz für sonstigen Text. Die veränderlichen Teile der Anzeige stehen im Hauptteil des Codes.

Die Anzeige sieht jetzt so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/neues%20layout2.jpg?raw=true)

## 1. Oktober <a name="15"></a>
Heute haben wir mit der Erstellung des Webservers angefangen.
Zuerst mussten wir die Macadresse des Boards im seriellen Monitor anzeigen lassen, um es dann in iSurf freizuschalten ([Quelle](https://techtutorialsx.com/2017/04/09/esp8266-get-mac-address/)):

Dann haben wir eine erste Verbindung mit dem Sketch, der in der [Dokumentation der ESP8266WiFi-Library](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html) vorgeschlagen wird hergestellt.

Das ist natürlich nur eine temporäre Lösung das Board in einem existierenden Netzwerk anzusteuern. Zum einen kann man den Höhenmesser dann nur an Orten mit funktionierender WLAN-Verbindung nutzen (also nicht draußen auf dem Feld!), zum anderen ist das ESP2866-Modul nicht sehr sicher, bietet also ein potentielles Einfallstor an der Firewall vorbei. Das Ziel ist also, dass der Höhenmesser sein eigenes Netzwerk erstellt, mit dem sich der Client dann verbinden kann. Erstmal haben wir dann aber einen Webserver mit Hilfe des Beispiels der Library aufgesetzt.

Das Ergebnis sieht dann so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/WebServer1.png?raw=true)

Diesen einfachen Webserver haben wir dann in den Code integriert und getestet, ob Kompatibilitätsprobleme auftreten. Das war nicht der Fall. 

## 2. Oktober <a name="16"></a>
Heute haben wir den Code aus dem Webserver-Beispiel auf das nötigste reduziert. Das Board kann jetzt ebenfals sein eigenes Netzwerk erstellen und ist nicht mehr auf ein bestehendes Netzwerk angewiesen. Außerdem haben wir ein Darstellungsproblem der IP-Adresse auf dem OLED-Display behoben.

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
