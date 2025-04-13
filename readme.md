# Installing Ubuntu GPIB, enabling the use of an Agilent 81357b USB-to-GPIB converter, and on toward PyVisa

It is best to update Ubuntu before starting: sudo apt upgrade. This will ensure the C-compiler matches what was used to build the kernel.


## Getting GPIB driver ready to go

Do everthing in this page: https://tomverbeure.github.io/2023/01/29/Installing-Linux-GPIB-Drivers-for-the-Agilent-82357B.html

* for new Ubuntu installs, you might need to install fxload, gcc, libtool, and autoconf, flex, and bison

* if you didn't update your system, this a step in the link might complain about the C compiler used to build the kernel vs the one being used now. 1)update the os or 2) do export CC="gcc-xx" CFLAGS="-O3 -Wall" where xx is some newer versoin of gcc)


## Firmware for the Agilent 81357b

Now go to this page: https://gist.github.com/turingbirds/6eb05c9267a6437183a9567700e8581a

Summary of important steps in this doc:

* download the firmware for the Agilent as stated
* fxload is already installed (step above)
* do the edit to gpib.conf
* do the 2 sudo modeprobe commands
* lsusb: see Bus 00X Device 00Y for the Agilent
* run the big sudo fxload command with /00X/00Y as the /dev/bus/usb/00X/00Y command
* might need to unplug/plug in the device and/or do lsusb a couple of times for X and Y

When the big fxload command runs properly only the green "ready" LED on the Aglient 81357b should now be on.

These steps may still be needed to get the green LED to come on:

* sudo chmod 666 /dev/gpib0

and

```
sudo ln -s /usr/local/lib/libgpib.so.0 /lib/libgpib.so.0
sudo gpib_config
```

should make only the green ready light come on.

### Not quite done yet

If you unplug the Agilent and plug it back in the red light will perist, because Ubuntu could not load the firmware. 

* Continue in the tomvebeure doc to the "Linux User Drivers" section to take care of this.
* After this, you can plug and unplug all you want and the ready light will result on the Agilent.


At this point, you should  be able to connect and talk to your device using

```
ibtest
```

Type issuing the ```*IDN?``` command and your device should response.


# PyVisa

Now install PyVisa.

When done, likely the famous test code of 

```
import pyvisa
rm = pyvisa.ResourceManager()
print(rm.list_resources())
```

will not list your devices.

* This may be the time to install NI's Visa library (but maybe not) and see if this helps (likely not).**

Proceeding then, run

```
pyvisa-info
```

It should complain that gpib-ctypes is not installed. Install this from here:
https://gpib-ctypes.readthedocs.io/en/latest/installation.html

and run pyvisa-info again.  The gpib-ctypes error should be gone and the test-code above should now work.

------

* Note: still not clear if installing NI's Visa library is needed



