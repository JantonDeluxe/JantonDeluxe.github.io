---
layout: page
title: Unterrichtstagebuch 2
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
>* [12. Februar](#17)
>* [13. Februar](#18)
>* [14. Februar](#19)
>* [6. März](#20)
>* [11. März](#21)
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
In den Ferien hat Anton Variablen in Englisch umbenannt und Websites herausgesucht zum Programmieren eines Splashscreens(https://cdn-learn.adafruit.com/downloads/pdf/adafruit-gfx-graphics-library.pdf, http://javl.github.io/image2cpp/).Dazu hat er die Temperaturanzeige auf dem Display verschönert, indem er bei positiven Temperaturen ein Leerzeichen hinzugefügt hat, wodurch die Temeraturanzeige nicht ständig verschiebt/flankert.
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
Liebes Unterrichtstagebuch,
Anton war leider krank, hatte aber daheim an der Anzeige der Temperatur im Graphen weitergearbeitet. Jan hatte sich währenddessen in das Erstellen von Bitmap-Animationen für das Display eingelesen.


## 23. Januar <a name="12"></a>
Anton war heute wieder krank und überarbeitete die Website, indem er eine Start-und About-Seite, eine Navbar, sowie neue Überschriften hinzugefügt hat. Dazu hat er das Spacing verändert. Jan hatte wiederum passende Ladebilder für eine Animation ausgewählt und Gimp für die Bearbeitung installiert.
Anton: krank, Überarbeitung der Website: Hinzufügen der Start- und About-Seite, Navbar, neue Überschriften + Spacing
Jan: Ladenbalkenbilder ausgewählt

## 28. Januar <a name="13"></a>
Anton war schon wieder krank und fügte zu Hause einen Kalibrierungsknopf und einen Start- und Stopp zur Website hinzugefügt. Jan hatte währenddessen die Ladebalken auf 128x64 Pixel zurechtgeschnitten.

## 4.Februar <a name="14"></a>
Da der Rest des Timers durch den Stopp-Knopf auf der Website fehlerhaft war, haben wir heute den Fehler behoben sowie eine Timeranzeige der Website hinzugefügt.
Zusammen: Fehler beim Reset des Timers behoben, Anzeige des Timers auf der Website

## 5.Februar  <a name="15"></a>
Heute haben wir zum Anzeigen des Maximums und der Temperatur Boxen auf die Website programmiert.
Zusammen: Anzeige des Maximums und der jetzigen Temperatur auf der Website nebeneinander in eigenen Boxen

## 6. Febraur <a name="16"></a>
Da nach Stoppen der Messung der Redirect nicht funktionierte, haben wir uns heute 
Zusammen: Redirect nach stoppen der MA
Heute war Anton bei Jugend debatiert, Jan hat währenddessen die Loadingscreenbilder auf 10 Balken mit Gimp reduziert.
Anton: bei Jugend debattiert
Jan: zuschneiden der einzelnen Loading-Screen-Bilder auf 10 Balken

## 14. Februar <a name="19"></a>
Anton war heute krank und hat daheim das Kalibrieren über die Website ermöglicht und eine kompatible Arduino-Firmware herrausgesucht. Dazu hat er die Passwörter für die Website von der GitHub-Seite gelöscht. Jan hat heute die Bilder für die Ladebildschirmanimation in ein Bitmapformat neu konvertiert. Dafür musste die Leserichtung und die Umkehrung der Farben neu gemacht werden. Anschleißend wurde diese in den Code eingefügt, um es zu testen.
Anton: krank, Fehlerbehebung beim Kalibriern über die Website und beim Upload auf das Board, Löschen der Passwörter aus den Dateien auf GitHub (Danke an Nick für die Hilfe!)
Jan: Umwandeln der Loading-Screen-Bilder in das korrekte Bitmap-Format (horizontale Lesrichtung, Farben umkehren), Integration dieser Bitmaps in den Code, testen, ob richtig angezeigt wird

Was wir noch machen können:
- Wechselnder Start/Stopp-Button
- Speichern der CSV-Datei in SPIFFS
- Chart.js taucht erst auf, wenn Start gedrückt
- Schickeres Desgin (Navbar mit neuen Farben, runder (mit Logo), Anzeige-Kästen, CSV-Knopf(button3) mit neuer Farbe, 
- funktionierender Redirect, wenn timer abgelaufen
- eigene Platine + Stromversorgung

## 6. März <a name="20"></a>
Jan hat heute am Projekttagebuch weitergearbeitet, während Anton sich um die Projektbeschreibung gekümmert hat.

## 11. März <a name="21"></a>
Heute haben wir weiterhin an der Dokumentation des Projekts gearbeitet. Jan hat dabei die Notizen des Projekttagebuchs ausformuliert und Anton hat an der Projektbeschreibung weitergearbeitet.

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

