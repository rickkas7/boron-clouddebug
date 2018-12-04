# boron-clouddebug

**Cloud debugging tool for the Particle Boron**

This is a temporary placeholder. It's really just tinker-debug, because the Boron system firmware does not include the necessary features to port the whole electron-clouddebug yet.

However it will get a debug log which can be helpful for diagnosing connection problems.

## To install

- Download the tinker-debug.bin file.
- Put the Boron in DFU mode (blinking yellow) by pressing RESET and MODE at the same time. Release RESET and continue to hold down MODE. The status LED should blink magenta (red and blue at the same time) then yellow. Once blinking yellow, release MODE.
- Flash the firmware from a command prompt or terminal window:

```
cd Downloads
particle flash --usb tinker-debug.bin
```

- After flashing is complete, press RESET.
- Capture the USB serial log output. One way is:

```
particle serial monitor
```

## To remove

- Go to the [firmware release site](https://github.com/particle-iot/firmware/releases/tag/v0.8.0-rc.25-mesh) and download [hybrid-0.8.0-rc.25-boron.bin](https://github.com/particle-iot/firmware/releases/download/v0.8.0-rc.25-mesh/hybrid-0.8.0-rc.25-boron.bin).
- Put the Boron in DFU mode (blinking yellow) by pressing RESET and MODE at the same time. Release RESET and continue to hold down MODE. The status LED should blink magenta (red and blue at the same time) then yellow. Once blinking yellow, release MODE.
- Flash the firmware from a command prompt or terminal window:

```
cd Downloads
particle flash --usb hybrid-0.8.0-rc.25-boron.bin
```

## Technical details

This is a Boron 0.8.0-rc.25 monolithic debug build of tinker, with serial logging enabled. 

To remove it, you should flash a hybrid build, which is able to overwrite a monolithic build but be able to be flashed with modular user firmware in the future.
