---
layout: page
title: Quellcode
subtitle: Gesamter Code zum Projekt
---

# .ino File
```c++
/* Höhenmesser

  Hardware:
  ---------
  Board: WEMOS D1 mini pro V1.0
  Altimeter: Bosch BMP180
  Display: GM009605 OLED

  

  Basiert teilweise auf dem Sketch BMP180_altitude_example aus der SFE_BMP180-Library:
  V10 Mike Grusin, SparkFun Electronics 10/24/2013
  V1.1.2 Updates for Arduino 1.6.4 5/2015

 
*/

// Libraries
#include <SFE_BMP180.h>
#include <Wire.h>
#include <SSD1306Ascii.h>
#include <SSD1306AsciiWire.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

// Header
#include "index.h"

// I2C-Adresse OLED
#define I2C_ADDRESS 0x3C

// Objekte
SFE_BMP180 pressure;
SSD1306AsciiWire oled;

// Webserver-Port setzen
ESP8266WebServer server(80);

// Name und Passwort Access Point
const char *ssid = "Janky";  
const char *password = "6zhnJI9ol."; 

// Kalibrierung: Anzahl der Messungen
const int messungen = 100;

// Globale Variablen
double highest;
double lowest;
double T;
double a;
double* pointereins = &T;
double* pointerzwei = &a;

float Array[messungen];
float ausgangsdruck = 0.0;


//Setup
void setup(void) {

  //Datenübertragung
  Serial.begin(115200);
  delay(100);
  Serial.println("");
  Serial.println("Datenübertragung gestartet!");
  
  //Display-Setup
  Wire.begin();
  oled.begin(&Adafruit128x64, I2C_ADDRESS);
  oled.set400kHz();
  oled.setFont(font5x7);
  
  Serial.println("Display gestartet!"); 

  oled.clear();
  oled.set2X();
  oled.println("START");
  oled.set1X();
  delay(500);
  oled.clear();

  //Sensor-Setup
  if (pressure.begin())
    Serial.println("BMP180 gestartet!");
  else
  {
    Serial.println("BMP180 fehlt!");
    while (1);
  }

  // IP-Adresse
  IPAddress ip(192, 168, 178, 220);
  IPAddress dns(192, 168, 178, 1);
  IPAddress gateway(192, 168, 178, 1);
  IPAddress subnet(255, 255, 255, 0);
  WiFi.config(ip, dns, gateway, subnet);

  // WLAN-Verbindung
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.print("Verbunden mit:");
  Serial.println(ssid);

  // Webserver-Setup
  server.on("/", handleRoot);
  server.on("/readData", handleData);
  server.onNotFound(handleNotFound);
  server.begin();

  Serial.println("HTTP-Server gestartet!"); 
  Serial.print("IP: ");
  Serial.println(WiFi.localIP());
  
  // Kalibrierung
  for (int i = 0; i < messungen; i++)
  {
    Array[i] = getPressure();
  }
  
  for (int i = 0; i < messungen; i++)
  {
    ausgangsdruck = ausgangsdruck + Array[i];
  }
  
  ausgangsdruck = ausgangsdruck / messungen;

  // statische Teile der Höhenanzeigen
  oled.setCursor(0, 3);
  oled.print("Hoehe:");
  oled.setCursor(0,5);
  oled.print("Max:");

  }


  // Hauptcode
  void loop(void) {

    // Variablen
    double P;
    char status;

    // Webserver
    server.handleClient();
    
    // Druck messen
    P = getPressure();

    // Temperatur messen
    status = pressure.startTemperature();
    delay(100);
    status = pressure.getTemperature(T);

    // Höhenunterschied
    a = pressure.altitude(P, ausgangsdruck);

    // Maximalwerte
    if (a < lowest) lowest = a;

    if (a > highest) highest = a;

    // Höhenunterschied anzeigen
    oled.set2X();
    oled.setCursor(40, 2);               
    if (a >= 0.0) oled.print(" ");      
    oled.print(a);
    oled.print("m");

    // Maximum anzeigen
    oled.setCursor(40, 4);               
    if (highest >= 0.0) oled.print(" ");
    oled.print(highest);
    oled.print("m");
    oled.set1X();

    // Druckanzeige
    oled.setCursor(0, 0);
    oled.print(getPressure());
    oled.print(" mbar");

    // Temperaturanzeige
    oled.setCursor(79, 0);
    if (a >= 0.0) oled.print(" ");
    oled.print(*pointereins);
    oled.println(" C");

    // IP-Adresse anzeigen
    oled.setCursor(0, 7);
    oled.print("IP: ");
    oled.print(WiFi.localIP());
    
    delay(333);
  }
  

// Webserver
void handleRoot() {
  String s = MAIN_page;
  server.send(200, "text/html", s);
}

void handleData(){
  double d = *pointerzwei;
  double t = millis() / 1000;
  String teil = String(String(t) + ";");
  String kombi = String(teil + String(d));
  server.send(200, "text/plain", kombi);
}

void handleNotFound(){              
  server.send(404, "text/plain", "404: Not found"); 
}


// Funktion getPressure
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

# Header
```
const char MAIN_page[] PROGMEM = R"=====(
<!doctype html>
<html>
<head>
  <title>H&ouml;henmesser</title>
   <script src = "https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.3/Chart.min.js"></script>
  <style>
  canvas{
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
  }

  /* Data Table Styling */
  #dataTable {
    font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  #dataTable td, #dataTable th {
    border: 1px solid #ddd;
    padding: 8px;
  }

  #dataTable tr:nth-child(even){background-color: #f2f2f2;}

  #dataTable tr:hover {background-color: #ddd;}

  #dataTable th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #4CAF50;
    color: white;
  }
  </style>
</head>

<body>
    <div style="text-align:center;">
    <h1 style="font-family:verdana;color:#999999">H&ouml;henmesser</h1>
    </div>
    <div class="chart-container" style="position: relative; height:350px; width:100%">
        <canvas id="Chart" width="400" height="400"></canvas>
    </div>
<div>
  <table id="dataTable">
    <tr><th>Zeit</th><th>H&ouml;he</th></tr>
  </table>
</div>

<script>
  var data = [];
  var time = "";

  let graph;

  function addGraph()
  {
    var ctx = document.getElementById("Chart").getContext('2d');
    graph = new Chart(ctx, {
      type: 'line',
        data: {
        labels: [time],  //Bottom Labeling
        datasets: [{
          label: "H\u00f6he",
          fill: false,  //Try with true
          backgroundColor: 'rgba( 243, 156, 18 , 1)', //Dot marker color
          borderColor: 'rgba( 243, 156, 18 , 1)', //Graph Line Color
          data: null, // Wenn es noch keine Daten gibt wird 'null' angezeigt
        }],
        },
        options: {
          title: {
            display: true,
            text: "H\u00f6he"
          },
          maintainAspectRatio: false,
          elements: {
            line: {
              tension: 0.5 //Smoothening (Curved) of data lines
            }
          },
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero:true
              }
            }]
        }
      }
    })
  }

  // Graphen nach erhalt neuer Daten aktualisieren
  function updateGraph() {
    graph.data.labels.push(time);
    graph.data.datasets.forEach((dataset) => {
        dataset.data.push(data);
    });
    graph.update();
  }

  //Beim Starten der Seite Graphen anzeigen
  window.onload = function() {
    addGraph();
  };

  // Update-Geschwindigkeit (500 ms)
  setInterval(function() {
    getData();
  }, 333); 


  // Graph mit Daten anzeigen
  function showGraph(data) {
    graph.data.labels.push(timeStamp);
    graph.data.datasets.forEach((dataset) => {
        dataset.data.push(data);
    });
    graph.update();
  }

  // Daten unter /readData abrufen
  function getData() {
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
      if (this.readyState == 4 && this.status == 200) {
        var response = this.responseText.split(";"); // Erstellt ein array aus den Teilen des response-strings (';' ist das Trennzeichen)
        time = response[0]; // Zeit hat den ersten Index...
        data = response[1]; // ...und die Höhe den zweiten
        updateGraph();
        var table = document.getElementById("dataTable");
        var row = table.insertRow(1); //Add after headings
        var cell1 = row.insertCell(0);
        var cell2 = row.insertCell(1);
        cell1.innerHTML = time;
        cell2.innerHTML = data;
      }
    };
    xhttp.open("GET", "readData", true);
    xhttp.send();
  }
</script>
</body>

</html>

)=====";
```
