# Proiect individual - Monitorizarea umiditatii aerului - Moldovan Bogdana Maria 235/1

## Project Description

The project involves monitoring the air humidity level using an Arduino UNO board, a DHT11 temperature and humidity sensor module, and two LEDs. The humidity values are updated every second. A humidity value above 65% triggers the illumination of a red LED, while a humidity value below 65% triggers the illumination of a green LED.

## Schematics

![Schema](https://user-images.githubusercontent.com/100224995/230779901-2059ca1a-ccff-427f-be10-88869323588e.png)

## Pre-requisites
### Componente hardware
1 x Arduino UNO board [Arduino Uno.pdf](https://github.com/at-cs-ubbcluj-ro/individual-project-mbir2969/files/11178687/Arduino.Uno.pdf)
7 x male-to-male jumper wires
2 x 5 mm LEDs
1 x DHT11 humidity sensor module [DHT11-Datasheet.pdf](https://github.com/at-cs-ubbcluj-ro/individual-project-mbir2969/files/11178689/DHT11-Datasheet.pdf)
1 x USB 3.0 to USB B cable for power supply connection
1 x 400-point breadboard
2 x 10k ohm resistors   

### Componente software
Arduino IDE https://www.arduino.cc/en/software

## Setup and Build
### Configurare hardware
The setup is done following these steps:
  1. Place the humidity sensor on the breadboard.
  2. Place the two LEDs and a resistor in front of each on the breadboard.
  3. Connect the sensor to the Arduino board as follows:
     a. Connect the VCC (+) pin of the sensor to the 5V pin of the board.
     b. Connect the GND (-) pin of the sensor to the GND pin of the board.
     c. Connect the Signal pin of the sensor to the digital pin 7 of the board.
  4. Connect the LEDs to the Arduino board as follows:
     a. Connect the Anode (+) pin of the LED to the digital pin 12 of the board.
     b. Connect the Cathode (-) pin of the LED to the GND pin of the board.
  5. Connect the board to the laptop using the USB 3.0 to USB B power cable.             

### Configurare software
For writing the code, Arduino IDE was used, where I set the COM4 port and Arduino Uno board as the working board. The code is presented and specified in the "Arduino Code" section. To use the C library dht.h for working with the humidity sensor, you need to extract the folder from the archive [DHTLib.zip](https://github.com/at-cs-ubbcluj-ro/individual-project-mbir2969/files/11178698/DHTLib.zip). This folder containing the files dht.h and dht.c should be added to C:\Users\YOURUSERNAME\OneDrive\Documents\Arduino\libraries.

(Note: Replace "YOURUSERNAME" with your actual username on your computer.)

## Running
To operate the setup, the code is compiled in the Arduino IDE and then uploaded to the board. Upon uploading, the LEDs on the board flash briefly. After the upload, you can monitor the updated air humidity value every second in the Serial Monitor (accessed from the top right corner of the Arduino IDE). Each sensor reading is processed as follows: if the humidity is above 65%, the message "High humidity" is displayed in the Serial Monitor, and the red LED on the breadboard lights up; if the humidity is less than or equal to 65%, the message "Low humidity" is displayed in the Serial Monitor, and the green LED on the breadboard lights up. Each of the two LEDs remains lit for one second for each reading.

## Demo - Video capture


https://user-images.githubusercontent.com/100224995/230737270-34bc3df8-c30a-461d-87bc-ec5716ff655f.mp4



## Arduino Code
```
#include <dht.h>

dht DHT;  // initializam senzorul

#define DHT11_PIN 7  // se seteaza pinul unde este conectat senzorul
int ledRosu = 3;     // se seteaza pinul unde este conectat LED-ul Rosu (LED pentru umiditate mare)
int ledVerde = 13;   // se seteaza pinul unde este conectat LED-ul Verde (LED pentru umiditate scazuta)

void setup() {
  Serial.begin(9600);         // incepe comunicarea seriala cu o rata de transmisie de 9600 de biti pe secunda
  pinMode(DHT11_PIN, INPUT);  // seteaza pinul senzorului ca fiind de intrare (INPUT)
  pinMode(ledRosu, OUTPUT);   // seteaza pinul LED-ului rosu ca fiind de iesire (OUTPUT)
  pinMode(ledVerde, OUTPUT);  // seteaza pinul LED-ului verde ca fiind de iesire (OUTPUT)
}


void loop() {
  DHT.read11(DHT11_PIN);              // citeste valorile senzorului DHT11
  Serial.print("Humidity = ");        // afiseaza mesajul "Humidity = " pe monitorul serial
  Serial.println(DHT.humidity);       // afiseaza valoarea de umiditate citita de senzor
  if (DHT.humidity > 65) {            // daca valoarea umiditatii citita este mai mare de 65
    digitalWrite(ledRosu, HIGH);      // aprinde LED-ul rosu
    delay(1000);                      // asteapta o secunda
    long time = millis();             // obtine timpul curent in milisecunde
    Serial.println("High humidity");  // afiseaza mesajul "High humidity" pe monitorul serial
    digitalWrite(ledRosu, LOW);       // stinge LED-ul rosu
  }
  if (DHT.humidity <= 65) {          // daca valoarea umiditatii citita este mai mica sau egala cu 65
    digitalWrite(ledVerde, HIGH);    // aprinde LED-ul verde
    delay(1000);                     // asteapta o secunda
    long time = millis();            // obtine timpul curent in milisecunde
    Serial.println("Low humidity");  // afiseaza mesajul "Low humidity" pe monitorul serial
    digitalWrite(ledVerde, LOW);     // stinge LED-ul verde
  }
  delay(1000);  // asteapta o secunda inainte de a repeta bucla
}
```
