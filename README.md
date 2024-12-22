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

The light sequence over time for both traffic lights is shown below.
For example 16 cycles Red, followed by 12 cycles Green and 3 cycles yellow.
The second light does the same but is 16 cycles shifted in time.

![image](https://github.com/user-attachments/assets/64e99518-fd01-41a9-ab39-a81e0b8819c6)

Converted into a timing diagram we get

![image](https://github.com/user-attachments/assets/2f71ac09-e697-48c5-bba9-104499cdf25e)

This timing diagram can be directly implementd into code by using 6 GPIOs, but our goal is to save GPIOs and if possible lower overall cost.

# Option 1: Schematic and function description:

In the schematic below the 6 signals have been optimized to only 4 signals to still drive all LEDs and without adding any additional hardware.

![image](https://github.com/user-attachments/assets/892a00ff-bb77-4b90-aca6-3b825df8fe5a)


