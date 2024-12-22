# Easy, small, cheap Traffic-Light
Tiny, cheap 2 way traffic light, using 4, 3 or even only 2 GPIOs demonstrated on CH32V003-J4M6


Many schematics for traffic lights have been published, often using an arduino or 555 timer chips.
The traffic light presented here differenciates by its simplicity, very small footprint and a very low cost.
Still it's a real 2-way traffic light using a red, yellow and green LED for each light.
Limitation: it work on 3.3V up to 5V

Three versions will be presented:
* Option1, using 4 GPIOs offering more freedoms
* Option2, an optimization of option 1 using 3 GPIOs at the cost of adding 1 PMOS
* Option3, a minimal solution only using 2 GPIOs at the cost of adding 1 PMOS + 1 NMOS

All schematics use the very cheap CH32V003-J4M6 but in fact any other MCU including arduino, ESP, attiny, pic etc... could be used
The code was written using the MountRiver IDE

# How it works:

The trick used here is to exploid the fact that LEDs conduct current in one direction, similar like diodes do.
And the fact that the reverse breakdown for most LEDs well exceeds 5V, so we can reverse the polarity without damaging the LEDs

The light sequence for the traffic light is shown below.
Red, Green and yellow aread for both traffic light are depicted over time
![image](https://github.com/user-attachments/assets/263bb37b-7b26-48a8-ae67-b0aaaebb1484)

Converted into a timing diagram we get
![image](https://github.com/user-attachments/assets/2f71ac09-e697-48c5-bba9-104499cdf25e)

# Option 1: Schematic and function description:

The 6 signals can be optimized to only 4 signals and still drive all LEDs (without additional hardware) using the schematic below

![image](https://github.com/user-attachments/assets/892a00ff-bb77-4b90-aca6-3b825df8fe5a)


