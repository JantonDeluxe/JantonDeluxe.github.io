---
layout: page
title: Unterrichtstagebuch
subtitle: Was wir an welchem Tag gemacht haben
---

### Inhaltsverzeichnis
>* [5. Dezember](#1)
>* [10. Dezember](#2)
>* [11. Dezember](#3)
>* [12. Dezember](#4)
>* [18. Dezember](#5)
>* [19. Dezember](#6)
>* [Ferien](#7)
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
Das Abrufen des headers funktioniert wieder. Fehlerquelle war der Variablenmodifikator PROGMEM, der nach dem Flashen der neuen Firmware nicht mehr funktioniert. Eigentlich werden so Teile des Codes im Flashspeicher gespeichert, was den Arbeitsspeicher entlastet.
Website überarbeitet: Navbar, Start-/Stopp-Knopf, About-Page, Start-Page, manuelle Kalibrierung, Übertragung von maximaler Höhe, Temperatur und Timer (Anzeige außerhalb des Graphen), Geschwindigkeit und Beschleunigung im Diagramm standardmäßig ausgeblentet (Übersicht)
Timer eingebaut, der nach Aktivierung runterzählt und die Messung stoppt.

## 22. Januar <a name="11"></a>


## 23. Januar <a name="12"></a>

## 28. Januar <a name="13"></a>
Heute war Anton krank, weshalb ich mir möglichenkeiten angeguckt habe ein Gif für das Display beim Starten des Arduinos 

## 4.Februar <a name="14"></a>

## 5.Februar  <a name="15"></a>


## 6. Febraur <a name="16"></a>

## 12.Februar <a name="17"></a>

Wir haben heute den Loadingscreen an das Setup geknüpft, damit das Display beim Start eine Ladebalkenanimation abspielt. Dafür haben wir festgelegt, das jeder Abschnitt des Setups 
an ein Ladebalkenbild geknüpft ist.

## 13. Februar <a name="18"></a>
Anton: Jugend debattiert
Jan: zuhause: Erstellen der Loadingscreen-Bilder mit GIMP, Schule: zuschneiden der Loading-Screen-Bilder auf 128x64 Pixel

## 14. Februar <a name="19"></a>
Anton: krank, zuhause: Fehlerbehebung beim Kalibriern über die Website und beim Upload auf das Board, 
Jan: Umwandeln der Loading-Screen-Bilder in das korrekte Bitmap-Format (horizontale Lesrichtung, Farben umkehren), Integration dieser Bitmaps in den Code
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
