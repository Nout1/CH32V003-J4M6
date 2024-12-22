# Easy, small, cheap Traffic-Light
Tiny, cheap 2-way traffic light, using 4, 3 or even only 2 GPIOs demonstrated on CH32V003-J4M6

Many schematics for traffic lights have been published, often using an arduino or 555 timer chips.
The traffic light presented here differenciates by its simplicity, very small footprint and a very low cost.
Still it's a real 2-way traffic light using a red, yellow and green LED for each light.
Limitation: it work on 3.3V up to 5V

Three versions will be presented:
* Option1, using 4 GPIOs offering more freedoms
* Option2, an optimization of option 1 using 3 GPIOs at the cost of adding 1 NMOS
* Option3, a minimal solution only using 2 GPIOs at the cost of adding 1 NMOS + 1 PMOS

All schematics use the very cheap CH32V003-J4M6 but in fact any other MCU including arduino, ESP, attiny, pic etc... could be used
The code was written using the MountRiver IDE

# How it works:

The trick used here is to exploid the fact that LEDs conduct current in one direction, similar like diodes do.
And the fact that the reverse breakdown for most LEDs well exceeds 5V, so we can reverse the polarity without damaging the LEDs

The light sequence over time, for both traffic lights, as well as the timing diagram are shown below.
For example the first ligth eluminates 16 cycles red, followed by 12 cycles green and 3 cycles yellow (LEDs called RED1, GR1 and YEL1)
The second light does the same but is 16 cycles shifted in time. (LEDs called RED2, GR2 and YEL2)
Notice that we have made the signals to drive green and yellew LEDs 100% independent. 
This allows us to choose if yellow leds should be "static" or for example "blink". In the timing diagram a blinking yellow LED is assumed.

![image](https://github.com/user-attachments/assets/54dc4336-8bfd-4ae0-b32c-e769c1f5cb1c)

This timing diagram could be directly implemented into code by using 6 GPIOs. Our goal is to lower the overall footprint, cost and number of used GPIOs, so that 8 pin MCUs like ATTINY13 or CH32V003-J4M6 can be used.

# Option 1: 4 GPIOs, Schematic and function description:

In the schematic below, the 6 signals have been optimized to only 4 signals, still driving all the LEDs and without adding any additional hardware.
Notice that only 3 resistors are required. This is possible because LEDs of the same color never conduct at the same time **and** have a very similar forward voltage, so we can use a common resisor for them.

![image](https://github.com/user-attachments/assets/892a00ff-bb77-4b90-aca6-3b825df8fe5a)

The timing diagram for the 4 signals is given below. 

![image](https://github.com/user-attachments/assets/2b25a604-bf16-4c48-b3c0-2a1af110bca6)

The RED and INV(RED) signals directy drive the RED1 and RED2 LEDs. (note: INV(RED) is the inverted signal of RED)
Only if INV(RED) is LOW, GR1 or YEL1 LEDs can conduct
Only if RED is LOW, GR2 or YEL2 LEDs can conduct
So we can combine both GREEN signals as one signal "GR1orGR2"
and we can combine both YELLOW signals as one signal "YEL1orYEL2"
Still the YELLOW LEDs can be "static" or "blink" without affecting the GREEN LEDs
If you want to minimize wires to each traffic light, consider to place each LED + it's dedicated shunt resistor into the light rather then on the controller. 
This helps to lower the number of wires to each light and reuslts in:
* Trafic light 1 (3 LEDs combined): 4 signals: INV(RED), RED, YEL1orYEL2, GR1orGR2
* Trafic light 2 (3 LEDs combined): 4 signals: the same wires with INV(RED) and RED swapped, which can be done at the connector side. Great!

# Option 2: optimization to use 3 GPIOs, Schematic and function description:
Since all yellow and green LEDs only "source" current to RED or INV(RED), we can replace INV(RED) by a single NMOS (I used a low cost A09T NMOS)
See the optimized schematic below only using 3 signals by adding 1 NMOS
RED1 is now connected from RED to GND and RED2 from 5V to RED
Since YEL1orYEL2 and GR1orGR2 remain independent, the yellow LEDs can still be "static" or "blinking" without affecting the GREEN LEDs
For the wiring, this still uses only 4 wires for each trafic light
* Trafic light 1 (3 LEDs combined): 4 signals: 5V, RED, YEL1orYEL2, GR1orGR2
* Trafic light 2 (3 LEDs combined): 4 signals: GND, RED, YEL1orYEL2, GR1orGR2. Nearly identical, great!

![image](https://github.com/user-attachments/assets/8fa2bb8b-fbd3-4763-93f2-14c9fce2cc37)

# Option 3: optimization to only use 2 GPIOs, Schematic and function description:
Since "YEL1orYEL2" is equal to INV("GR1orGR2") and only sources current to the LEDs, we can also omit "YEL1orYEL2" and replace it by a PNMOS controlled by "GR1orGR2" 
Since  the signal to drive YELLOW and GREEN LEDs now no longer is independt, we now loose the option to have a blinking YELLOW LED not affecting the GREEN LEDs.
See the schematic below optimized to only use 2 signals, by adding 1 NMOS and 1 PMOS
For the wiring the trafic lights you pay a penalty
* Traffic light1: 6 wires: 5V, GND, RED, INV(RED), GR1orGr2, Yel1orYel2
* Traffic light2: 5 wires: GND, RED, INV(RED), GR1orGr2, Yel1orYel2

![image](https://github.com/user-attachments/assets/7e694705-087a-47ed-a941-166733ece35b)

One more trick can help to optimize the wiring. We eliminate GND from trafic light 1 by connecting the RED1 LED as depicted below
In that way both trafic lights need 5 wires
* Traffic light1: 5 wires: 5V, RED, INV(RED), GR1orGr2, Yel1orYel2
* Traffic light2: 5 wires: GND, RED, INV(RED), GR1orGr2, Yel1orYel2

![image](https://github.com/user-attachments/assets/5be2b290-3d92-4d49-937e-39a3e33a8a57)

# Dimensioning and building the circuit:

**Resistors: **The GPIOs on CH32V003 can sink and source each 8mA (to stay in spec) and max 20mA (to not damage).
With a 5V supply I have used 2.2K serial resistors for RED LEDs, 1K for GREEN LEDs and 1.2K for YELLOW LEDs
Driving LEDs with 2...4mA if often sufficient to get enough brightness for hobby purposes.
If this is insuffient, additional buffering can be applied.

NMOS and PMOS: 
* Any 5V capable NMOS with VTH below 3V can be used. I've used A09T, a small, cheap SMD type
* Any 5V capable PMOS with abs(VTH) below 3V can be used. I've used A19T, a small, cheap SMD type

Given the low complexity, I've build the circuit on a breadboard
And used some 3D printed traffic lights

# Code:

The timing diagram for all 3 cases is identical. 
In code we just need to omit 0,1 or 2 lines that define the 4 output signals
