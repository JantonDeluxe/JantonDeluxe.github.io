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
Unsere Vorkenntnisse sind auf einfache Programmieraufgaben mit [Scratch](https://de.wikipedia.org/wiki/Scratch_(Programmiersprache)) im Rahmen des Jugendwettbewerbs Informatik und Grundlagen der Arduino-Programmierung beschränkt.

## 14. August
Heute haben wir einen [GitHub-Account](https://github.com/JantonDeluxe), ein [Repository](https://github.com/JantonDeluxe/luft-waffle) und unsere [Website](https://jantondeluxe.github.io/luft-waffle/), die das README anzeigt, erstellt.
Für die Website benutzen wir vorläufig das [Jekyll](https://jekyllrb.com/)-Theme Minimal, sind aber noch auf der Suche nach einem besseren Theme.

## 15. August
Heute haben wir uns für ein Arduino-Projekt entschieden und dafür angefangen, uns mit dem [Arduino](https://arduino.cc) und der Programmiersprache [C++](https://www.w3schools.com/cpp/) auseinanderzusetzten. Ebenfalls haben wir erste Links in unsere Website eingefügt und weiterüberlegt, wie wir am besten lernen und was für ein Projekt genau wir umsetzen wollen.

## 20. August
Heute haben wir uns dafür entschieden, einen Höhenmesser für Wasserraketen zu bauen. Dafür haben wir Ziele aufgestellt (siehe [Projektbeschreibung](https://jantondeluxe.github.io/2019-10-06-projektbbeschreibung/#ziel)). Dazu haben wir uns mit Pseudo-Code überlegt, wie wir diese Ziele grob umsetzen könnten.

## 21. August
Zuhause haben wir die Aufgaben 1 bis 4 von StarlogoTNG erfolgreich abgeschlossen. 

In der Stunde haben wir angefangen den Laptop für unser Projekt einzurichten:
- Laptop in iSurf registriert
- [Arduino IDE](https://www.arduino.cc/en/Guide/Windows) installiert
- [D1 mini Pro Hardware Package](https://github.com/esp8266/Arduino) über die Arduino IDE installiert 
  (_Voreinstellungen_ -> _zusätzliche Boardverwalter-URLs_ -> Link einfügen)
- [D1 mini Pro-Treiber](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) installiert
- [BMP180-Library](https://github.com/LowPowerLab/SFE_BMP180) installiert [(Tutorial)](https://learn.adafruit.com/adafruit-all-about-arduino-libraries-install-use/installing-a-library)

## 22. August
Heute haben wir den Höhenmsser (ein [Bosch BMP 180-Sensor](https://www.sparkfun.com/products/retired/11824)) mit Hilfe des [Sparkfun-Tutorials](https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all) mit dem Mikrocontroller auf einem Breadboard verkabelt.

Dann wollten wir zum Testen das Example-Sketch auf den Mikrocontroller laden, was leider nicht funktioniert hat:
```
esptool.FatalError: Timed out waiting for packet headed
```
Deshalb haben wir statt der neuen Universal Windows Platform App aus dem Microsoft Store die Arduino IDE diesmal als normales Windows-Programm (über den .exe Installer) installiert, was das Problem aber nicht behoben hat. Sicherheitshalber haben wir deshalb den D1-Treiber ebenfalls neu installiert und weiter nach dem Fehler gesucht.

## 27. August
Heute haben wir endlich den Fehler gefunden, warum unser Board nicht mit der Arduino IDE kommunizieren konnte: Wir hatten vergessen, das Board engültig zu installieren. Das geht in der Arduino IDE unter

_Werkzeuge_ -> _Board_ -> _Boardverwalter_ -> Suche “ESP8266” (der ESP8266 ist der auf unserem Board verbaute Mikrocontroller) -> _Installieren_. 

Zusätzlich muss man für unser spezielles Board weitere Zusatzeinstellungen in den prefrences.txt hinzufügen, wofür es glücklicherweise eine [Vorlage](https://arduino-projekte.info/wp-content/uploads/2017/07/boards.txt) gibt. Dann muss man die Arduino IDE nur noch neustarten und unter 

_Werkzeuge_ -> _Board_ -> "LOLIN(WEMOS) D1 mini Pro" auswählen.

Sehr geholfen haben uns bei der Installation diesese beiden Tutorials: 
- https://arduino-projekte.info/installation-eps8266-modul-wie-z-b-wemos/
- https://arduino-projekte.info/installation-wemos-d1-mini-lite-wemos-d1-mini-pro/

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
Heute haben wir das OLED-Display verkabelt und ein [Testprogramm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/OLEDtest/OLEDtest.ino) geschrieben.

Und das hat funktioniert:
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/hallo!.jpeg?raw=true)

## 11. September
Heute haben wir angefangen, Daten vom Höhenmesser auf dem Display anzuzeigen. Dafür haben wir die beiden Testprogramme erst einmal kombiniert übersetzt und etwas kommentiert:

[Kombiniertes Programm](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/Kombi/Kombi.ino)

Das sieht dann so aus:
  
![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/45DB887A-5F1C-4270-9310-210C16FA2DC9.jpeg?raw=true)


## 12. September
Heute haben wir zunächst die Fehlermeldungen getestet und dann Fehlercodes aufgestellt, da die angezeigten Fehlermeldungen zu lang für das Display waren. Dabei haben wir gleich die Beschreibung unseres Codes oben erweitert:
  
Anmerkung: Letztendlich lassen wir die Fehlermeldungen über die serielle Schnittstelle ausgeben. Da spielt die Länge keine Rolle mehr, weshalb wir die Fehlercodes gelöscht haben.

Danach haben wir angefangen, die maximale Höhe zu berechnen und anzuzeigen. Das ganze funktioniert sehr gut, sieht aber nicht so aus. Deshalb haben wir angefangen, uns zu überlegen, wie ein besseres Layout aussehen könnte.

## 24. September
Heute haben wir für das neue Layout für die Temperatur-Anzeige angefangen. 
In unseren ersten Versuchen wurde erstmal nur die Temperatur zum Start-Zeitpunkt angezeigt. Das wäre akzeptabel, aber nicht das, was wir eigentlich wollen.
Dann haben wir versucht mit Hilfe von Pointern die Temperatur aus der Definition von getPressure() zu verwenden, was elegant, aber für uns auch sehr zeitaufwändig war: Kurz vor Ende der Stunde waren wir fertig, das ganze hat jedoch nicht funktioniert hat, weil die Library dafür nicht ausgelegt ist.

## 25. September
Heute haben wir das Temperatur-Problem auf eine relativ einfache Weise gelöst. Statt T aus der Definition von getPressure() direkt zu verwenden, haben wir die Temperaturmessung aus getPressure() kopiert und vereinfacht. Die Temperatur wird jetzt provisorisch unter dem Maximum angezeigt.

## 26. September
Heute haben wir das neue Layout gebaut. Der baseline-Druck steht weiterhin oben rechts, jetzt aber in einer Reihe und mit der Einheit mbar statt mb. Darunter kommen, immernoch im setup, die Teile der Anzeige, die sich nicht verändern - also "Hoehe:", "Max:" und Platz für sonstigen Text. Die veränderlichen Teile der Anzeige stehen im Hauptteil des Codes.

Die Anzeige sieht jetzt so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/neues%20layout2.jpg?raw=true)

## 1. Oktober
Heute haben wir mit der Erstellung des Webservers angefangen.
Zuerst mussten wir die Macadresse des Boards im seriellen Monitor anzeigen lassen, um es dann in iSurf freizuschalten ([Quelle](https://techtutorialsx.com/2017/04/09/esp8266-get-mac-address/)):

Dann haben wir eine erste Verbindung mit dem Sketch, der in der [Dokumentation der ESP8266WiFi-Library](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html) vorgeschlagen wird hergestellt.

Das ist natürlich nur eine temporäre Lösung das Board in einem existierenden Netzwerk anzusteuern. Zum einen kann man den Höhenmesser dann nur an Orten mit funktionierender WLAN-Verbindung nutzen (also nicht draußen auf dem Feld!), zum anderen ist das ESP2866-Modul nicht sehr sicher, bietet also ein potentielles Einfallstor an der Firewall vorbei. Das Ziel ist also, dass der Höhenmesser sein eigenes Netzwerk erstellt, mit dem sich der Client dann verbinden kann. Erstmal haben wir dann aber einen Webserver mit Hilfe des Beispiels der Library aufgesetzt.

Das Ergebnis sieht dann so aus:

![alt text](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/WebServer1.png?raw=true)

Diesen einfachen Webserver haben wir dann in den Code integriert und getestet, ob Kompatibilitätsprobleme auftreten. Das war nicht der Fall. 

## 2. Oktober
Heute haben wir den Code aus dem Webserver-Beispiel auf das nötigste reduziert. Das Board kann jetzt ebenfals sein eigenes Netzwerk erstellen und ist nicht mehr auf ein bestehendes Netzwerk angewiesen. Außerdem haben wir ein Darstellungsproblem der IP-Adresse auf dem OLED-Display behoben.

## Ferien
In den Herbstferien haben wir eine Webseite für das Projekt erstellt. Wir verwenden jetzt das GitHub-Template [beautiful jekyll](https://deanattali.com/beautiful-jekyll/) von Dean Attali. Die "Installation" wird in der [Read-Me-Datei](https://github.com/daattali/beautiful-jekyll/blob/master/README.md) des dazugehörigen GitHub-Repositorys beschrieben.

Ebenfalls haben wir weiter am Webserver herumgebastelt. So haben wir z.B. Variablen über Buttons auf einer Website geändert und Messwerte mit Hilfe von Poitern als Text ausgegeben.
Webserver: 
Variablen über knöpfe ändern
Dateien speichern https://42project.net/esp8266-flash-dateisystem-spiffs-beispielhaft-benutzen/
Realtime Graphics https://circuits4you.com/2019/01/11/esp8266-data-logging-with-real-time-graphs/
Variablen übergeben mit Pointern: https://www.w3schools.com/cpp/cpp_pointers.asp

## 22.10.2019 
Heute haben wir den Baseline-Wert als Durchschnitt von 100 Messungen definiert. Damit sollte sich die Genauigkeit der Messungen erhöhen. Quelle: https://www.arduinoforum.de/arduino-Thread-Gewichteten-Durchschnitt-berechnen-20-Werte-in-fortlaufender-Variable-speichern
Zusammen mit Nick haben wir angefangen Probleme beim Anpassen des HTML-/Java Skript-Codes an unseren vorhandenen Code zu beheben.

## 23.10.2019
Heute sind wir mit der Anpassung fertig geworden und haben den Höhenmesser getestet. Dafür sind wir die Treppen hoch und runtergelaufen und haben die messwerte kontrolliert. 

## 24.10.2019
Heute haben wir den Code vereinfacht. Die Kombination der Strings in `handleData` haben wir z.B. von 8 auf 4 Zeilen vereinfacht und mehrfach vorkommende Variablen mit Hilfe von Pointern zu globalen Variablen zusammengefasst.

## 29.10.2019
Heute haben wir an der Projektbeschreibung und dem Projekttagebuch weitergeschrieben.

## 5.11.2019
Heute haben wir eine Übersicht zu den Komponenten und Anschlüssen des D1 mini Pro erstellt und an der Projektbeschreibung weitergeschrieben.

## 6.11.2019
Heute haben wir mit [Fritzing](https://fritzing.org/home/) ein Diagramm der Verkabelungen erstellt und an der Projektbeschreibung weitergeschrieben.
