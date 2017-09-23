<h1>ICSP Pogo Pin Adapter</h1>

## Overview

The ICSP pogo pin adapter is geared toward hobbyists who love to design their own PCB's and program them with an Arduino!
It is specifically designed for Atmel microcontrollers that are programmable via SPI and is very useful for
flashing the Arduino bootloader to the chip. The only thing you have to do is design your PCB with six test
point pads placed in a standard 2x3 ICSP configuration with 2.54mm (0.1") spacing.

## Supported Hardware

Below is a list of supported Atmel microcontrollers:

- ATtiny25/45/85
- ATtiny24/44/84
- ATtiny2313/4313
- ATmega48A/PA/88A/PA/168A/PA/328/P
- ATmega328PB (improved version of 328P)
- ATmega640/V-1280/V-1281/V-2560/V-2561/V

Keep in mind there are many other variants, but I've listed the most common and most supported ones. Most variants
vary in terms of flash memory. The first number indicates the flash memory size in KB. For example, ATtiny25 has 2KB
of flash, ATtiny45 has 4KB, and ATtiny85 has 8KB and is the most common.

## Instructions

### PCB Design Info

This section provides details on how to design your PCB for use with this pogo pin adapter.
- In order to use this pogo pin adapter you need to design your PCB with six test points, arranged in a 2x3 array with 2.54mm (0.1") spacing as shown below:
- The SPI (MISO/MOSI/SCK/RESET) connections go to your Atmel MCU, depending on which chip you're using. Below is a list of the connections you need to make for the various chip types:
- NOTE: If the test points are on the backside of the PCB, you need to make sure that the test points still have this physical arrangement *as viewed from the bottom of the PCB*. This is extremely important, because if you simply use the EAGLE mirror tool it will show you the bottom layer in blue as viewed from the front!
- If you are using EAGLE you can find pre-made test points libraries from the [Sparkfun EAGLE libraries](https://github.com/sparkfun/SparkFun-Eagle-Libraries) under "SparkFun-Connectors.lbr" named "TEST-POINT".

### Kit Assembly

If you bought the already-assembled adapter you can skip to the next section! If you bought the kit, keep reading!
- In the kit should be a PCB that is actually two separate PCB's. Snap them apart and trim/file the connecting link
if needed
- Place both PCB's on a table with the text facing up.
- Attach the two standoffs to the top of the larger PCB using two of the screws in the kit.
- Screw the smaller board on top of the standoffs with the remaining two screws.
- It's time to solder the pogo pins in place! First place a piece of tape on the bottom of the larger board. This is to keep the pogo pins at the same level.
- Now place the six pogo pins (facing up) through the holes on the small PCB and down through the holes on the bottom PCB until they rest on the tape.
- Keep the assembly on a flat surface and first solder the pogo pins to the *upper* PCB so that the pins don't move in place.
- Now remove the piece of tape from the bottom PCB and solder them to the large board.
- To finish the assembly, solder the 2x3 female header block from the bottom of the larger board. This part plugs into the Arduino ICSP header. Alternatively, you can solder a [2x3 header](https://www.sparkfun.com/products/10877) to the *bottom* of the larger PCB and use a [2x3 ribbon cable](https://www.amazon.com/Connector-Cable-SODIAL-2-54mm-12-inch/dp/B01GNVN48O/ref=sr_1_8?ie=UTF8&qid=1506182997&sr=8-8&keywords=2x3+idc+ribbon+cable) as an extension. However, make sure you plug the cable into the Arduino ICSP header the right way!
- or you can simply use male-to-female [Dupont wires](https://www.amazon.com/Haitronic-Multicolored-Breadboard-Arduino-raspberry/dp/B01LZF1ZSZ/ref=sr_1_3?ie=UTF8&qid=1506189503&sr=8-3&keywords=dupont+wires+m+to+f) with the default configuration, making sure you connect the pins appropriately. 

### Using the Adapter

These instructions assume that you are using the Arduino IDE and that you are already comfortable with the coding/upload process. If not, there are many tutorials available online!
- Install the latest version of the [Arduino IDE](http://arduino.cc/en/main/software)
- Go to File > Examples > ArduinoISP and load the example sketch "ArduinoISP"
- Make sure nothing is connected to your Arduino yet! You can have the cable attached to the ICSP header but don't press the pogo pins onto anything.
- Upload the ArduinoISP sketch to your Arduino by choosing the appropriate board and COM port.
- Place the 10uF capacitor between the RESET and GND pins on the Arduino. The white stripe (negative side) should go to GND.
- Plug the pogo pin adapter into the Arduino ICSP header (making sure you do it in the right direction!), or use the ribbon cable extension if you chose that route.
- Connect the standalone pin labeled "RST/D10" to digital pin 10 on the Arduino.
- Now you are ready to program your custom PCB and Atmel chip! To program the target PCB you will need to align the test points on the target PCB with the pogo pins. You can do this by either placing the target PCB directly onto the pogo pins in the correct orientation, or lifting the Arduino with the pogo pin adapter and pressing it against the target PCB. If you do it this way it might be easier to use the jumper wires so you only have to lift the adapter part and not the entire Arduino.
- Keep the pogo pins firmly pressed against the test points during programming, and follow the instructions in the next section for more details about programming!

### Burning Bootloader/Uploading Code

In any of the cases below, the ArduinoISP sketch should have already been loaded to your Arduino board. This allows the Arduino to act as a programmer.

#### ATtiny25/45/85 and ATtiny24/44/84

Many ATtiny chips don't have hardware serial (TX/RX) so instead of burning the Arduino bootloader, which enables you to program a chip via TX/RX (with a USB-to-Serial adapter like FTDI), you will need to  unless you use software to "bit-bang" the chip via two pins instead, like the [Digispark ATtimy85 board](http://digistump.com/products/1), which uses the [micronucleus](https://github.com/micronucleus/micronucleus) USB bootloader.
- In Arduino IDE go to Tools/Boards/Board Manager and search "attiny" and find "attiny by David A. Mellis" and install and restart Arduino IDE.
- Select the appropriate chip type and clock rate, then click "Burn Bootloader" under Tools. Note that this doesn't actually burn a bootloader per se, but configures the chip by setting the appropriate bits and fuses internally so that you can upload to it afterward. You only have to do this once.
- Now you can open your Arduino sketch and upload code to the ATtiny using the pogo pin adapter by clicking "Upload" while pressing the pins against the test points!

#### ATtiny2313/4313

For these chips I highly recommend [this tutorial](https://oscarliang.com/program-attiny2313-using-arduino/). The cool thing about them is that they have hardware serial, which makes it very easy to debug, unlike the ATtiny25/45/85 and ATtiny24/44/84 processors.

#### ATmega168A/PA/328/P

The ATmega chips have hardware serial whic his useful for debugging purposes. Because they have hardware serial TX/RX pins, you will need to decide whether you want to flash a bootloader to the chip or not. The advantage of the bootloader is that you can reprogram the chip via TX/RX with a USB-to-Serial board instead of writing code to the chip directly via SPI. This is useful if you want to program, test, program, test... However, it should be noted that the ATmega3328 Optiboot bootlader has a 1s delay after pressing the RESET button, and this may not be acceptable for your custom PCB. Either way, you can always change your mind!

##### Without External Clock

If your chip doesn't have an external clock, follow the instructions [here](https://www.arduino.cc/en/Tutorial/ArduinoToBreadboard) under the section "Minimal Circuit (Eliminating the External Clock)" in order to get Arduino support.

##### With Bootloader

- If you are using a 16MHz crystal, in the Arduino IDE go to Tools > Boards and select "Arduino Duemilanove" or "Arduino Nano", then select "ATmega168" or "ATmega328" in the "Processor" menu.
- If you are using a standalone ATmega, select "ATmega328 on a breadboard (8 MHz internal clock)"
- Click "Tools/Burn Bootloader" while pressing the pogo pins against the test points on the target PCB.
- Now that the bootloader is loaded onto the chip, you can safely store your pogo pin adapter in your drawer and pull out a USB-to-serial adapter like [this CP2104 board](https://www.adafruit.com/product/3309) which is much cheaper than the standard FTDI boards, and this one includes a micro USB! If you use this board, make sure to install the [Si Labs drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers).
- To program the chip all you need to do is connect the programmer's RX to the chip's TX, and vice versa, connect VCC and GND, then connect the programmer's RTS pin (or DTR if you're using a standard FTDI programmer) to the chip's RESET pin via a 0.1uF capacitor (if the cap is polarized, the "+" should be on the programmer side, and "-" should be on the target chip's RESET pin).
- Open Arduino IDE and hit "Upload" and watch the dots fly across the bottom of the screen! Serial monitor should work too!

##### Without Bootloader

- If you are using a 16MHz crystal, in the Arduino IDE go to Tools > Boards and select "Arduino Duemilanove" or "Arduino Nano", then select "ATmega168" or "ATmega328" in the "Processor" menu.
- If you are using a standalone ATmega, select "ATmega328 on a breadboard (8 MHz internal clock)"
- Without the bootloader you can simply open your sketch, press the pogo pins against the target board's test points, and click "Upload". If you don't want to sit there and press the pogo pins against the test points, you can wait for it to almost finish compiling, then press the pogo pins when it's close to uploading. Just a tip for lazy people  :)  ... or you could just make a custom jig to hold the board for you!
- Note that if you had burned the bootloader prior to using this method to upload code, the bootloader will be erased.
- If you need serial debugging you can always program it in the code and use a USB-to-serial programmer connected to TX/RX to use the serial monitor.
