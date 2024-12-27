# Introduction
Flickering flames, based on a LED, are popular to simulate a fireplace, a tourch or a candle.
Personally I've used a flickering flame to simulate a fireplace in a doll house as shown below.
The 3D model comes from Akoffeman, [thingiverse](https://www.thingiverse.com/thing:4503449)



# Steps to achieve linear brightness perception:
Flickering is more than just a random generator controlling the pulse-time of a PWM signal.  
That concept would completely ignore the physical reality.

**First:** The human eye perceives brightness logarithmically, not linear. 
So to make a LED appear to change its brightness linearly, the duty cycle of the PWM needs to be adjusted to the brightness raised to a power, usually around 2.2.

For simplicity we will use a power of 2 (=quadratic).

**Second:** Flicking does not vary from fully on to fully off but makes more limited brightness variations.

To emulate these behavious, a 3 step approach was used.
First we generate a random number between -8 and +8 and add this to the previous value of the flickering.
In this way flickering can only make a limited change in brightness within 1 time step.
Second we limit the low value of the flickering to 16 so it never can fully shut down the light. We also limit the high value to 63, which will corresponds to maximum brightness.
Third we convert the generated flicker value into a brightness by applying a scaling and a power law 2 on it. In this way our eyes will see all variations as linear variations.

See the code snipped to do all this. 

# Implementation: One PWM on PC1 or add a 2nd PWM on PC2 to drive a 2nd LED.
Given we have 6 GPIOs and only one is used for a flickering LED, why not add a 2nd PWM and 2nd LED?
Both PWMs use the same timer, TIM2, set to a frequency of 200Hz with a pulse between 0 and 999 (0.1% resolution).
Both PWM are controlled by their own flicker value and each drive a LED.

# What I learned while doing this project:
Understranding remapping is essential for using the 8-pin versions of CH32V003, since many functions are multiplexed to the one pin.
As can be found in the documentation, 4 pin mappings are supported:
