# Installing Ubuntu GPIB and on toward PyVisa

1. Do everthing in this page: https://tomverbeure.github.io/2023/01/29/Installing-Linux-GPIB-Drivers-for-the-Agilent-82357B.html

(this might complain about the C compiler used to build the kernel vs the one being used now. 1)update the os? 2) do export CC="gcc-xx" CFLAGS="-O3 -Wall" where xx is some newer versoin of gcc)


2. This should leave the Agilent 82357B with all 3 lights on (including the red one)

3. Now go to this page: https://gist.github.com/turingbirds/6eb05c9267a6437183a9567700e8581a

and start at "All lights should be on." The steps

sudo chmod 666 /dev/gpib0

and

sudo ln -s /usr/local/lib/libgpib.so.0 /lib/libgpib.so.0
sudo gpib_config

should make only the green ready light come on.

At this point, you should  be able to connect and talk to your device using

ibtest

Type issuing the *IDN? command and your device should response.


4. Now install PyVisa.
When done, likely the famous test code of 

import pyvisa
rm = pyvisa.ResourceManager()
rm.list_resources()

will not list your devices.

a) this may be the time to install NI's Visa library (but maybe not) and see if this helps (likely not).**

Proceeding then, run

pyvisa-info

It should complain that gpib-ctypes is not installed. Install this from here:
https://gpib-ctypes.readthedocs.io/en/latest/installation.html

and run pyvisa-info again.  The gpib-ctypes error should be gone and the test-code above should now work.

------

**Note: still not clear if installing NI's Visa library is needed



