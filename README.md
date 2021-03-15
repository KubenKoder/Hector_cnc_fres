# Hector_cnc_fres
Alt som rører hector cnc-fresen på Kuben vgs robotikklinja

## Historikk

Hector is a CNC machine in the [fabricateable machines family](https://github.com/fellesverkstedet/fabricatable-machines/wiki)

#### 2017
HECTOR was initially a 2017 fab academy final project. [Here is the full fab academy documentation.](http://archive.fabacademy.org/archives/2017/fablabverket/students/100/web/projects/diy_cnc/index.html)
#### 2018
In 2018 it got an upgrade with [JMC iHSS60 nema24 integrated closed loop stepper motors](https://www.aliexpress.com/item/NEMA24-3Nm-425oz-in-Integrated-Closed-Loop-Stepper-motor-with-driver-36VDC-JMC-iHSS60-36-30/32822797339.html) which made it more reliable and simplified its electonics.
[Upgrade documentation on the fabricateable machines repo](https://github.com/fellesverkstedet/fabricatable-machines/blob/master/hector-medium-format-cnc/README.md)

#### 2020
HECTOR is currently being fitted for use by the pupils at Kuben upper secondary vocational school in Oslo, Norway. [Kuben school page in Norweigian](https://kuben.vgs.no/) 

#### 2021

##### 08.03
Vi testet motoren. Vi har ikke helt fått den til å funke som ønsket.
Det vi mangler er å finne startsekvensen som kan aktivere motoren. Vi fikk den til å kjøre en gang, men vet ikke helt hva vi gjorde for at det skulle skje. Ifølgen [denne](https://howtomechatronics.com/tutorials/arduino/arduino-brushless-motor-control-tutorial-esc-bldc/) fremgangsmåten for børsteløs motor kontroller kan man bruke Arduino sitt innebygde bibliotek for servoer. Vi startet uten et potenometer men ente opp med å bruket et. Men vi vurderer å bytte den ut med en vanlig knapp.

Kode som funket en gang:
```C++
/*
        Arduino Brushless Motor Control
     by Dejan, https://howtomechatronics.com
*/


#include <Servo.h>
Servo ESC;     // create servo object to control the ESC

int potValue;  // value from the analog pin

void setup() {
  // Attach the ESC on pin 9
  ESC.attach(9,1000,2000); // (pin, min pulse width, max pulse width in microseconds) 

  potValue = analogRead(A0);   // reads the value of the potentiometer (value between 0 and 1023)
  potValue = map(potValue, 0, 1023, 0, 180);   // scale it to use it with the servo library (value between 0 and 180)
  ESC.write(potValue);    // Send the signal to the ESC
}

void loop() {
  potValue = 90;
  ESC.write(potValue);    
}
```
Denne koden er fra fremgangsmåten ovenfra, men endrett litt. Her er den orginale koden:
```C++
/*
        Arduino Brushless Motor Control
     by Dejan, https://howtomechatronics.com
*/
#include <Servo.h>
Servo ESC;     // create servo object to control the ESC
int potValue;  // value from the analog pin
void setup() {
  // Attach the ESC on pin 9
  ESC.attach(9,1000,2000); // (pin, min pulse width, max pulse width in microseconds) 
}
void loop() {
  potValue = analogRead(A0);   // reads the value of the potentiometer (value between 0 and 1023)
  potValue = map(potValue, 0, 1023, 0, 180);   // scale it to use it with the servo library (value between 0 and 180)
  ESC.write(potValue);    // Send the signal to the ESC
}
```

##### 10.03
Motoren trekker opp mot 90A, og trenger nøyaktig 22.2V. Hvis ikke vil den gi en "feil-volts-melding" der den rykker litt fram og tilbake og piper. Det er satt inn en ny PSU, men vi trenger en enda større, fordi den vi har er bare laget for å gi 40A. Den går da i "boost-mode" for å kompansere.

Alle ledninger er blitt dratt ut, så vi kan begynne å komprimere hele anlegget. Det vi trenger å få på plass er en boks for kretskortet og for Arduinoen som skal styre motoren. 


Koden for motoren som ble brukt for test:
```C++
/*
 Controlling a servo position using a potentiometer (variable resistor)
 by Michal Rinott <http://people.interaction-ivrea.it/m.rinott>

 modified on 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Knob
*/

#include <Servo.h>
#include <ESC.h>

Servo myECS;  // create servo object to control a servo



int potpin = 0;  // analog pin used to connect the potentiometer
int val;    // variable to read the value from the analog pin

void setup() {
  
  myECS.attach(9);  // attaches the servo on pin 9 to the servo object
  Serial.begin(9600);
}

void loop() {
  int SensorValue = analogRead(A0);
  val = analogRead(potpin);            // reads the value of the potentiometer (value between 0 and 1023)
  val = map(val, 0, 1023, 1000, 2000);     // scale it to use it with the servo (value between 0 and 180)
  myECS.write(val);                  // sets the servo position according to the scaled value

  Serial.println(SensorValue);
  
  delay(10);                           // waits for the servo to get there
  
}
``` 

##### 15.03
Vi har montert av koblingsbrettet og flyttet Hector opp på toppen. Vi har funnet en boks som vi kan montere PSU-ene inn i på siden, og modelert en boks for kretskortet. 

Boks:
!(https://github.com/KubenKoder/Hector_cnc_fres/blob/main/bilder/Skjermbilde%202021-03-15%20134232.png)
!(https://github.com/KubenKoder/Hector_cnc_fres/blob/main/bilder/Skjermbilde%202021-03-15%20134352.png)
!(https://github.com/KubenKoder/Hector_cnc_fres/blob/main/bilder/Skjermbilde%202021-03-15%20134523.png)