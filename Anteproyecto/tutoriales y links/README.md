### tutoriales de internet

https://github.com/Circuit-Digest/Interfacing-Thermal-Printer-with-Raspberry-Pi#code-examples

https://circuitdigest.com/microcontroller-projects/thermal-printer-interfacing-with-raspberry-pi-zero-to-print-text-images-and-bar-codes 

https://www.youtube.com/watch?v=LwKOvT7rCiU 

https://www.youtube.com/watch?v=x_vnZuSBBmk 

https://www.youtube.com/watch?v=v1eDYvFvRoA&t=104s 

### creador de imagenes formato boleta

https://javl.github.io/image2cpp/ 

### codigo encoder a lcd

https://forum.arduino.cc/t/menu-lcd-encoder-click-encoder-como-hacerlo/277548 

```
#include "Arduino.h"  //Incluye Ide Arduino.
//#include "WProgram.h" 
#include "Switch.h"     //INCLUYE LA LIBRERIA SWITCH
#include <Wire.h>                //INCLUYE LA LIBRERIA PARA EL DAPTADOR IC2
#include <LiquidCrystal_I2C.h>    // INCLUYE LA LIBRERIA PARA EL LCD 
LiquidCrystal_I2C lcd(0x27, 2, 1, 0, 4, 5, 6, 7, 3, POSITIVE);// CONSTRUCTOR DE EL OBJETO LCD 


const byte pinA = 2;      // encoder pin A to Arduino pin 2 which is also interrupt pin 0 which we will use
const byte pinB = 3;      // encoder pin B to Arduino pin 3 which is also interrupt pin 1 but we won't use it
byte state = 0;           // will store two bits for pins A & B on the encoder which we will get from the pins above
int level = 0;            // a value bumped up or down by the encoder

const byte  swEpin = 7;  //PIN EN EL CONECTAREMOS EL SWITCH 
const byte  pinD = 13;   //PIN PARA HACER BRILLAR EL LED DEL PIN13 DE ARDUINO
int contador =0;         //VARIABLE PARA ALMACENAR EL CONTADOR 

/* A truth table of possible moves 
    1 for clockwise
   -1 for counter clockwwise
    0 for error - keybounce */
int bump[] = {0,0,-1,1};
String bits[] = {"00","01","10","11"}; //Solo para imprimir en serial, sera eliminado mas tarde.

     Switch swE        =   Switch(  swEpin,   INPUT, HIGH); //CONSTRUCTOR, QUE CREA UN OBJETO SWITCH
//     ↑     ↑                 ↑      ↑          ↑      ↑
//  FUNCION NOMBRE DEL      FUNCION  PIN DEL   ESTO ES IGUAL QUE DIGITAL
//            SWITCH                 SWITCH    DIGITAL.READ(INPUT), ENABLE PULL DOWN RESISTOR

void setup() {
  pinMode(pinA,INPUT);    // reads Pin A of the encoder
  pinMode(pinB,INPUT);    // reads Pin B of the encoder
    /* Writing to an Input pin turns on an internal pull up resistor */
  digitalWrite(pinA,HIGH);
  digitalWrite(pinB,HIGH);
    /* Set up to call our knob function any time pinA rises */
  attachInterrupt(0,knobTurned,RISING);    // calls our 'knobTurned()' function when pinA goes from LOW to HIGH
  level = 50;              // a value to start with
  
  Serial.begin(9600);   
  /* Set up for using the on-screen monitor */
  Serial.println("Encoder Ready");
  Serial.print("level = ");
  Serial.println(level);   // to remind us where we're starting
  
  pinMode(pinD, OUTPUT); 
  lcd.begin(16,2);   // initialize the lcd for 16 chars 2 lines, turn on backlight
  lcd.backlight();
  lcd.clear();       //LIMPIA LA PANTALLA LCD. 

  lcd.setCursor(0,0); 
  lcd.print("POSIC. ");
  lcd.setCursor(0,1); 
  lcd.print("CONTADOR ");
}

void loop(){
swE.poll(); 
//↑ FUNCION NECESARIA PARA REFRESCAR EL ESTADO DEL BOTON. 


  if(swE.released()== HIGH){
//   ↑N.SW   ↑ ESTA FUNCION SOLO DEVUELVE HIGH CUANDO EL BOTON HA SIDO SOLTADO 
//    POR LO QUE ES PERFECTA PARA ESTE TRABAJO.
        contador= contador+1 ;
        Serial.print("Contador ="); 
        Serial.println(contador); 
        digitalWrite(pinD, HIGH); 
        lcd.setCursor(9,1); 
        lcd.print(contador);}
   
  else{
    digitalWrite(pinD, LOW); }
    lcd.setCursor(9,0); 
    lcd.print(level);
}



void knobTurned(){
  /* AH HA! the knob was turned */
  state = 0;    // reset this value each time
  state = state + digitalRead(pinA);   // add the state of Pin A
  state <<= 1;  // shift the bit over one spot
  state = state + digitalRead(pinB);   // add the state of Pin B
  
  /* now we have a two bit binary number that holds the state of both pins
     00 - something is wrong we must have got here with a key bounce
     01 - sames as above - first bit should never be 0
     10 - knob was turned backwards
     11 - knob was turned forwards
     */
     
  /* We can pull a value out of our truth table and add it to the current level */
  level = level + bump[state];
  
  /* Let's see what happened */
  Serial.print(bits[state] + "    ");  // show us the two bits
  Serial.print(bump[state],DEC);       // show us the direction of the turn
  Serial.print("    ");
  Serial.println(level);               // show us the new value
}
```
