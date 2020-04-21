# Simmel Scrypts

Incantations to bring Simmel to life.

Bootloader is at https://github.com/simmel-project/bootloader.git

Circuitpython is at https://github.com/simmel-project/circuitpython.git

```
git clone https://github.com/simmel-project/circuitpython.git
cd circuitpython
git checkout simmel
git submodule init
git submodule sync
git submodule update
cd ports/nrf
make BOARD=simmel
```

1. Attach openocd using `./swd` and then telnet to localhost:4444
2. Run `nrf51 mass_erase`
3. Run `program ../bootloader/_build/build-simmel/simmel_bootloader-0.3.2-32-g91cd582-nosd.hex`
4. Run `program ../bootloader/lib/softdevice/s140_nrf52_6.1.1/s140_nrf52_6.1.1_softdevice.hex`
5. Run `reset`
6. Load `firmware.uf2` via USB from circuitpython
