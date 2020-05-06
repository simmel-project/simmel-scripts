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


Programming needs a build of openocd.

1. Attach openocd using `./swd` and then telnet to localhost:4444
2. Run `nrf51 mass_erase`
3. Run `program ../bootloader/_build/build-simmel/simmel_bootloader-0.3.2-32-g91cd582-nosd.hex`
4. Run `program ../bootloader/lib/softdevice/s140_nrf52_6.1.1/s140_nrf52_6.1.1_softdevice.hex`
5. Run `reset`
6. Load `firmware.uf2` via USB from circuitpython
