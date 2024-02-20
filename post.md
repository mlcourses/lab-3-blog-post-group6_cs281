# Lab 3: Ramp Circuit

## Overview and Motivation
This lab will introduce us to changing numbers on a LED display using a knob. Specifically, we will use a potentiometer to change the resistance of a voltage divider, which in turn change the numbers on the LED display. The lab will arm us with background and skills in combinational logic, analog and digital input and output, SOP designs and Boolean Simplification, as well as electrical components such as resistors and Ohm's Law. Since the circuit is complex, it helps us practice wiring and color coding skills.
## Materials
Like other labs, we will need a PB-503 breadboard and an Arduino. We also need a 7-segment LED display that can show numbers from 0-5 using combinational circuits.

## Project Steps
### Arduino and Potentiometer
Before diving into building the complex combinational circuits, we need to make sure the Potentiometer is giving correct analog input, and the Arduino correctly converts the input from the pot into digital output. We start by connecting the V output of the potentiometer to the A0 pin on the Arduino. Since we already connect the laptop to the Arduino, we only need to connect the Arduino to the breadboard's ground. Next, we compile and transfer the following code to the Arduino:

    const int potpin = 0;
    const int WAIT = 1000; // 1 second delay
    void setup () {
        Serial.begin(9600);
        pinMode(11,OUTPUT);
        pinMode(12,OUTPUT);
        pinMode(13,OUTPUT);
        pinMode(potpin,INPUT);
    }

    void loop () {
        int val;
        int dval;
        int bitval;

        val = analogRead(potpin);
        dval = val/171; // normalizing factor-->adjust this to get the range you want

        Serial.print("From Pot: ");
        Serial.println(val);
        Serial.print("Decimal Value Conversion: ");
        Serial.println(dval);

        //use bit ops to get each bit!
        bitval = dval & 1;
        digitalWrite(11,bitval); // signal C

        dval = dval >> 1;
        bitval = dval & 1;
        digitalWrite(12,bitval); // signal B

        dval = dval >> 1;
        bitval = dval & 1;
        digitalWrite(13,bitval); // signal A

        delay(WAIT);
    }

Looking at the code, we see that the most significant bit of dval corresponds to pin 11, while pin 13 is the least significant.
The Potentiometer now should be able to adjust the value displayed on the Arduino IDE's terminal, which ranges from 0 to 5 as the knob goes from left to right. As the Arduino and Potentiometer are working correctly, we are ready to build our combinational circuit for the LED display. There is a Logisim construction attached below to confirm that our expressions light up the LEDs correctly.


### LED A (number that its on, simplified expression, gates used, wiring steps, any reused output, testing) ... Utsav

### LED B 

### LED C

### LED D

### LED E

### LED F

### LED G

## Conclusion