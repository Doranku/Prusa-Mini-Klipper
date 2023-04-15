# Prusa-Mini-Klipper

Running Klipper on a Prusa Mini based on Mainsail.
https://docs.mainsail.xyz/

The directory *config* contains the configuration for
the Mini. The idea is to keep the changes to the 
PrusaSlicer printer settings to a minimum, so try to
be Marlin compatible.

Needs some work:
- (un)load macro

Untested so far:
- M600 color changes

# Why use klipper

Because you can! You get some extra control with regards to input shaping.
But you could spend the time to tune the original Prusa Firmware to speed up
and/or increase quality. OctoPi and all its plugins with the original firmware
give you a much smoother experience. 

With klipper I have been able to speed up my function prints at some cost to
visual quality. When keeping the default PrusaSlicer Mini profiles the added
tuning for pressure advance and anti-resonance should improve the visual
quality, I did not test this since I mostly don't care about visuals.

# Installing klipper

*To play it save: first make sure compiling works before modifying
the Mini.*
*Might void warrenty.*
*Only proceed at your own risc*
*Insert scary warnings about you house burning down here*

Assuming you have installed Mainsail and are logged in to your 
default user (pi for Mainsail OS Raspberry Pi)
```
cd ~/klipper
make menuconfig
```

- Enable extra low-level configuration options
- select STMicroelectronics STM32
- STM32F407
- Bootloader offset to 128KiB + 512 byte offset
- Clock Reference to 12 MHz crystal
- Communication interface to USB (on PA11/PA12)

```
make flash
```

If everything compiles correctly it will fail to find a device to flash.
At this moment you need to boot the Mini into DFU mode. You need to break 
the appendix, possibly voiding your warrenty depending on local consumer 
protection laws.

See https://help.prusa3d.com/article/flashing-custom-firmware-mini_14

Use
```
lsusb
```
to find out what the device ID of the printer is. Use that to actually flash it.

```
make flash FLASH_DEVICE=0483:df11
```
Reset the Mini and you should see an image indicating a developer firmware 
running. The display is not supported at the moment of writing. People
have been working of display support in the past, for example see:
https://github.com/singh-gur/mini_klipper

# Reverting back to Prusa Firmware

*UNTESTED*

Clone and build a DFU version of the Prusa Firmware:
```
git clone https://github.com/prusa3d/Prusa-Firmware-Buddy
```

Build it:
```
python utils/build.py --preset mini --build-type release --generate-dfu --bootloader yes
```

Copy the dfu firmware from *build/mini_release_boot/* to your mainsail device.

Put the Mini in DFU mode by shorting the correct pins and resetting. And than flash it:
```
dfu-util -a 0 -D firmware.dfu
```
Remove the short and reset the Mini.
