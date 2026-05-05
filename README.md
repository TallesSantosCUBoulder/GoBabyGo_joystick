# GoBabyGo Joystick-Controlled Car
**ENGR140: First Year Engineering Projects - Colorado Mesa University**  
**Sponsor:** Talles Santos  
**Students:** Michael Riley (Civil Engineering)  
&emsp;&emsp;&emsp;&emsp;&emsp;Francisco Cuen (Mechanical Engineering)  
&emsp;&emsp;&emsp;&emsp;&emsp;Jaclyn Pellegrini (Mechanical Engineering)  
&emsp;&emsp;&emsp;&emsp;&emsp;Caleb Kasayka (Mechanical Engineering)  

## Introduction ##
“GoBabyGo” is a non-profit organization dedicated to providing children with limited mobility the opportunity to play independently by adapting motorized toy cars. Many children with mobility challenges struggle to press the pedal and steer simultaneously, making it difficult to operate the vehicle without constant assistance. Typical “GoBabyGo” modifications reroute the pedal wiring to a button mounted on the steering wheel, simplifying acceleration and improving usability.  

After multiple iterations, the final design operated successfully and remained within budget. It reliably controlled forward/backward motion and steering, and testing showed consistent performance with minimal drift. The system was safe, intuitive, and easy for children to use. Future work should focus on simplifying the steering circuit and adapting the design for broader use across different ride‑on car models to benefit more children.  

![Image of the Completed Car](Images/CompletedCar.png)  
*The completed modified car*  

## Project Overview
While the standard modifications help many children, some still find it difficult to press a button and steer at the same time. This project aims to create a more intuitive control system by replacing both the pedal and steering wheel with a single joystick. The joystick must control forward and backward motion as well as left and right steering, operate safely under 12 V and 3 A, and be implemented for under $200, as well as:
* Directional Control
    * Provide forward, reverse, left, and right motion; each direction successfully actuates correct motor behavior 10/10 trials with a response rate of .5 seconds. 
* Consistent Steering & Speed Response
    * Vehicle follows the same path in 5 repeated trials within acceptable tolerance (e.g., ±6 inches lateral drift over 10-15ft).
* 12V Maximum Power
    * Must not exceed 12V thought out the electrical system.
* Dashboard Fit
    * Total joystick assembly dimensions must not exceed 2.75 in (height) × 3.125 in (width). Zero interference points and no natural obstructions.
* Heat Limit
    * Must not exceed 113° over 15 minute time intervals.

### Design Requirements

###  Electrical Solutions
For this car, a modification was made for the movement. Using a joystick instead of a steering wheel and foot-powered pedals, the car can move forwards and backwards without restrictions, but turning left and right is altered. Instead of turning left and right without restriction, moving or holding the joystick in either direction causes the car to turn for only a set amount of time before stopping and waiting for another input. This is to help keep the motors from stalling as ... ? (Include a video/gif of the car moving in all four directions with the joystick)  
To implement the timed movement for turning left and right, a 555 circuit was created and added to each of the direction inputs.  


![555 Timer Circuit Diagram](Images/555CarCircuit.png)  
*555 Timer Circuit Diagram*  
&emsp;  
The combination of resistors and capacitors makes the five volt pulse from the timer last for only sec second before cutting off regardless of whether the joystick is still held down, resulting in the short burst of turning that the car experiences.  

![Full Car Circuit Diagram](Images/PCBSchematic.png)  
*The Full Circuit Diagram of the car*  
&emsp;  

![Full Car PCB Model](Images/PCBCircuitModel.png)  
*The Full PCB Model of the car*  
&emsp;  

The full construction of the circuit requires the following components:
* [3 full BTS7960 High Current H-Bridge Motor Drivers](/Datasheets/BTS7960%20Motor%20Driver.pdf)
    * When an input is received, it sends a signal to the H-bridge motor controller which recognizes the voltage as a directional input and then outputs that signal into the motor itself. This allows for safe, effective, bi-directional movement. The H-bridge has a total of 12 attachment points. two for power in, two for power to motor, two for input detection (seen as “IN” in Appendix F), two for inhibiting direction (“INH” in Appendix F, two for ISs (seen in Appendix F), a ground for the logic board (GND in Appendix F), and a 5.5V input for the logic board (“VCC” in Appendix F). This design will take advantage of all the inputs / outputs except for the SIs. Full representation of the function of an H-Bridge Controller can be found in Appendix F.
* [2 LM555 Timer Chips](/Datasheets/lm555.pdf)
    * A 555 timer is an integrated circuit that takes an electrical input and outputs a pulse over a certain interval of time. [4] This component is needed to turn the front steering motor to a certain point so that the motor doesn’t overturn the mechanical component, breaking the car, and burning out the motor. Power is inputted through the VCC pin which powers the circuit (as seen in Appendix G). When a signal comes through the Trigger pin, it goes through the circuit within the 555 and then a capacitor and resistor attached to the discharge and threshold pins which controls how long a pulse is sent out of the output pin. For this project, the 555 is operated in monostable mode, meaning it sends out a singular pulse and then returns to its original state. To get the pulse to last for a certain time, the equation T=1.1RC where T is time, R is the strength of the resistor, and C is the value of the capacitor. For the motor a 1 second pulse is needed so values of a100kΩ resistor and a 10µF capacitor are needed. [5]
* [A Joystick](/Datasheets/SANWA%20JLF-TP-8YT%20Joystick%20Instruction%20Manual.pdf)
    * The joystick has five wires with a gauge of 23 AWG (American Wire Gauge). These wires are connected to a circuit board via a connector. This system consists of four signal wires, each for a different switch that is also connected to the circuit board. The last wire serves as a 5v-12v source wire; the input (5v-12v) will flow through this wire and in return, will output through one of the four switches (possibly two) depending on which switch is compressed or activated. Appendix D illustrates the internal switching logic of the joystick.
* [An RC Car](/Datasheets/BCP%20Sky908%20User%20Manual.pdf)
    * For this project an existing car was bought and provided to the group, but the car model used costs around $250 USD.
* [2 NPN Bipolar Junction Transistors](/Datasheets/2n2222a.pdf)
    * The transistor are the necessary part of the timing circuit. For the 555 to properly work, it needs sharp, high input. This is not as simple as it would seem because our joystick is mainly used in a “toggle” sense, where the driver will hold it left to turn left. After some testing, we realized that this toggled system would lead to the output of the 555 system always being held high. To fix this, we implemented what is called a pulse shaping module. This consists mainly of a capacitor momentarily allowing a transistor to drain a power source to ground, creating a quick pulse of electricity that the 555 timers can properly understand. Appendix H shows the pulse shaping module implemented into the design of the circuit. Appendix I contains a picture of all components from 5.2.5 to 5.2.6 on the working PCB.
* [Joystick Mount](/Datasheets/PutLinkHere.webp)
    * The joystick already has two existing mounting holes, 0.278 inches in diameter, implemented into the factory design. With this, a CAD drawing was produced and integrated with these existing holes (see Appendix E). The design functions similar to a washer where it will mount on the original steering wheel location. Two bolts will run through the top (the side where the joystick extrudes), and below there will be two nuts securing the simple assembly in place. Appendix E consists of a SolidWorks drawing of the mount.
* A 12V to 5V Voltage Step Down
    * Must be able to handle 3A of current and 15W of power.
* 6 100k Ohm Resistors
* 4 10k Ohm Resistors
* 4 10uF Capacitors
* A 12V Battery
    * The model of car used includes the 12V 7AH battery.

The circuit starts at the battery going into the two gearboxed motors of the car and the voltage step down. The 5V from the stepdown powers the rest of the system: all three of the motor drivers and the two 555 timer circuits. While the forward and backwards motor drivers are only activated with the direct signal from the joystick, the left and right one first goes through there respective timing circuit to give the proper timed signal.

## Results

## Recommentdations and Next Steps
Must not exceed 113° over 15 minute time intervals.For general use, it is recommended that:
    • The driver does not exceed 77lbs [6]
    • The car avoids liquid 
    • The car is not driven in the rain	 
    • The car avoids sharp drops offs like curbs

        8.2. Recommended next steps  
Firstly, the telecontrol should be reattached. This feature was initially implemented into the car for safety, but the snow munchers did not have time to fully integrate this. To re-implement this feature, a second PCB would have to be printed to allow for toggleable control between the IBT-2 motor drivers and the standard relay. This could be done most simplistically with 4 transistors for each motor. Two to allow for flow from the telecontrol relay and the other two to allow for flow from the joystick. The second reintegration should be rewiring the lights. The old lights are already attached and ready to run, however they need to be wired to a new power to turn on with the car. Similarly, the sound system can be easily repaired; however, the old speaker was damaged when removed, so a replacement would need to be purchased and reinstalled inside the dashboard.

## How to Operate