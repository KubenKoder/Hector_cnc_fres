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
Det vi mangler er å finne startsekvensen som kan aktivere motoren. Vi fikk den til å kjøre en gang, men vet ikke helt hva vi gjorde for at det skulle skje. Ifølgen [denne](https://howtomechatronics.com/tutorials/arduino/arduino-brushless-motor-control-tutorial-esc-bldc/) fremgangsmåten for børsteløs motor kontroller kan man bruke Arduino sitt innebygde bibliotek for servoer.

Kode som funket en gang:
```C++
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