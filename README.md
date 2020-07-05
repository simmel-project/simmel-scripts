# Simmel Scrypts

## Prerequisites
We assume the following:

* You are running on an Raspberry Pi 4
* You have `openocd` compiled. Recursively clone it from https://github.com/ntfreak/openocd.git,
Configure it with

`./configure --enable-sysfsgpio --enable-bcm2835gpio`

and install it. This is tested against commit a1c51caafbac67df36dbecb27dd4b195730354b9.

If you are not using a Raspberry Pi 4, check the `rpi-simmel.cfg` file and uncomment
the lines corresponding to the model that you're using.

## Getting Started
Clone and build the following items:

The Bootloader is at https://github.com/simmel-project/bootloader.git

```
git clone git@github.com:simmel-project/bootloader.git
cd bootloader
git submodule init
git submodule update
make BOARD=simmel
```

## Building Circuit Python (the first time)

Circuitpython is at https://github.com/simmel-project/circuitpython.git

```
git clone https://github.com/adafruit/circuitpython.git
cd circuitpython
git submodule init
git submodule sync
git submodule update
cd ports/nrf
make BOARD=simmel
```
You will want `build-simmel/firmware.uf2`.  If you're debugging, you'll also want `build-simmel/firmware.elf`.


Initial programming needs a build of openocd in order to update the bootloader settings.  These live at address `0x7f000`, and are part of the bootloader program.  These may be revised at a later date, because this settings field is mostly useful for updating the SoftDevice that resides at the beginning of flash.

1. Attach openocd using `./swd` and then telnet to localhost:4444
2. Run `nrf51 mass_erase`
3. Run `program ../bootloader/_build/build-simmel/simmel_bootloader-0.3.2-56-g02b947e-nosd.hex`
4. Run `program ../bootloader/lib/softdevice/s140_nrf52_7.0.1/s140_nrf52_7.0.1_softdevice.hex`
5. Run `reset`
6. Load `firmware.uf2` via USB from Circuit Python.

## Building Circuit Python (subsequent times)

Syncronize with the latest master.  This will put you on a "detached head" state, meaning you're not on any particular branch.

```
git fetch origin           # Download the latest copy of all repositories to the git work area
git stash                  # If you've made changes, back them up here
git checkout origin master # Check out the latest master from the work area
git submodule sync         # If any submodule URLs have changed, update them
git submodule update       # Check out the latest submodules
git checkout -b mywork     # Exit `detached head` state and give it a new branch name
```

The process of building is the same:

```
cd ports/nrf               # Change to the NRF ports directory
rm -rf build-simmel        # Remove any existing directory -- this is important if the .ld file changes
make BOARD=simmel          # Build circuitpython
```

## Loading Circuit Python

You can load `build-simmel/firmware.elf` via GDB, or load `build-simmel/firmware.uf2` via the bootloader.
