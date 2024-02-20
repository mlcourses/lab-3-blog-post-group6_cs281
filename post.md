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


### LED A (number that its on, simplified expression, gates used, wiring steps, any reused output, testing)

To make LED A light up when needed, We utilized a combination of XOR, NOT, and OR gates. Spicifically, we minimized the SOP expression needed to light up LED A. We made this expression simpler by using K-Maps. Before even focusing on LED A, We created a LED functionality table with Values from 0 to 5 (the numbers we wanted to light up in LED using Potentiometer). In the table, we had three inputs, B2, B1, B0. Since we have three inputs, our table extended from 0 to 7 (2^n) but we only needed upto 5 so, we didn't care much about 6 and 7. After using our table and SOP/K-Map expressions, We came to a conclusion that to light the number 1(A), we needed to light up LED A. 

Our SOP Expression for LEDA is (~B2)(~B1)(~B0) + (~B2)(B1)(~B0)+ (~B2)(B1)(B0) + (B2)(~B1)(B0). We minimized this expression using K-Maps for efficiency. The K-Map minimal expression for LEDA is ~(B0 XOR B2)+B1. After this, we wired the bredboard using 2 gates, XOR gate, NOT gate and OR gate.


After building it, we tested the correctness of this. With LED A wired correctly, LED A light up. This testing process involved systematically checking different input combinations and verifying the resulting LED illumination, thereby validating the functionality of LED A and Circuit design for it. 


  [Vid of LED A Working](https://youtube.com/shorts/AdsHs8C6BbE?feature=share)


### LED B Utsav

To light up LED B, we selected two NOT gates, an AND gate and an OR gate. To determine what gates we need to light up LED B, we minimized our SOP expression of (~B2)(~B1)(~B0) + (~B2)(~B1) B0 + (~B2)(B1)(~B0) + (~B2)(B1)B0 + (B2)(~B1)~(B0) into (~B2)+(~B0)B2 using K-Maps. After deterning the necessary gates, we connected them according to the minimized expression. This helped us a lot in terms of efficiency since it simplified the logic and also reduced the number of gates needed to control LED B effectively. The inputs B2, B1, B0 were routed appropriately to the inputs of the gates. With these connected properly, including the IC chips being powered up properly (Vcc, and GND), LED B light up as expected.

  
  [vid of LED B Working](https://youtube.com/shorts/woT4dJWxtGY?feature=share)

  After building it, we tested the correctness of this. With LED B wired correctly, LED B light up. This testing process involved systematically checking different input combinations and verifying the resulting LED illumination as expected according to the minimized expression, thereby validating the functionality of LED B and Circuit design for it. 

### LED C Utsav

To light up LED C, We selected a NOT gate, an AND gate and an OR gate. This was derived from our K-Maps which was minimized from the SOP expression. The SOP expression was (~B2)(~B1)(~B0) + (~B2)(~B1)(B0) + (~B2)(B1)(B0) + (B2)(~B1)(~B0) + (B2)(~B1)(B0). Our minimized expression that we derived from this was (~B1)+(B1.B0). This helped us a lot in terms of efficiency since it simplified the logic and also reduced the number of gates needed to control LED C effectively.

After determining the necessary gates (IC chips), we connected them accordingly with our K-Maps, ensuring proper routing of inputs and outputs. As necessary, we powered up the IC chips with Vcc and Gnd for a reliable operation.


  [vid of LED C Working](https://youtube.com/shorts/U26imPzQ8zQ?feature=share)

To verify the functionality of the circuit, we conducted thorough testing by applying different input combinations and observing the illumination of LED C. We ensured that it light up as expected for the desired input values to conform the accuracy of our circuit design.

### LED D

To light up LED D, We used two NOT gates, one OR gate and one XOR gate. The SOP expression that we used for this is (~B2)(~B1)(~B0) + (~B2)(B1)(~B0) + (~B2)(B1)(B0) + (B2)(~B1)(B0). We used K-Maps to simplify this into (~(B2) XOR ~(B1)) + B1. This helped us a lot in terms of efficiency since it simplified the logic and also reduced the number of gates needed to control LED D effectively. 

After determining the necessary gates (IC chips), we connected them accordingly with our K-Maps, ensuring proper routing of inputs and outputs. As necessary, we powered up the IC chips with Vcc and Gnd for a reliable operation.

  [Vid of LED D working](https://youtube.com/shorts/KXEbo0dtIIU?feature=share)

### LED E

### LED F

### LED G

## Conclusion