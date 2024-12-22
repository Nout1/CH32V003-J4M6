# Traffic-Light
Tiny, cheap 2 way traffic light, using 4, 3 or even only 2 GPIOs demonstrated on CH32V003-J4M6


Many schematics for traffic lights have been published, often using an arduino or 555 timer chips.
The traffic light presented here differenciates by its simplicity, very small footprint and a very low cost.
Still it's a real 2-way traffic light using a red, yellow and green LED for each light.
Limitation: it work on 3.3V up to 5V

The trick is to exploid the fact that LEDs conduct current in one direction, similar like diodes do
and the fact that the reverse breakdown for most LEDs well exceeds 5V.
The circuit can be driven by any MCU including arduino, ESP, attiny, pic etc..., but here demonstrated on the very cheap CH32V003-J4M6 using the MountRiver IDE

Three versions will be presented:
* Option1, using 4 GPIOs offering more freedoms
* Option2, an optimization of option 1 using 3 GPIOs at the cost of adding 1 PMOS
* Option3, a minimal solution only using 2 GPIOs at the cost of adding 1 PMOS + 1 NMOS

Code is written for CH32V003-J4M6, but can be easily adopted towards other MCU
