# Simmel Scrypts

Incantations to bring Simmel to life.

Bootloader is at https://github.com/simmel-project/bootloader.git

```
git clone git@github.com:simmel-project/bootloader.git
cd bootloader
git submodule init
git submodule update
make BOARD=simmel
```

## Building Circuit Python (the first time)

Circuitpython is upstream at https://github.com/adafruit/circuitpython.git

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


Initial programming needs a build of openocd in order to update the `MBR`, which is a Nordic-specific construct.  On Simmel, the `MBR` is located at `0x7f000`.

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
