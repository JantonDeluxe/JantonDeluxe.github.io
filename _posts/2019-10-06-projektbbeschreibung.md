---
layout: page
title: Projektbeschreibung
subtitle: Wie funktioniert der Höhenmesser?
---

### Inhaltsverzeichnis
>* [Ziel](#1)
>* [Hardware](#2)
>* [Software](#3)
>* [Quellen](#4)

## Ziel<a name="1"></a>
Höhenmesser, der günstiger ist, als die 50€ off-the-shelf-Dinger.
Höhe (aktuell und max. messen) und ausgeben (auf display und auf webserver)

## Hardware <a name="2"></a>
Wemos D1 mini Pro (Arduino)

Bosch BMP180

GM009605 OLED-Display mit SSD1306-Controller

## Software <a name="3"></a>
Hier beschreiben wir die einzelnen Komponenten des Programms getrennt, damit die Erklärung nicht zu kompliziert wird.

***
### Serielle Schnittstelle
Für die Übertragung von Daten an den PC nutzen wir die serielle Schnittstelle des Arduino. Diese Überträgt per Micro-USB-Kabel Daten an den PC, die mit dem seriellen Monitor in der Arduino IDE ausgelesen werden können.

`Serial.begin()` startet die Übertragung, in diesem Falle mit einer Übertragungsgeschwindigkeit von 115200 Bits pro Sekunde.

`Serial.println("")` schreibt Daten als ASCII-Text und beginnt danach im Gegensatz zu `Serial.Print()` eine neue Zeile. Hier verhindert eine leere Ausgabe Darstellungsfehler zu Beginn der Übertragung.

```c++
 Serial.begin(115200);
 delay(100);
 Serial.println("");
 Serial.println("Datenübertragung gestartet!");
```
***

### Höhenmesser
*Bei der Einbindung des Höhenmessers haben wir uns am [Example-Sketch](https://github.com/JantonDeluxe/luft-waffle/blob/master/Code/SFE_BMP180_example/SFE_BMP180_example.ino) von Mike Grusin aus der SFE_BMP180-Library orientiert.*

#### Bibliotheken einbinden
Für die Benutzung des BMP180 benötigt man zwei Libraries oder auf deutsch Programmbiliotheken:
* **SFE_BMP180** ist die Library, die den Maschinencode für den Höhenmesser enthält. Damit ermöglicht sie das auslesen und ansteuern des Höhenmessers. 

* **Wire** sorgt dafür, dass der Arduino mit dem Höhenmesser kommunizieren kann, da dieser zur Kommunikation das I²C-Protokoll benutzt.[[1]][BMP180-Datenblatt] 
I²C steht für "Inter-Integrated Circuit bus" und ist, wie der Name schon sagt, dafür da, integrierte Schaltkreise (wie den Höhenmesser und den Arduino) zu verbinden. Der größte Vorteil dieses Busses ist, dass mehrere Schaltkreise über die gleiche Leitung verbunden werden können, man also nur eine Leitung benötigt.[[2]][I²C]

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


### Höhe berechnen
Die Methode `pressure.altitude()` berechnet mit Hilfe der Barometrischen Höhenformel den zurückgelegten Höhenunterschied basierend auf dem aktuellen Druck und dem Ausgangsdruck.[[3]][Höhenformel]
![Barometrische Höhenformel](https://github.com/JantonDeluxe/luft-waffle/blob/master/Bilder/Druck.jpg?raw=true)

```c++
a = pressure.altitude(P, ausgangsdruck);
```

## Quellen
[BMP180-Datenblatt]:https://ae-bst.resource.bosch.com/media/_tech/media/datasheets/BST-BMP180-DS000.pdf
[I²C]:https://www.ipd.kit.edu/mitarbeiter/buchmann/microcontroller/i2c.htm
[Höhenformel]:https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-/all

