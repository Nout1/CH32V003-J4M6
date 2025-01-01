# Introduction: non-linear servo movement

Many applications use servo motors since they only need one PWM signal and 1 GPIO to control them.
The CH32V003-J4M6 MCU a low cost choice to let a servo rotate.
Most servos are controlled in a linear fashion but Eg the movement of a head, hand etc... are not linear.
Instead they accellerate fast, move, and finally slow down the closer the final position is reached.

# How it works + calculating the PWM settings
Controlling a servo is done by changing the pwm signal. For this demo the popular sg9 servo will be used.
While the frequency for this servo it typically 50Hz and remains fixed, we need to vary the pulse time the pwm signal is high.
The sg9 needs a pulse = 1.5ms to set angle to 0 degree and 2.5ms to set angle to 180 degree.
ch32v003 operates by default with a 48Mhz clock.
If we want the PWM signal to support steps of 1 degree, we need 250 steps in 2.5msec or 2000 steps in 20msec, the period time of a 50Hz PWM. So the 48Mhz needs to be scaled down by N to create a So the clock for the timer that generates the PWM must change every 10usec (2000 clocks in 20msec), or 100KHz. The prescaler can divides the system clock by powers of 2 only, so 2,4,8,16 etc... We need a division by 48MHz/100Khz = 48. A power of 2 closest to this is 32 or 64.
prescaler at 64 => 1 step is 13.33usec so in 20msec we have 1500 steps => 180 degree = 2msec will have 150 steps or 1.2 degree of resolution.
prescaler at 32 => 1 step is 6.67usec so in 20msec we have 3000 steps => 180 degree = 2msec will have 300 steps, or 0.6 degree ot resolution.
The timers on ch32v003 are 16bit, so can easy support 1500 or 3000 steps

# Setting up PWM on PA1 (PWM on PC1/PC2 see flicking flame demo)


# slow down tue linear movement of an angle
Changing the PWM pulse from one value to a next value can be done by changing the compare value.
However this would result is a very abrupt reaction of the servo to accelerate as fast as possible, move, and stop abrupt with mechanical forces and counter reactions. Not really what we want.
So in most cases we wat to slow down this reaction and more slowly move to the new angle.
A none blocking, linear approach, is given below. We cut the movement into smaller steps and make every time step a small angle step.

# non-linear angle moment
Closer to real physics, many objects accelerate fast but slow down when approaching their end goal. Eg. throw an appel to the sky. The higher the apple gets the more itnslows down. Or move your head from left to look to something moving on the right. You accelerate fast and slow done once then object becomed visible to get a sharp image.
To emulate a similar effect on our servo we can use again a step but make it non-linear.
The 1 line code below is exactly doing that. We take the difference between initial start and end agles, multiply it by Eg. 0.95 and subtract it from the final end angle. The result is depicted below.
Every step we subtract a smaller fraction from the end angle until the fraction gets neglectable and the`final end angle is reached. This more natural way of movement results is a smoother behavior of mechanical objects and reduces power spikes.
Setting the 0.95 value closer to one limits the initial acceleration at the cost of a slower reaction speed. Updating the new pwm pulse value faster or slower also affect the reaction time. Ads also the pwm frquency sets a limit to how fast updates can be made or otherwise multiple steps will merge into one update.
Some experimenting with these 3 parameters will quickly show what works best for your specific case.
