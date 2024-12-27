# DEMO projects for CH32V003-J4M6  (SOP8 package)

CH32V003-J4M6 is a tiny 8-pin MCU from WCH offering 48MHz clock frequency, 16KB FLASH and 2KB RAM.
It can easily replace the popular ATTINY13, at a much lower cost.

Besides an 8-pin version also 16-pin, 20-pin and a 40-pin versions exist, which add many additional GPIOs.
Also these variants can easy compete with ATTINY84 or an arduino, again at a much lower cost.
Find much more information about these MCUs on WCH's webpage: https://www.wch-ic.com

Unfortunately not so many projects exist that use this MCU.

This repository intends to add some more demos and what I learned while making them.

# IDE used

WCH his default IDE is called Mounriver Studio. It can be found here: http://www.mounriver.com + adds demos + the HAL framework, a set of functions defining a bit higher level language/syntax to program the MCUs.

Also the Arduino IDE, PlatformIO and other IDEs support this MCU family.

An alternative resource is https://github.com/cnlohr/ch32v003fun.
It also shows some demos but does not use HAL.

If you want to convert from HAL to ch32v003fun or opposit, just copy/past the code in ChatGPT and as ChatGPT to conver the code to WCH HAL or ch32v003fun syntax.
It will do the job for you.
