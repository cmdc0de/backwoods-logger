![http://www.bigmessowires.com/blsync.png](http://www.bigmessowires.com/blsync.png)

# Introduction #

The blsync utility is a command-line program used to transfer data from a Logger to a PC. The blsync adapter is a hardware module that plugs into the Logger's ISP port, and converts it to a 6-pin serial connector.

# USB-Serial #

Blsync communicates with the Logger using a 5V or 3.3V serial connection. You will need a USB-serial adapater such as:

  * [Adafruit FTDI Friend](http://www.adafruit.com/products/284)
  * [Sparkfun FTDI Basic Breakout 3.3V](http://www.sparkfun.com/products/9873)
  * [Sparkfun FTDI Cable 3.3V](http://www.sparkfun.com/products/9717)

The Backwoods Logger runs at 3.3V, but because the blsync adapter contains a built-in level-converter, you can use either a 5V or 3.3V USB-Serial cable.

# Blsync Adapter Construction #

The blsync adapter plugs into the Logger's ISP port, and converts it to a 6-pin serial connector. Only the RXD, TXD, and GND pins from the serial connector are used, and VCC, RTS#, and CTS# are not connected.

You can build a blsync adapter from 0.1" header and a MOSFET, or even just wires on a breadboard.

![http://www.bigmessowires.com/blsync-schematic.png](http://www.bigmessowires.com/blsync-schematic.png)

A BS170, 2N7000, or similar N-channel enhancement mode MOSFET is used to perform level conversion on the TX line. If you will always be using a 3.3V USB-serial cable, you can omit the MOSFET and connect TX directly to MISO.

A pre-assembled blsync adapter board using a custom-made PCB is also available for sale, for a more finished solution.

Be sure to connect the blsync adapter board to the Logger with the correct orientation. The blsync board should extend out and away from the Logger, as shown in the photo.

![http://www.bigmessowires.com/blsync-connected.png](http://www.bigmessowires.com/blsync-connected.png)

**Note:** The blsync adapter can not be used to update the firmware in the Logger's ATmega microcontroller-- it is only for syncing data from the Logger. Maybe someday...

# Running the Blsync Utility #

Plug the blsync adapter into the Logger's ISP port, and connect the USB-serial cable to the blsync adapter's 6-pin header. Be careful not to insert either one backwards, or you may damage the Logger.

Open a command prompt window, and run the blsync.exe program. Under Windows, a command prompt can be opened by:

  * Click the taskbar's Start button (the Windows logo button)
  * Type "cmd" into the "search programs and files" box, and press Enter.

Blsync can retrieve the Logger's firmware version number, temperature / altitude / pressure graphs, and snapshot list. Data can be saved in CSV format for importing into Excel, or as raw binary.

The blsync utility is configured using command line options:

```
Usage: blsync -p port [-b speed] [-v] [-c] [-r] [-g filename] [-s filename]
    -p port       Port to use for Logger communication, such as COM1.
    -b speed      Bit rate for communication. Default is 38400.
    -v            Display the Logger firmware version number.
    -c            Save files in CSV format. This is the default.
    -r            Save files in raw binary format instead of CSV.
    -g filename   Sync the graph data, and save it to the named file.
    -s filename   Sync the snapshot data, and save it to the named file.
```

For example, to sync the temperature, altitude, and pressure graphs from the Logger connected to COM3, and save them in CSV format to the file graphs.csv, the command line would be:

```
blsync.exe -p COM3 -c -g graphs.csv
```

By importing the CSV file into Excel or another analysis program, you can quickly create graphs such as this 40 hour temperature / pressure profile:

![http://www.bigmessowires.com/blsync-graph.jpg](http://www.bigmessowires.com/blsync-graph.jpg)