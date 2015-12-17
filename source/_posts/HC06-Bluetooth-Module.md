title: HC06 Bluetooth Module
date: 2015-12-17 01:51:34
tags:
---
The HC06 is the same hardware as HC05 but has fewer features. JY-MCU is written on the rear of HC06
modules.

## Connecting To The Serial Port

These devices are 3.3v only. The wiring is straightforward:

    |  Serial Adapter  |  HC06  |
	| 3.3v             | Vcc    |
	| Gnd              | Gnd    |
	| Tx               | Rxd    |
	| Rx               | Txd    |

The state pin emits the same pattern as the red led and can be used to detect current status. The
key pin is unused.

## Entering Commands

HC06 monitors input for 1 second then acts upon what's been received. Newlines should not be used
at the end of commands, as would normally be the case for AT commands.

The available command set is small. The useful subset is easy to remember:

    | Command    | Response   | Notes                           |
	| AT         | OK         | Status check                    |
	| AT+NAMEFoo | setname    | The bluetooth name is now "Foo" |
	| AT+PIN1234 | setpin     | The bluetooth PIN is now 1234   |
	| AT+BAUD4   | OK         | The default baud rate, 9600     |
	| AT+BAUD8   | OK         | 115200 baud rate                |

There are no terminating newlines in these commands. The HC06 simply waits for up to 1 second
and executes at command entered until that point. This means you'll have to copy / paste commands
for them to be fully entered quickly enough.

## Using The Bluetooth Serial Bridge

Connect to the bluetooth device, connect to the bluetooth com port, now any text entered is
sent to the serial connection and vice versa.
