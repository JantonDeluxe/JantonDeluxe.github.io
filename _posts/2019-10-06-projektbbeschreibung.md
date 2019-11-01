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

### Höhenmesser

Für die Benutzung des BMP180 benötigt man zwei Libraries oder auf deutsch: Programmbiliotheken. Diese enthalten Code-Teile, wie z.B. vorgefertigte Funktionen oder Maschinencode, auf die dann im eigenen Programm einfach zugegriffen werden kann, ohne sie dort selbst schreiben oder einfügen zu müssen.

```c++
#include <Wire.h>
#include <SFE_BMP180.h>
```

*Wire* sorgt dafür, dass der Arduino mit dem Höhenmesser kommunizieren kann, da dieser zur Kommunikation das I²C-Protokoll benutzt.[[1]][BMP180-Datenblatt] I²C steht für "Inter-Integrated Circuit bus" und ist, wie der Name schon sagt, dafür da, integrierte Schaltkreise (wie den Höhenmesser und den Arduino) zu verbinden. Der größte Vorteil dieses Busses ist, dass mehrere dieser Schaltkreise über die gleiche Leitung verbunden werden können.[[2]][I²C]

*SFE_BMP180* ist die Library, die den Maschinencode für den Höhenmesser enthält. Damit ermöglicht sie das auslesen und ansteuern des Höhenmessers. 




## Quellen
[BMP180-Datenblatt]:https://ae-bst.resource.bosch.com/media/_tech/media/datasheets/BST-BMP180-DS000.pdf
[I²C]:https://www.ipd.kit.edu/mitarbeiter/buchmann/microcontroller/i2c.htm

