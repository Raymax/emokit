Emokit
======

By Cody Brocious and Kyle Machulis

Contributions by

* Severin Lemaignan (Base C Library and mcrypt functionality)

Code fixed for OpenViBE by Maxime Bordes (tested on Linux only with Emotiv headset consumer)

Description
===========

Emokit is a set of language for user space access to the raw stream
data from the Emotiv EPOC headset. Note that this will not give you
processed data (i.e. anything available in the Emo Suites in the
software), just the raw sensor data.

The C library is currently supported on:

* OS X/Linux - Via libusb-1.0
* Windows - via Win32 HID calls

The Python library is currently supported on:

* Linux - udev rules and file system access (no special library required)
* Windows - pywinhid
* OS X - Coming soon (via pyusb)

Required Libraries
==================

Python
------

* pywinhid (Windows Only) - http://code.google.com/p/pywinusb/
* pyusb (OS X, Optional for Linux) - http://sourceforge.net/projects/pyusb/

C Language
----------

* CMake (Required on all platforms) - http://www.cmake.org
* WDK (Windows Only) - http://www.microsoft.com/whdc/devtools/WDK/default.mspx
* libusb-1.0 (All non-windows platforms) - http://www.libusb.org
* Mcrypt (Required on all platforms) - https://sourceforge.net/projects/mcrypt/

Extra libraries might be required to install the previous ones like:
* libmcrypt - http://sourceforge.net/projects/mcrypt/files/Libmcrypt/
* mhash - http://sourceforge.net/projects/mhash/

Usage
=====

C library
---------

See emokitd.c and emokit_osc examples

Python library
--------------

  import emotiv
  headset = emotiv.Emotiv()
  try:
    while True:
      for packet in headset.dequeue():
        print packet.gyroX, packet.gyroY
  finally:
    headset.close()

Platform Specifics
==================

OS X
----

You will need to install the EmotivNullDriver.kext on OS X for
software to be able to access the EPOC. To do this, copy the
osx/EmotivNullDriver.kext directory to /System/Library/Extensions/.
Once this is done, from the terminal, run

sudo kextutil /System/Library/Extensions/EmotivNullDriver.kext

Or else just reboot. This will blacklist the emotiv from the HID
Manager so it can be read by Emokit. No telling what this will do in
conjunction with the Emotiv OS X drivers, I haven't tested that yet.

Linux
-----

There are two ways to run Emokit on Linux

* Copy the udev rules to /etc/udev/rules and restart udev, in which
  case you'll have access to /dev/hidrawX, where X is probably 0 and
  1. You're interested in whatever the higher number is.
* Use the libusb driver, which will detach from the HID Manager as
  long as you run whatever you need in sudo. Otherwise, you'll need to
  blacklist the VID/PID pair out of the kernel

Windows
-------

No platform specifics. Should just work.

OpenViBE
========

Download the version of antoche which includes the Emokit driver: https://github.com/antoche/openvibe

On linux, you need to manually install emokit.h into /usr/local/include/emokit folder and libemokit.a (generated when building Emokit) into /usr/local/lib folder.

Then, build and install OpenViBE: execute first "linux-install_dependencies", then "linux-clean" and finally "linux-build". If everything goes well, OpenViBE is installed in a "dist" folder inside the main folder of the software.

To get Emokit driver and OpenViBE working properly, run the acquisition-driver (containing Emokit driver) and the designer in administrator mode.

Credits - Cody
==============

Huge thanks to everyone who donated to the fund drive that got the
hardware into my hands to build this.

Thanks to Bryan Bishop and the other guys in #hplusroadmap on Freenode
for your help and support.

And as always, thanks to my friends and family for supporting me and
suffering through my obsession of the week.

Credits - Kyle
==============

Kyle would like to thank Cody for doing the hard part. 

He would also like to thank emotiv for putting emo on the front of
everything because it's god damn hilarious. I mean, really, Emo
Suites? Saddest hotel EVER.
