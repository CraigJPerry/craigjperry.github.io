title: ESP8266 Arduino
date: 2015-12-16 19:18:31
tags:
---
## The Software

The Arduino environment can be used to program ESP8266 WiFi boards. To configure the Arduino IDE v1.6.5
or newer, go to *File -> Preferences* and add the following *Additional Boards Manager URL*:
http://arduino.esp8266.com/stable/package_esp8266com_index.json

Then go to *Tools -> Board -> Boards Manager*. Install the latest esp8266 board manager.

### Libraries

Available libraries include HTTP servers, mDNS / Bonjour & SSDP service discovery alongside
any other Arduino libraries which don't make use of native ATMEL hardware functions.

An implicit *ESP* object provides access to platform native functionality not found in the other
Arduino environments. 

## The Hardware

The most common board is the ESP01. It's a tiny 3.3v device with 2 rows of 4pin headers at one end and
a pcb antenna at the other. It's capable of WiFi b/g/n in client or access point modes at up to 300m range.

The processor runs at 80MHz and has a 512K SPI flash alongside it which runs at 40MHz. 64K of this flash
is used as a filesystem (SPIFFS).

There are 4 GPIO pins, 2 of which double up as UART Tx/Rx. The UART defaults to 115200 baud rate. There's
also a blue LED on the GPIO1 / UART TXD pin, it is active when the GPIO1 line is pulled low.

### Pinouts

With the board sitting so that the PCB antenna is facing upwards on the right hand side, the pins are:

    | GPIO1 / UART TXD / Blue LED | GND              |
    | Chip Enable (pullup to Vcc) | GPIO2            | 
    | RST                         | GPIO0            |
    | Vcc                         | GPIO3 / UART RXD |

If using GPIO1 and / or GPIO3, then the Serial port is unavailable. Likewise, if using the serial port
then only 2 GPIOs are available for use.

The nature of the pinout means these boards are not particularly easy to use with a protoyping
breadboard. I went with this idea: http://tim.jagenberg.info/2014/11/08/diy-esp8266ex-breadboard-adapter/

### Programming Connections

Whatever the pins are used for in your circuit, they are automatically overridden when the board is in
programming mode.

Connections to a 3.3v USB Serial adapter are:

    |   ESP8266   |  Serial Adapter  |
    | GPIO1 / Tx  | Rx               |
    | GND         | GND              |
    | Chip Enable | 3.3v             |
    | GPIO2       | N/A              |
    | RST         | RTS              |
    | GPIO0       | N/A              |
    | Vcc         | 3.3v             |
    | GPIO3 / Rx  | Tx               |

### Other Boards

Other boards are available, almost all of them expose more pins than the ESP01. The most flexible
appears to be the NodeMCU v1.0 which for around £5 comes with an ESP-12E, a much larger 4MB SPI flash,
an onboard voltage regulator and an onboard CP2012 USB to serial adapter all in a breadboard-friendly
sized package.
