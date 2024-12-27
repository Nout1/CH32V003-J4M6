# Introduction
Flickering flames, based on a LED, are popular to simulate a fireplace, a tourch or a candle.
Personally I've used a flickering flame to simulate a fireplace in a doll house as shown below.
The 3D model comes from Akoffeman, [thingiverse](https://www.thingiverse.com/thing:4503449)



# Steps to achieve linear brightness perception:
Flickering is more than just a random generator controlling the pulse-time of a PWM signal.  
That concept would completely ignore the physical reality.
**First:** The human eye perceives brightness logarithmically, not linear. 
So to make a LED appear to change its brightness linearly, the duty cycle of the PWM needs to be adjusted to the brightness raised to a power, usually around 2.2
For simplicity we use a power of 2 (=quadratic).
**Second:** Flicking does not vary from fully on to fully off but makes more limited brightness variations.

To emulate these behavious, a 3 step approach was used.
First we generate a random number between -8 and +8 and add this to the previous value of the flickering.
In this way flickering can only make a limited change in brightness within 1 time step.
Second we limit the low value of the flickering to 16 so it never can fully shut down the light. We also limit the high value to 63, which will corresponds to maximum brightness.
Third we convert the generated flicker value into a brightness by applying a scaling and a power law 2 on it. In this way our eyes will see all variations as linear variations.
See the code snipped to do all this. 

