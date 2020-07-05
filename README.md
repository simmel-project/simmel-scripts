# Simmel Scrypts

We assume the following:

* You are running on an Raspberry Pi 4
* You have `openocd` compiled. Recursively clone it from https://github.com/ntfreak/openocd.git,
Configure it with

`./configure --enable-sysfsgpio --enable-bcm2835gpio`

and install it.

If you are not using a Raspberry Pi 4, check the `rpi-simmel.cfg` file and uncomment
the lines corresponding to the model that you're using.

# Quickstart

Clone and build the following items:

* Bootloader is at https://github.com/simmel-project/bootloader.git
* Circuitpython is at https://github.com/simmel-project/circuitpython.git

Now provision the binaries:

1. Attach openocd using `./swd` and then telnet to localhost:4444
2. Run `nrf51 mass_erase`
3. Run `program ../bootloader/_build/build-simmel/simmel_bootloader-0.3.2-32-g91cd582-nosd.hex`
4. Run `program ../bootloader/lib/softdevice/s140_nrf52_6.1.1/s140_nrf52_6.1.1_softdevice.hex`
5. Run `reset`
6. Load `firmware.uf2` via USB from circuitpython
