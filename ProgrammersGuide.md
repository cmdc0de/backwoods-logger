# Requirements #
  * **AVR programmer hardware.** The officially-supported hardware is the [AVRISP mkII](http://store.atmel.com/PartDetail.aspx?q=p:10500054), which is $34 from Atmel. The $22 [USBtinyISP](http://www.adafruit.com/products/46) AVR programmer is another popular choice.
  * **AVR downloader software.** [AVRDUDE](http://savannah.nongnu.org/projects/avrdude/) is a very popular multi-platform stand-alone downloader. Atmel's free [AVR Studio 5](http://www.atmel.com/microsite/avr_studio_5/default.asp?source=redirect) is a full development environment that also includes a downloader.
  * **AVR development environment (optional).** If you want to customize the Backwoods Logger software or add new features, you'll need [AVR Studio 5](http://www.atmel.com/microsite/avr_studio_5/default.asp?source=redirect)  or another AVR development environment like [WinAVR](http://winavr.sourceforge.net/) or [avr-gcc](http://www.avrfreaks.net/wiki/index.php/Documentation:AVR_GCC).

# Programming the Default Software #
If you just want to load the default Backwoods Logger software, all you need is the AVR downloader software and programming hardware. Make sure the Logger is powered on, and connect the programmer hardware to the six-pin ISP connector on the Logger. Launch AVRDUDE (or your other downloader). If necessary, select the device type as ATMEGA328P. Confirm that the board is working by connecting to the AVR and reading the device ID.

There are two different versions of the Backwoods Logger software, one for the Classic and one for the Mini. Find the file **hikea.hex** in the subdirectory for your version of the Logger, and use the downloader to flash the file to the AVR.

### Fuses ###
In addition to flashing the hikea.hex file, you'll also need to set some configuration fuses on the ATmega chip. Starting from the defaults, set the **EESAVE** fuse on to prevent your stored altitude/temperature data from being erased every time you reprogram the AVR. For the Mini only, set the **CKDIV8** fuse off to get the correct buzzer tones and fast screen drawing.

The Classic fuse settings should be: _low: 0x62, high: 0xD1, extended: 0xFF_

The Mini fuse settings should be: _low: 0xE2, high: 0xD1, extended: 0xFF_

# Customizing the Software #
You are encouraged to experiment with changes to the Backwoods Logger software and hardware. Development is an open source, open hardware project that welcomes everyone's involvement.

The Backwoods Logger source code is written in C, and is built with AVR Studio 5. The included hikea.avrgccproj file will only work with AVR Studio 5. If you're using a different version of AVR Studio, or a different compiler entirely, you'll need to create your own project file or makefile with the following settings:

### Logger Classic ###
  * define these preprocessor symbols: _LOGGER\_CLASSIC, NOKIA\_LCD, F\_CPU=1000000_
  * include these libraries: _libm.a_
  * change these fuses: **EESAVE** on. Fuse settings are _low: 0x62, high: 0xD1, extended: 0xFF_

### Logger Mini ###
  * define these preprocessor symbols: _LOGGER\_MINI, SSD1306\_LCD, F\_CPU=8000000_
  * include these libraries: _libm.a_
  * change these fuses: **EESAVE** on, **CKDIV8** off. Fuse settings are _low: 0xE2, high: 0xD1, extended: 0xFF_

Add your awesome new feature to the C files, compile the project to generate a new .hex file, and program the .hex file to the Logger using the same method as for the default software.