# Running PX4 Firmware on the STM Discovery Development Kit

```sh
mkdir -p ~/src
```


## ST-LINK for Linux

[ST-Link for Linux](https://github.com/texane/stlink)

```sh
sudo apt-get install libusb-1.0-0-dev autoconf autogen intltool
```

```sh
cd ~/src
git clone https://github.com/texane/stlink.git
```

```sh
cd stlink/
./autogen.sh
./configure
make
```

=> Connect the STM32F4Discovery Board over the USB Mini to your PC

```sh
./st-util
```

You should get something similar to:

```sh
~/stlink $ ./st-util 
2016-01-10T22:35:19 INFO src/stlink-common.c: Loading device parameters....
2016-01-10T22:35:19 INFO src/stlink-common.c: Device connected is: F4 device, id 0x10016413
2016-01-10T22:35:19 INFO src/stlink-common.c: SRAM size: 0x30000 bytes (192 KiB), Flash: 0x100000 bytes (1024 KiB) in pages of 16384 bytes
2016-01-10T22:35:19 INFO gdbserver/gdb-server.c: Chip ID is 00000413, Core ID is  2ba01477.
2016-01-10T22:35:19 INFO gdbserver/gdb-server.c: Target voltage is 2879 mV.
2016-01-10T22:35:19 INFO gdbserver/gdb-server.c: Listening at *:4242...
```

CTRL+C to finish this test.

## Bootloader

https://github.com/PX4/Bootloader

```sh
cd ~/src
git clone https://github.com/PX4/Bootloader
```

```sh
cd Bootloader
make
```

```sh
~/src/stlink/st-flash write px4discovery_bl.bin 0x08000000
2016-01-10T22:42:27 INFO src/stlink-common.c: Loading device parameters....
2016-01-10T22:42:27 INFO src/stlink-common.c: Device connected is: F4 device, id 0x10016413
2016-01-10T22:42:27 INFO src/stlink-common.c: SRAM size: 0x30000 bytes (192 KiB), Flash: 0x100000 bytes (1024 KiB) in pages of 16384 bytes
2016-01-10T22:42:27 INFO src/stlink-common.c: Attempting to write 8796 (0x225c) bytes to stm32 address: 134217728 (0x8000000)
EraseFlash - Sector:0x0 Size:0x4000
Flash page at addr: 0x08000000 erased
2016-01-10T22:42:27 INFO src/stlink-common.c: Finished erasing 1 pages of 16384 (0x4000) bytes
2016-01-10T22:42:27 INFO src/stlink-common.c: Starting Flash write for F2/F4/L4
2016-01-10T22:42:27 INFO src/stlink-common.c: Successfully loaded flash loader in sram
enabling 32-bit flash writes
size: 8796
2016-01-10T22:42:27 INFO src/stlink-common.c: Starting verification of write complete
2016-01-10T22:42:27 INFO src/stlink-common.c: Flash written and verified! jolly good!
```

=> connect the STM32F4Discovery Board additionaly over the USB Micro Port to your PC

=> press the Black Reset Button, the red LED starts flashing fast

## Build PX4 Firmware

```sh
cd ~/src
git clone https://github.com/PX4/Firmware.git
```

```sh
cd Firmware
make px4-stm32f4discovery_default upload
..
..
[ 97%] Built target firmware_nuttx
[ 98%] uploading /home/stefan/src/firmware/build_px4-stm32f4discovery_default/src/firmware/nuttx/nuttx-px4-stm32f4discovery-default.px4
Loaded firmware for 63,0, size: 350212 bytes, waiting for the bootloader...
If the board does not respond within 1-2 seconds, unplug and re-plug the USB connector.
Found board 63,0 bootloader rev 5 on /dev/serial/by-id/usb-3D_Robotics_PX4_BL_DISCOVERY_0-if00
ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
ffffffff ffffffff ffffffff ffffffff type: ÿÿÿÿ
idtype: =FF
vid: ffffffff
pid: ffffffff
coa: //////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////8=

sn: 002200223231471432383435
chip: 10016413
family: STM32F40x
revision: Z
flash 1032192

Erase  : [====================] 100.0%
Program: [====================] 100.0%
Verify : [====================] 100.0%
Rebooting.

[100%] Built target upload
```


=> Press the black Reset button, to restart the bootloader !








