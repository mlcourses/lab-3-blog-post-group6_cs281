# Lab 3: Ramp Circuit

## Overview and Motivation
This lab is an introduction to designing circuit that changes light signals on LED display by modifying the voltage values going through it. We will get to know Voltage Divider and Potentiometer. Specifically, we will use a potentiometer to change the resistance of a voltage divider, which changes the voltage going through the LED and, accordingly, changes the numbers (light signals) on the LED display. Sounds intimidating? Hang in there, this lab will be interesting...!

The lab is useful in improving our skills in writing SOP expression, simplifying Boolean expression using K-map, and using Logisim to check the correctness of our circuit. After this lab, you will be able to build combinational circuit based on the combinational logic using multiple electrical components to modify the voltage and resistance such as Voltage Divider, resistors, and Arduino. 

The complexity of this combinational circuit will also give us a chance to practice color-coding our wires and organizing our circuit. 

## Materials
Like other labs, we will need a PB-503 breadboard and an Arduino. We also need a 7-segment LED display that can show numbers from 0-5 controlled by combinational circuits.

As mentioned above, we will use Voltage Divider and Potentiometer for the circuit in this lab. First, let's see what they are:

- **The Voltage Divide**r: is an *arrangment of resistors*. The goal of building a voltage divider is to modify the voltage value going through a circuit as we want. 

[This is an example of the arrangement of resistors (creating a Voltage Divider) compared to a simple circuit without the voltage divider](https://drive.google.com/file/d/1-N5O_i_QeqHqdP0CTq7SlfFA_EzuxVC9/view)

- **The potentiometer (the "pot"):** A potentiometer is a variable resistor that allows us to adjust our voltage divider. The idea behind the way the potentiometer on voltage divider is that it changes the resistance values going through the voltage divider. Hence, we will change the voltage values based on the effect of resistance onto voltage value of a circuit, which is presented by the formula:
[Simple voltage formula](https://drive.google.com/file/d/1oqhVUFsj-R9r1wd_B5hHpJMXX-YNMIA2/view)

[Now with the voltage divider, our formula turns into](https://drive.google.com/file/d/1_H_ns-8sXkxy_MkouRRxsUaltquBEgTu/view)

- The "pot" will modify the resistance value in order to create our target voltage values for the circuit.

On our familiar breadboard, there are 2 potentiometers. The 10k potentiometer is a knob at the bottom of the breadboard. The total sum of resistance is `10kΩ`. By changing the knob, we can achieve various values for R1 and R2 between 0 and 10kΩ so long as their sum is `10kΩ`. There is also a 1k potentiometer which acts in the same way except the total resistance is `1kΩ` instead of `10kΩ`. 

That's enough theory, let's build our circuit...!

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

Looking at the code, we see that the most significant bit of dval corresponds to pin `11`, while pin `13` is the least significant.
The Potentiometer now should be able to adjust the value displayed on the Arduino IDE's terminal, which ranges from 0 to 5 as the knob goes from left to right. As the Arduino and Potentiometer are working correctly, we are ready to build our combinational circuit for the LED display.

**Important notes:** We will work with multiple resistors in this lab. The resistors are placed next to the LED segments so that the current does not overload and damage the LED. Most of them will be placed close to each other. In order to keep our current stable, make sure those resistors do not touch each other.

### The circuit on paper: 
Before jumping right into building the circuit physically, we need to understand how these wires and components interact together. We will need the truth table showing how the inputs are selected: 

[Truth table showing how inputs are selected](https://drive.google.com/file/d/1F6tPZ7Hiy9ZjbHo7ueKtPLv7WOQSMSw_/view)

We will have 3 inputs and 7 outputs represents 7 LEDs - A, B, C, D, E, F, G

After utilizing K-map to simplify the SOP expression, you are recommended to test the correctness of the circuit on Logisim before designing a real circuit. 

[This is how the circuit looks like on Logisim](https://drive.google.com/file/d/19VX75Vw6b9X0MN2GeTNPy5XCgI3wjcKC/view)

The circuit might look complicated but as logn as it follows correctly our truth table, we are good to go!

### Wire up the Voltage divider:
We will have to wire 7 LEDs in the lab. However, there is a general way for how we will wire our Voltage Diver and the potentiometer.

[General way for how we will wire our Voltage Diver and the potentiometer](https://drive.google.com/file/d/1fIGjMOBcgIS0VQIW5IXJbyNZZz6VZZty/view)

Basically, the leftmost connection column of the pot will be wired to `GND`. The rightmost column of the "pot" will be wired to `+5V`. The middle connections will pick off the voltage V depending on the position of the potentiometer knob. This is considered as output voltage. 

### LED A (number that its on, simplified expression, gates used, wiring steps, any reused output, testing)

To make LED A light up when needed, We utilized a combination of XOR, NOT, and OR gates. We minimized the SOP expression needed to light up LED A by using K-Maps. Before even focusing on LED A, we created a LED functionality table with values from 0 to 5 (the numbers we wanted to light up in LED using Potentiometer). In the table, we had three inputs, B2, B1, B0. Since we have three inputs, our table extended from 0 to 7, but we only needed upto 5 so we do not have to care about 6 and 7. Here is a general functionality table that we expanded on and used to create different SOP Expressions for LED A through LED G. We expanded this table depending on our three inputs B2, B1 and B0 and created a truth table for LED A through LED G.


Here is the picture of the General LED Functionality table.

![Basic Functionality Table](/resources/FunctionalityTable.png)


Our SOP Expression for LED A is `(~B2)(~B1)(~B0) + (~B2)(B1)(~B0)+ (~B2)(B1)(B0) + (B2)(~B1)(B0)`. We minimized this expression using K-Maps for efficiency. The K-Map minimal expression for LED A is `~(B0 XOR B2)+B1`. After this, we wired the breadboard using 2 gates, XOR gate, NOT gate and OR gate. Here are the steps taken to light up LED A:

- **XOR Operation between `B0` and `B2`:** We started the process to light up LED A by inputting `B0` and `B2` into an XOR gate using the 7486 IC Chip. One 7486 IC chip has 4 different inputs and outputs (2 inputs = 1 output per XOR gate). After this, we took the output of this XOR operation and plugged it in a NOT Gate.

- **Invert Operation of `B0 XOR B2`:** After XOR, we inverted the output of the XOR operation by inputting the output of XOR into a NOT gate i.e. 7404 IC chip. One 7404 IC chip has 6 different NOT gates (1 input = 1 output per gate). Then, we took the output of this operation and plugged them in an OR gate.

- **OR Operation between `~(B0 XOR B2)` and `B1`:** After inverting the XOR output, we took the output of our NOT gate (7404 chip) and plugged in an OR gate. We used 7432 IC chip to do the OR operation. One 7432 IC chip has 4 different OR gates i.e. 4 different inputs and outputs (2 input = 1 output per gate). Our next input in the OR opration was `B1`. Then we took the output of our OR operation and plugged them as an input to LED A. 


- **Testing:** LED A needs to be light up when our voltage value is at 0, 2, 3, and 5.


Here is a video of LED A working properly in all cases, to light up from number 0 to 5.

  [Video of LED A working](https://youtube.com/shorts/AdsHs8C6BbE?feature=share)

After building the circuit, we tested its correctness. With LED A's combinational logic wired correctly, it lights up. This testing process involved systematically checking different input combinations and verifying the resulting LED illumination, thereby validating the functionality of LED A and Circuit design for it. 


### LED B 

To light up LED B, we selected two NOT gates, an AND gate and an OR gate. To determine what gates we need to light up LED B, we minimized our SOP expression of `(~B2)(~B1)(~B0) + (~B2)(~B1) B0 + (~B2)(B1)(~B0) + (~B2)(B1)B0 + (B2)(~B1)~(B0)` into `(~B2)+(~B0)B2` using K-Maps. After determining the necessary gates, we connected them according to the minimized expression. This helped us a lot in terms of efficiency since it simplified the logic and also reduced the number of gates needed to control LED B effectively. The inputs B2, B1, B0 were routed appropriately to the inputs of the gates. With these connected and the IC chips being powered up properly (Vcc and GND), LED B lights up as expected. Steps taken to light up LED B:

- **Invert `B0`:** We started this process by inverting the input `B0`. We used a NOT gate i.e. 7404 IC chip. 

- **AND Operation between `~B0.B2`:** After inverting `B0`, we took the output of this and inputted it in an AND gate (7408 IC Chip). One 7408 IC chip has 4 different inputs and outputs i.e same chip can be used for 4 different AND operations. 

- **OR Operation with `~B2`:** Now, we again used the previous 7404 IC Chip to invert `B2` since one 7404 IC chip can work as 6 different NOT gates. Then, we took this output and inputted it in an OR Operation (7432 IC chip). The second input for the OR operation was the output of AND operation between `~B0.B2`. After this, we took the output of this operation and connected this into our LED B to light it up.


- **Testing:**LED B needs to be light up when our voltage value is at 0, 1, 2, 3 and 4.

Here is a video of LED B working properly in all cases, lighting up from number 0 to 5.
  [vid of LED B Working](https://youtube.com/shorts/woT4dJWxtGY?feature=share)

After building the circuit, we tested to see if it's working as expected. With LED B wired correctly, LED B lights up. This testing process involved systematically checking different input combinations and verifying the resulting LED illumination as expected according to the minimized expression, thereby validating the functionality of LED B and Circuit design for it. 

### LED C 

To light up LED C, We selected a NOT gate, an AND gate and an OR gate. This was derived from our K-Maps which was minimized from the SOP expression. The SOP expression was `(~B2)(~B1)(~B0) + (~B2)(~B1)(B0) + (~B2)(B1)(B0) + (B2)(~B1)(~B0) + (B2)(~B1)(B0)`. Our minimized expression that we derived from this was `(~B1)+(B1.B0)`. Just as LED B, this simplified the logic and also reduced the number of gates needed to control LED C, thereby making the process more efficient. Here are the steps taken to light up LED C:

- **AND Operation for `B1` and `B0`:** To light up LED C, we started our wiring process by taking our two inputs `B1` and `B0` and inputting them in an AND gate i.e. (7408 IC Chip). One 7408 IC chip has 4 different inputs and outputs meaning one 7408 chip can be used as 4 different AND gates for 4 different operations. We only needed one for this step.

- **Invert `B1`:** After the AND operation, we Inverted our `B1` using a NOT gate (7404 IC chip). Then, we took the output of this and used in an OR operation.

- **OR Operation between `~B1` and output of `B1.B0`:** According to our K-Map, after inverting `B1` and having an output of `B1.B0`, we inputted our output of AND operation and output of our NOT gate into an OR gate (7432 IC Chip). Then, we took the output of this operation and connected it to our LED C and light up LED C.

- **Testing:** LED C needs to be light up and in order to show the number 0, 1, 3, 4, and 5.

After determining the necessary gates (IC chips) and executing the wiring process, we connected them accordingly with our K-Maps, ensuring proper routing of inputs and outputs. As always, we powered up the IC chips with Vcc and Gnd for a reliable operation.

Here is a video of LED C working properly in all cases, lighting up from number 0 to 5.
  [vid of LED C Working](https://youtube.com/shorts/U26imPzQ8zQ?feature=share)

To verify the functionality of the circuit, we conducted thorough testing by applying different input combinations and observing the illumination of LED C. We ensured that it lights up as expected for the desired input values to confirm the accuracy of our circuit design.

### LED D

To light up LED D, We used two NOT gates, one OR gate and one XOR gate. The SOP expression that we used for this is `(~B2)(~B1)(~B0) + (~B2)(B1)(~B0) + (~B2)(B1)(B0) + (B2)(~B1)(B0)`. We used K-Maps to simplify this into `(~(B2) XOR ~(B1)) + B1`. Once again, this helped us a lot in terms of efficiency since it simplified the logic and also reduced the number of gates needed to control LED D effectively. Steps on how to connect them:

- **Invert `B2` and `B1`:** To light up LED D, we first start with two NOT gates (7404 IC Chip) inverting the inputs `B2` and `B1` by inputting them with into two NOT Gates. One 7404 Chip has 6 diffferent NOT Gates. For this operation, we only needed two NOT Gates for two inputs `B2` and `B1`.

- **XOR `~B2` and `~B1`:** After inverting `B2` and `B1`, we take the outputs and plug them in a XOR gate (7486 IC Chip). One 7486 IC Chip has upto 4 different inputs and outputs (two inputs to one output). 

- **OR with `B1`:** Now, we take the output of our XOR operation and input this in an OR gate (7432 IC Chip). Similar to an XOR gate, an OR gate also has 4 different inputs and outputs. Now, we finally completed our K-Map expression. After this, we simply take the output of our operation and connect it with our LED D input.

- **Testing:** LED D needs to be light up and in order to show the number 0, 2, 3, and 5. 


After determining the necessary gates (IC chips) and managing the wiring process, we connected them accordingly with our K-Maps, ensuring proper routing of inputs and outputs. Again, we powered up the IC chips with Vcc and Gnd for a reliable operation.

Here is a video of LED D working properly in all cases, to light up from number 0 to 5.
  [Vid of LED D working](https://youtube.com/shorts/KXEbo0dtIIU?feature=share)

After building the circuit for D, we tested its correctness. With LED D's combinational logic wired correctly, it lights up. This testing process involved systematically checking different input combinations and verifying the resulting LED illumination as expected according to the minimized expression, thereby validating the functionality of LED D and Circuit design for it. 

### LED E (Vuong)

Now we will continue to build the circuit that lights up LED E. This one has a much simpler circuit than the previous ones. The boolean expression for LED E is `(~B2)(~B0)`. See that we need the negation of the 2 inputs. Follow the instructions: 

- **Negate `B0` and `B2`:** First, we wire one hole on the same role with `B2` with an input pin on the Inverter gate (7404 IC chip) to negate `B2`. Similarly, wire a hole on the same role with `B0` to another inverter input pin on the 7404 (inverter) chip. 

- **Connect 2 outputs `~B0` and `~B2` together:** Choose a 2-input AND gate on the 7400 AND chip.  Wire the output pin of `~B2` on the 7404 INVERTER chip and wire with 1 input pin on the AND 7400 chip. Next, wire the output pin of `~B0` on the same 7404 chip and connect it to the other input pin. We're nearly done with our circuit for the LED E. Now we only need to connect the output of the circuit to the LED.

- **Connect the circuit to the 7-segment:** Now, wire the output of the AND gate we just built to an empty row on the breadboard, near the 7-segment. Take a resistor, wire one end of the resistor with the output of the circuit we just built. Connect the other end to the pin E on the 7-segment. Now, we're done with our circuit for LED E. 

- **Testing:** Due to the position of LED E, it should lights up when our Volt value is at 0 and 2. See the video to understand how it works.

[Video showing how LED lights up in the circuit](https://drive.google.com/file/d/1Lqr9JXy8fbRZOmIBiVwSRJ_zQhbCa_DC/view?t%253D5)


### LED F (Vuong)
Now let's proceed to LED F. If you're run out of holes that are in the same row with any output, you can create more connected holes!. Take a wire, connect one end of it with a hole on the same row with any input `B0, B1,B2` that don't have any holes left. Connect the other end with another empty row on the breadboard, close to that input orginal row. Now, you have any rows connected to that input. Now, let's build LED F (with boolean expression `B2 + (~B0)(~B1)`) circuit step by step:

- **Build `(~B0)(~B1)`:** First, we negate `B0`. Remember from the previous circuit of LED E we just negated `B0`. We will use that output to avoid making our circuit overly complicated. On the row having the output pin of `~B0`, connect a wire from that hole to one of the input pin on a 2-input AND gate of the 7400 AND chip. Do you remember that we also had `~B1` built in the LED C's circuit. Let's reuse that. Similarly to `~B0`, wire the output of `~B1` with the other input pin of the AND gate. We're done building `(~B0)(~B1)`!

- **Build `B2 + (~B0)(~B1)`:** Wire the output of `(~B0)(~B1)` to one input pin of a 2-input OR gate on the 7432 OR chip. Wire `B2` with the other input pin. 

- **Connect with the LED F:** Wire the output of the OR gate to an empty row on the breadboard (let's choose one near the 7-segment). Take a resistor. Connect one end of the resistor to the row that we just pinned the output of circuit in. Connect the other end of the resistor to the F pin on the 7-segment. And... we're done building circuit for LED F.

- **Testing:** Due to the position of LED F, if we have our circuit correct, LED F will light up when the Volt out value of our circuit is at 0, 4, 5. See the video to understand how it works. 

[Video showing how LED F works](https://drive.google.com/file/d/14bWkAK4fRdiGQ1Mi1y7thp1DeyaHTyhj/view?t%253D3)


### LED G (Vuong)
Let's roll to our last LED, LED G. This one is simple. The boolean expression for this one is `B1 + B2`:

- **Wire `B1+B2`:** Wire `B1` to one input pin of the 2-input OR gate on the 7432 OR chip. Wire `B2` to the other input pin of the OR gate. 

- **Connect the output with the LED:** Wire output of the OR gate to another empty row on the breadboard near the 7-segment. Take another resistor. Connect one end of it to the wire connected to the `B1+B2` we just built (connect it to one hole on the row the wire is pinned on). Connect the other end of the resistor to the G pin on the 7-segment. 

- **Testing:** Because LED G is in the middle of the LED board, it should light up when our Volt value is at 2, 3, 4, 5. The video below describes how the LED G works. 

[Video showing how LED F works](https://drive.google.com/file/d/1cngTwDoqp9G6lHq0j2LeBOV3rAf0gvH7/view?t%253D2)


## Conclusion (Vuong's still working on it)
This lab is an opportunity to learn how to desgin a complete combinational circuit that allows us to modify the Voltage values to achieve the result we want to see. To sum up, we have worked with several topcis/electrical components in this lab:

- **SOP expression and how to use K-map in designing circuit:** This lab gives you a chance to use K-map to simplify SOP expression which is useful for building circuit. Utilizing K-map makes the circuit much simplified and easy to physically wire. 

- **7-segment display**: We're used to the logic probe on our breadboard. The 7-segment display is like a combination of all LEDs on our logic probe that has its LED arranged so that it can send out numerical signals. Just like its name, a 7-segment display has 7 pins where we can wire and connect our circuit to. Each pin takes in an input signal that will light up the LED at that corner. All 7 LEDs (represent 7 pins) form a number ranging from 0 to 9 (we only generate 0-5 signal in this lab). The 7-segment display is used as a Voltage indicator in this lab. 

- **Voltage Divider:** We used Analog Voltage Divider to modify the voltage value going through our LEDs. Due to the way we wired our circuit (according to our input signals), we can control the signal of the 7-segment display by modifying our Analog Voltage Divider. 

- **Potentiometer:** or "pot" is used to modify the resistance of the voltage divider.  

- **Use resistors:** Different from other labs, we use more resistors in this lab to modify the voltage values. Now, you see how we can use a small device to modify the light signals as we want!

- **Practice putting everything together!** Designing, writing and simplifying SOP expressions are only some parts of the work building a computer system. To successfully designing and building a system, we also need to practice wiring and working with complicated physical circuit. Due to the more complex design (compared to other previous labs) of the circuit in this lab, we are given a chacne to practice arranging multiple components on the breadboard and color-coding our circuit. Keeping our circuit organized and neat is an important step for debugging later. 
