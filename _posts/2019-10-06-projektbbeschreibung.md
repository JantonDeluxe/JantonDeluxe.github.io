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

Für die Benutzung des BMP180 benötigt man zwei Libraries:
```c++
#include <SFE_BMP180.h>
#include <Wire.h>
```
"SFE_BMP180" ist die Höhenmesser-Library, "Wire" sorgt dafür, dass der Arduino mit dem Höhenmesser kommunizieren kann, da dieser zur Kommunikation das I²C-Protokoll benutzt.[[1]][BMP180-Datenblatt] Für diese Kommunikation

## Quellen
[BMP180-Datenblatt]:https://ae-bst.resource.bosch.com/media/_tech/media/datasheets/BST-BMP180-DS000.pdf

