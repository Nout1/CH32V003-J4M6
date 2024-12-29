# DEMO projects for CH32V003-J4M6  (SOP8 package)

CH32V003-J4M6 is a tiny 8-pin MCU from WCH offering 48MHz clock frequency, 16KB FLASH and 2KB RAM.
It can easily replace the popular ATTINY13 and ATTINY85 at a much lower cost.

Besides an 8-pin version also 16-pin, 20-pin versions exist, which add many additional GPIOs.
Also these variants can easy compete with ATTINY84 or an arduino, again at a much lower cost.
Given the lower cost, the popularity for these MCUs is rapidly increasing.
Find much more information about them on WCH's webpage: https://www.wch-ic.co

This repository intends to add some more example projecte and what I learned while making them.

# IDE used

WCH his default IDE is called Mounriver Studio. It can be found here: http://www.mounriver.com
It includes some demos = the HAL framework, a set of functions defining a bit higher level language/syntax to program the MCUs.

Also the Arduino IDE, PlatformIO and other IDEs now support this MCU family.

An alternative resource to WCH is https://github.com/cnlohr/ch32v003fun.
It also shows some examples but instead of HAL it directly programs the registers.
HAL is possibly easier for beginners.

If you want to convert from HAL to ch32v003fun syntax or opposit, just copy/past the code in ChatGPT and ask ChatGPT to convert the code to WCH HAL or ch32v003fun syntax.
It will do the job for you.
