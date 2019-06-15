

# Boron Cloud Debug

*Special code for debugging cloud connection issues with the Particle Boron*

## What is this?

This is a tool to debug cloud connection issues. It:

- Prints out cellular parameters (ICCID, etc.)
- Prints out your cellular carrier and frequency band information
- Pings your IP gateway
- Pings the Google DNS (8.8.8.8)
- Does a DNS server lookup of the device server (device.spark.io)
- Makes an actual cloud connection
- Acts like Tinker after connecting 
- Can print out information about nearby cellular towers

It uses a special debug version of the system firmware, so there's additional debugging information generated as well.

Here's a bit of the output log:

```
service rat=GSM mcc=310, mnc=11094, lac=2 ci=a782 band=GSM 850 bsic=3b arfcn=ed rxlev=40
neighbor 0 rat=GSM mcc=310, mnc=11094, lac=80592df ci=a56f band=GSM 850 bsic=18 arfcn=eb rxlev=37
neighbor 1 rat=GSM mcc=310, mnc=11094, lac=100 ci=a5f2 band=GSM 850 bsic=25 arfcn=b4 rxlev=22
    15.443 AT send      20 "AT+UPING=\"8.8.8.8\"\r\n"
    15.443 AT read  +   14 "\r\n+CIEV: 2,2\r\n"
    15.484 AT read OK    6 "\r\nOK\r\n"
ping addr 8.8.8.8=1
    15.484 AT send      31 "AT+UDNSRN=0,\"device.spark.io\"\r\n"
    17.204 AT read  +   67 "\r\n+UUPING: 1,32,\"google-public-dns-a.google.com\",\"8.8.8.8\",55,812\r\n"
    17.865 AT read  +   67 "\r\n+UUPING: 2,32,\"google-public-dns-a.google.com\",\"8.8.8.8\",55,651\r\n"
    17.936 AT read  +   27 "\r\n+UDNSRN: \"52.91.48.237\"\r\n"
    17.946 AT read OK    6 "\r\nOK\r\n"
```

The source code is in this repository so you can see how it works, however you need to build it as a monolithic debug build for it to work properly. 

## Prerequisites 

- You should have the [Particle CLI](https://docs.particle.io/guide/tools-and-features/cli/) installed.

## To Install 

- Download one of the boron-clouddebug.bin files from this repository. You should download the 0.9.0 version unless you've upgraded your device, in which case you should use the closest version not larger than the version on your device. You only need one of these!

  - [boron-clouddebug-v0.9.0.bin](https://github.com/rickkas7/boron-clouddebug/raw/master/boron-clouddebug-v0.9.0.bin)
  - [boron-clouddebug-v1.2.0-beta.1.bin](https://github.com/rickkas7/boron-clouddebug/raw/master/boron-clouddebug-v1.2.0-beta.1.bin)
  - [boron-clouddebug-v1.1.0.bin](https://github.com/rickkas7/boron-clouddebug/raw/master/boron-clouddebug-v1.1.0.bin)

- Put the Boron in DFU mode (blinking yellow) by pressing RESET and MODE at the same time. Release RESET and continue to hold down MODE. The status LED should blink magenta (red and blue at the same time) then yellow. Once blinking yellow, release MODE.
- Flash the firmware from a command prompt or terminal window:

```
cd Downloads
particle flash --usb boron-clouddebug-v0.9.0.bin
```

- After flashing is complete, press RESET.
- Capture the USB serial log output. One way is:

```
particle serial monitor
```

- If you want to use the optional commands like




## Doing a carrier scan (Boron 2G/3G only)

If you are stuck at blinking green, it can be helpful to do a tower or carrier scan to see if any towers are visible.

Reset the Boron and immediately open the particle serial monitor.

The Boron will breathe white. Tap the MODE button once and a carrier scan will begin. You must do this within the first 10 seconds of startup otherwise the normal tests will be done.

It's not currently possible to do a carrier scan with the Boron LTE (SARA-R410M-02-B).


## Running Advanced Tests

If you connect using particle serial monitor, the default options are used. If you want to use a custom APN or keep-alive you need to use a terminal program that allows you to send USB serial data, such as:

- PuTTY or CoolTerm (Windows)
- screen (Mac or Linux)
- Particle Dev (Atom IDE) Serial Monitor
- Arduino serial monitor

There's more information on using serial terminals [here](https://github.com/rickkas7/serial_tutorial).

Once you connect to the serial terminal you can press Return or wait a few seconds and you should get a menu:

```
clouddebug: press letter corresponding to the command
a - enter APN for 3rd-party SIM card
k - set keep-alive value
c - show carriers at this location
t - run normal tests (occurs automatically after 10 seconds)
or tap the MODE button once to show carriers
```

If you do nothing, the t option (run normal tests) is run, which behaves like the previous version of cloud debug.

If you press c (show carriers at this location), the program will scan nearby towers and show the carriers, frequencies, and signal strength. 
This takes several minutes to run, which is why it's not done by default. It is only supported on the Boron 2G/3G, it is not supported on the Boron LTE.

If you are using a 3rd-party SIM card, you can set the APN and keep-alive values using the a and k options, respectively. Note on the Boron 
the APN and external SIM enabled settings are stored in configuration flash and will continue to be used after flashing normal firmware.


## To Remove

- Go to the [firmware release site](https://github.com/particle-iot/firmware/releases/tag/v0.9.0) and download:

  - [system-part1-0.9.0-boron.bin](https://github.com/particle-iot/device-os/releases/download/v0.9.0/system-part1-0.9.0-boron.bin)
  - [tinker-0.9.0-boron.bin](https://github.com/particle-iot/device-os/releases/download/v0.9.0/tinker-0.9.0-boron.bin)

- Put the Boron in DFU mode (blinking yellow) by pressing RESET and MODE at the same time. Release RESET and continue to hold down MODE. The status LED should blink magenta (red and blue at the same time) then yellow. Once blinking yellow, release MODE.
- Flash the firmware from a command prompt or terminal window:

```
cd Downloads
particle flash --usb system-part1-0.9.0-boron.bin
particle flash --usb tinker-0.9.0-boron.bin
```

If you've upgraded to a version newer than 0.9.0 you should restore that version instead.
