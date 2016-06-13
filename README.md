About
=====

With these configuration files you can program bare ATmega microcontrollers from the [Arduino IDE](http://arduino.cc), without using the Arduino bootloader. It supports chips using external or internal clocks.

Skipping the Arduino bootloader means sketches start immediately after power-on, without any delay, and you have a little extra flash memory available to your programs. Using the (optional) slower internal clock options means you can save on components, but also on power (since a slower chip draws less current).

Supported Chips and Clocks
==========================

  * **ATmega168** and **ATmega328p**: I've personally used these. Support is **OK**.
  * **ATmega328**: Same configuration fuses as the ATmega328p. Reported to work. Support is **OK**.
  * **ATmega168p**: Same configuration fuses as the ATmega168. Needs confirmation. Support is **experimental**.
  * **ATmega8**: Imported from another fork. Needs help with testing. Support is **experimental**.

If you have an ATmega168p and this works for you, feel free to drop me a note. If you have an ATmega8 and can spare some minutes testing configuration fuses, check out [issue #5](https://github.com/carlosefr/atmega/issues/5). Your help would be appreciated.

The core `delay()` function is not very precise for clock rates other than external 8 and 16MHz. The internal clock should be fine for most cases but external 12 and 20 MHz drift very noticeably. If your code depends on precision timing, beware.

Install
=======

Open the Arduino IDE preferences window and add the following URL to the `Additional Boards Manager URLs` list:

  * https://raw.githubusercontent.com/carlosefr/atmega/master/package_carlosefr_atmega_index.json

Now go to `Tools > Board > Boards Manager` and search for `Barebones ATmega Chips`. Select it from the list and click `Install`. A new section called `ATmega Microcontrollers` will immediately appear in the `Tools > Board` menu.

![ATmega](https://raw.githubusercontent.com/carlosefr/atmega/master/atmega_addon.png)

Programming
===========

To program the microcontroller you will need an ISP programmer. An [Arduino as ISP](http://arduino.cc/en/Tutorial/ArduinoISP) works just fine:

![Arduino as ISP](http://arduino.cc/en/uploads/Tutorial/SimpleBreadboardAVR.png)

Choose your ISP programmer in the `Tools > Programmer` menu. Then choose your ATmega microcontroller family from `Tools > Board`, the specific chip you have from `Tools > Processor` and your choice of clock frequency and source from `Tools > Clock`.

To set the ATmega configuration fuses, use the `Tools > Burn Bootloader` menu item. This doesn't actually burn an Arduino bootloader onto the chip, it only sets the chip configuration for the chosen clock settings.

To load programs into the microcontroller, use the `Upload` button as usual. You can also use the `Sketch > Upload with Programmer` menu entry. Both will make the IDE upload the code using the selected ISP programmer.

Pin Mapping
===========

The ATmega168/328 families have identical pin configurations. Check [this diagram](http://arduino.cc/en/Hacking/PinMapping168) for their correspondence to Arduino pin numbering:

![ATmega168/168p/328/328p](https://raw.githubusercontent.com/carlosefr/atmega/master/atmega328p.png)

The pin layout for the ATmega8 is also identical to these, but additional functions may be missing (eg. PWM on the left-side pins).

Arduino IDE Versions
====================

These configuration files are meant for Arduino IDE 1.6 or later. If you're still using version 1.0 of the IDE, you'll need to get the [ide-1.0.x](https://github.com/carlosefr/atmega/releases/tag/ide-1.0.x) release instead (only the ATmega168 and 328p are supported).

Tips and Caveats
================

If you're using an ATmega chip that already has the Arduino bootloader inside or has otherwise been configured to require an external clock source, you may get an `avrdude: Yikes! Invalid device signature.` error. In this case, connect an appropriate external clock source to it (most likely 16 MHz) and try again. Once the ATmega has been configured to use its internal clock source, you can remove the external one and the error shouldn't happen again.
