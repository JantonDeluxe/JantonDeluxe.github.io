---
layout: page
title: Unterrichtstagebuch 2
subtitle: Angriff der Programmierer
---

### Inhaltsverzeichnis
>* [3. Dezember](#1)
>* [4. Dezember](#2)
>* [5. Dezember](#3)
>* [10. Dezember](#4)
>* [11. Dezember](#5)
>* [12. Dezember](#6)
>* [18. Dezember](#7)
>* [19. Dezember](#8)
>* [Ferien](#9)
>* [14. Januar](#10)
>* [15. Januar](#11)
>* [16. Januar](#12)
>* [22. Januar](#13)
>* [23. Januar](#14)
>* [28. Januar](#15)
>* [4. Februar](#16)
>* [5. Februar](#17)
>* [6. Februar](#18)
>* [12. Februar](#19)
>* [13. Februar](#20)
>* [14. Februar](#21)
>* [6. März](#22)
>* [11. März](#23)
>* [13. März](#24)
>* [Quarantäne](#25)

## Vorwort
Dieser Stundenblog baut auf dem [Stundenblog](https://jantondeluxe.github.io/2019-10-06-unterrichstagebuch/) und der [Projektbeschreibung](https://jantondeluxe.github.io/2019-10-06-projektbbeschreibung/) des ersten Halbjahres auf. Dinge, die wir zuhause erledigt haben, haben wir am Vortag eingetragen. 
Im zweiten Halbjahr haben wir uns vorgenommen, besser zusammenzuarbeiten und beide etwa gleich viel zum Höhenmesser beizutragen, was letztes Halbjahr noch nicht optimal funktioniert hat. Dafür haben wir zunächst zusammen an einem Problem weitergearbeitet, um uns gegenseitig auf den gleichen Stand zu bringen. Danach haben wir beide unterschiedliche Teile des Projektes vorangetrieben und sie dann zusammengeführt. Um dies transparent zu machen, haben wir im Stundenblog protokolliert, wer was gemacht hat. Den Stundenblog selbst hat mehrheitlich Jan geschriebn.


## 3. Dezember <a name="1"></a>
Heute haben wir zusammen haben wir die Update-Frequenz des Chart.js-Diagramms erhöht: Dafür haben wir mit `millis()` einen Timer in den Code eingebaut, der die minimale Länge eines Messzyklus misst und den durchschnittlichen Wert (Schwankung um etwa 5ms) von 115ms als neues Intervall festgelegt. 

## 2. Dezember <a name="2"></a>
Heute haben wirzusammen den Timer weiterverwendet, um die Geschwindigkeit und Beschleunigung des Höhenmessers im jeweiligen Intervall zu berechnen. Diese Werte senden wir nun auch an die Website und stellen sie im Chart.js-Diagramm dar. 

## 5. Dezember <a name="3"></a>
Heute haben wir zusammen Darstellung im Diagramm verschönert: Dafür haben wir neue Farben und ihre entsprechenden rgba-Farbcodes herausgesucht, sowie die Glättung des Graphen reduiziert, damit die Darstellung präziser ist. Ebenfalls werden alle Graphen bis auf den der Höhe standardmäßig ausgeblendet, damit man als Betrachter nicht mit Informationen überwältigt wird.

## 10. Dezember <a name="4"></a>
Heute haben wir zusammen versucht, die Netzwerkverbindungen des Boards zu harmonisieren. Ziel ist es, Netzwerk-Zugangsdaten nicht immer manuell eingeben zu müssen. Dabei gilt zu beachten, dass die [Wifi-Status-Codes](https://www.arduino.cc/en/Reference/WiFiStatus) sich bei [ESP8266-Chips](https://github.com/esp8266/Arduino/blob/4897e0006b5b0123a2fa31f67b14a3fff65ce561/libraries/ESP8266WiFi/src/include/wl_definitions.h) von denen normaler Arduinos unterscheiden. Da diese Netzwerkeinstellungen in einem gesonderten Flashbereich gespiechert werden, haben wir mithilfe des [esptools](https://github.com/espressif/esptool) den Flashspeicher formatiert. Dabei ist uns ein nicht mehr nachvollziehbarer Fehler unterlaufen oder wir sind auf einen Bug gestoßen. Auf jeden Fall war eine normale Verbindung zu dem Board nicht mehr möglich: Lediglich Boardinformationen wie den Prozessortyp und die MAC-Adresse konnten wir noch abrufen. Unsere Befürchtung war, das Board gebrickt zu haben.

## 11. Dezember <a name="5"></a>
Heute haben wir zusammen versucht, das Board zu reparieren. Nach einer Reinstallation der Treiber etc. konnten wir die Ursache auf einen fehlerhaften Bootloader zurückführen. Um dies zu beheben, haben wir zuhause mehrere Stunden versucht, eine neue Firmware auf das Board zu flashen. Dafür muss man den Pin D3 (GPIO0) erden (also mit Ground/GND verbinden). Dann kann man den Strom anschließen. Das Blinken der OnBoard-LED signalisiert, die Flash-Bereitschaft des Boards. Zum Flashen haben wir das Tool [ESPeasy](https://github.com/letscontrolit/ESPEasy) verwendet. Nun läuft der Upload auf das Board wieder.

## 12. Dezember <a name="6"></a>
Heute mussten wir zusammen zunächst die Libraries usw. neuinstallieren und die WLAN-Verbindung wieder auf seine alte Weise hergestellen. Der einzige jetzt noch auftretende Fehler liegt bei der Übertragung des index-Headers, der durch den Variablenmodifikator `PROGMEM` im Flashspeicher abgelegt ist.

## 18. Dezember <a name="7"></a>
Heute haben wir zusammen Ideen gesammelt, wie wir den Höhenmesser mit weiteren neuen Funktionen und Verbesserungen ausstatten können. Da die alte Display-Library nur ASCII-Zeichen anzeigen konnte und wir mit der Anzeige mehr vorhatten, haben eine neue, bessere [Display-Library](https://github.com/adafruit/Adafruit_SSD1306) getestet und angefangen, die Anzeige darauf umzustellen.

## 19. Dezember <a name="8"></a>
Heute haben wir zusammen die Umstellung der Anzeige zu Ende gebracht. Noch erkennt man außer der schöneren Schriftart nicht viel, allerdings haben wir durch die neue Library nun auch die Möglichkeit Bilder, Animationen usw. anzeigen zu lassen. In China hatten wir schon vor einiger Zeit für unter 1,60€ ein Data-Logger-Shield für den D1 mini Pro bestellt - diese Woche war es angekommen und heute haben wir angefangen, es einzurichten.

## Ferien <a name=9"></a>
In den Ferien hat Anton Variablen in Englisch umbenannt, um den allgemeinen Programmier-Konvetionen zu entsprechen. Dazu hat er die Temperaturanzeige auf dem Display verschönert, indem er bei positiven Temperaturen ein Leerzeichen hinzugefügt hat, wodurch die Temeraturanzeige nicht ständig verschiebt/flackert.
Jan hat Websites zum Programmieren eines Splashscreens herausgesucht und sich eingearbeitet (https://cdn-learn.adafruit.com/downloads/pdf/adafruit-gfx-graphics-library.pdf, http://javl.github.io/image2cpp/).

## 14. Januar <a name="10"></a>
Heute haben wir uns auf den neuesten Stand der Entwicklungen nach den Ferien gebracht, sowie Fragen und das weitere Vorgehen besprochen.

## 15. Januar <a name="11"></a>
Heute haben wir zusammen angefangen, die SD-Karte mit Hilfe des Datalogger-Shields einzzubinden und uns überlegt, wie wir Messreihen darauf spreichern, starten und stoppen wollen.

## 16. Januar<a name="12"></a>
Heute haben wir zusammen Stoppmechanismen getestet. Ergebnis: die Kopplung des Stoppmechanismus an die Geschwindigkeit (Vorzeichenwechsel bei Überschreiten des Scheitelpunkts der Flugbahn) funktioniert nicht, da die Messabweichung zu groß ist. Ein Timer bzw. ein Start-/Stopp-Knopf auf der Website sind bessere Wege die Messungen zu starten und stoppen.
Des weiteren haben wir versucht die SD-Karte zum Laufen zu bringen, was jedoch nicht funktioniert hat. Wir vermuten, dass für den Betrieb des ganzen Shields die Batterie benötigt wird.


Das Abrufen des Headers funktioniert wieder. Fehlerquelle war der Variablenmodifikator PROGMEM, der nach dem Flashen der neuen Firmware nicht mehr funktioniert. Eigentlich werden so Teile des Codes im Flashspeicher gespeichert, was den Arbeitsspeicher entlastet.
Website überarbeitet:Timer (Anzeige außerhalb des Graphen)

## 22. Januar <a name="13"></a>
Anton war leider krank, hatte aber daheim an der Anzeige der Temperatur im Graphen weitergearbeitet. Jan hatte sich währenddessen in das Erstellen von Bitmap-Animationen für das Display eingelesen und Entwürfe für die Animation erstellt.

## 23. Januar <a name="14"></a>
Anton war heute wieder krank und überarbeitete die Website, indem er eine Start-und About-Seite, eine Navbar, sowie neue Überschriften hinzugefügt hat. Dazu hat er das Spacing auf der Website verändert, damit sie nicht so überladen wirkt. Jan hatte wiederum passende Ladebilder für eine Animation ausgewählt und Gimp für die Bearbeitung installiert. Dann hat er mit der Bearbeitung der einzelnen Bilder der Animation begonnen.

## 28. Januar <a name="15"></a>
Anton war schon wieder krank und fügte zu Hause einen Kalibrierungsknopf und einen Start- und Stopp zur Website hinzu. Jan hatte währenddessen mit GIMP die Ladebalkenilder fertiggestellt auf 128x64 Pixel zurechtgeschnitten.

## 4.Februar <a name="16"></a>
Da der Reset des Timers durch den Stopp-Knopf auf der Website fehlerhaft war, haben wir heute den Fehler behoben, sowie eine Timeranzeige auf der Website hinzugefügt.

## 5.Februar  <a name="17"></a>
Anton hat heute zum Anzeigen des Maximums und der Temperatur Boxen mit HTML und CSS erstellt, die (nebeinander) auf der Website angezeigt werden. Währenddessen hat Jan herausgefunden, warum das Data Logger Shield für den D1 mini Pro nicht funktioniert: Wie bei anderen Benutzern, wurde auch bei unserem Shield ein Widerstand falsch aufgelötet. Mit der fehlerhafen Platine können wir also nicht weiterarbeiten.

## 6. Febraur <a name="18"></a>
Heute war Anton bei Jugend debatiert, Jan hat währenddessen die Loadingscreenbilder von 13 auf 10 Balken mit Gimp reduziert, da durch das Wegfallen des Data Logger Shields nun drei Schritte im Setup weniger ausgeführt werden.

## 12. Februar <a name="19"></a>
Da nach dem Stoppen der Messung der Redirect auf die Startseite nicht funktionierte, haben wir zusammen zunächst dieses Problem gelöst.

## 13. Februar <a name="20"></a>
Heute gab es Probleme beim Upload auf das Board. Letztendlich konnten wir den Fehler auf eine automatische Aktualisierung der Treiber durch die Arduino IDE zurückführen. Diese Treiber enthielten Bugs, die den Upload nach Beginn abstürzen ließen. Wir haben also angefangen, alte Versionen des Treiber zu suchen und zu überprüfen, ob sie den Bug auch enthalten oder nicht.

## 14. Februar <a name="21"></a>
Anton war heute krank und hat daheim das Kalibrieren über die Website ermöglicht und eine kompatible Arduino-Firmware herrausgesucht. Dazu hat er die Passwörter aus den Dateien auf der GitHub-Seite gelöscht. Jan hat heute die Bilder für die Ladebildschirmanimation in ein Bitmapformat neu konvertiert. Dafür musste die Leserichtung und die Umkehrung der Farben neu gemacht werden. Anschleißend wurden diese in den Code eingefügt, um es zu testen.

## 6. März <a name="22"></a>
Jan hat heute am Projekttagebuch weitergearbeitet, während Anton sich um die Projektbeschreibung gekümmert hat.

## 11. März <a name="23"></a>
Heute haben wir weiterhin an der Dokumentation des Projekts gearbeitet. Jan hat dabei die Notizen des Projekttagebuchs ausformuliert und Anton hat an der Projektbeschreibung weitergearbeitet.

## 13. März <a name="24"></a>


## Quarantäne <a name="25"></a>
