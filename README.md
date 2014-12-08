Linux Bridge Over USB
=====================

For projects requiring more performance, Arduino Bridge can be run on
32 bit boards: Teensy 3.1 or Arduino Due, for more memory and much
higher performance.

USB is used for the Bridge communication, eliminating the slow serial
console bottleneck.

You can run the Linux Bridge on Raspberry Pi, Beaglebone, Wandboard,
or even a high-end Ubuntu PC.  These options allow you to scale a
Bridge-based project up to far more powerful hardware.


Quick Install Steps
-------------------

Simply copy files to their locations on the Linux computer:

        sudo cp 00-bridge.rules /etc/udev/rules.d/
        chmod 755 run-bridge run-bridge-udev
        sudo cp run-bridge run-bridge-udev /usr/bin/
        sudo cp -r bridge /usr/lib/

If using a non-Teensy board, edit run-bridge-udev with its USB ID
numbers.  Optionally, edit the USB serial number, to restrict to using
only a specific Teensy.

On the Arduino side, use this modified Bridge library:

https://github.com/PaulStoffregen/Bridge

Use Bridge.begin(Serial) on Teensy 3.1, or Bridge.begin(SerialUSB)
on Arduino Due, to run the Bridge communication over the native USB
Serial port.


Security Considerations
-----------------------

The Bridge library allows complete access to your Linux system.  Any
USB device with matching ID numbers can take control.  Projects designed
with this code must not expose USB ports to untrusted persons.


Bridge Startup Details
----------------------

On Arduino Yun, the Linux serial console, which always runs a login
shell, connects directly to the microcontroller's serial port.

When using Teensy 3.1 or Arduino Due by USB, an extra step is needed
to properly handle plugging and unplugging the USB cable.

When the cable is attached, udev creates the /dev/ttyACM# device.
00-bridge.rules causes udev to execute run-bridge-udev right after
the device is created.

The run-bridge-udev runs getty, with an automatic login, so the USB
serial port automatically starts up with a similar shell login as the
serial console on Arduino Yun.

Using Bridge.begin(Serial) causes the Arduino library to execute the
"run-bridge" script, the same as it would on Arduino Yun.  Well, except
your can run the Arduino code much faster and with far more memory on
Teensy 3.1, and the Linux Bridge can run on a wide range of more powerful
boards, and the Bridge communication takes place at native USB speed.


