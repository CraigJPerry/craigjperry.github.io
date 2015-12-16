title: Unbricking FTDI Chips with Vagrant
date: 2015-12-16 20:45:44
tags:
---
Some fake FTDI chips had their Product ID reset by old FTDI drivers from 0x6001 to 0x0000 which rendered
them unusable. It's possible to reprogram the Product ID string and the modern FTDI drivers no longer
make this change. I used Vagrant to create a linux virtual machine for this:

    vagrant init ubuntu/trusty64
	vagrant up

I plugged the FTDI in to my host. I opened the virtualbox GUI and assigned this USB device to the
VM. The required USB drivers are not present in this image, so i added them:

    sudo apt-get install linux-image-extra-virtual

It's possible at this point to tell udev to use the ftdi_sio module for devices reporting a product
ID of 0x0000 but that would only make the chip usable from this virtual machine.

I downloaded Mark Lord's ft232r_prog v1.25 from http://rtr.ca/ft232r/ and built it. I then used it
to update the configured product id:

    sudo ./ft232r_prog –old-pid 0x000 –new-pid 0x6001

After re-plugging the device in, it was usable as normal from my host machine.
