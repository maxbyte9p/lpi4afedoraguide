# Lichee Pi 4A Fedora Starter Guide

## Grab files
### Download Script
This will download the Fedora T-Head multi-desktops image and U-Boot binary for the Lichee Pi 4A.
```
sh download_required_files
```

### DIY
You can also grab the files yourself from here:
Image: http://openkoji.iscas.ac.cn/kojifiles/work/tasks/7991/1347991/fedora-disk-multi-desktops_thead_th1520-f38-20230516-002100.n.0-sda.raw.xz

U-Boot: 
https://openkoji.iscas.ac.cn/pub/dl/riscv/T-Head/th1520_light/FW/u-boot-with-spl_lpi4a.bin

### IMPORTANT
As of right now I belive the U-Boot binary is not compatible with the minimal image. I have not been able to get the minimal image working SO MAKE SURE YOU USE MULTI-DESKTOP.

Another important note. If you have the 16GB LPDDR4 model of the board the Fedora U-Boot binary still has the bug where only half the memory is recognized. This issue is fixed in the Debian image that you can get for the board.

## Flash Fedora Image To SD Card
To flash the Fedora image using the command line. 

WARNING: I'm not responsible if you make a mistake and write over the wrong drive!
```
sudo wipefs -a /dev/sdX
sudo dd if=fedora-disk-multi-desktops_thead_th1520-f38-20230516-002100.n.0-sda.raw of=/dev/sdX status=progress bs=4M
```

You may also use Balena Etcher which is a more user friendly tool. 
URL: https://etcher.balena.io/

## Flash U-Boot Binary
### Watch For USB Gadget Device
I included a simple shell script that will watch for the burning gadget in order to flash U-Boot. Once the device is detected it will pop up as 'T-HEAD USB download gadget'.
```
sh detect_download_gadget_lichee.sh
```

### Put The Board In Burning Mode
To put the board in burning mode you must locate the button labeled "Boot". It is near the USB-C connecter and wifi module on the board. You must hold down the button as you power on the board. 

### Flash The Binary
To flash the binary you must have android-tools installed and run the following command after the steps from before. This will flash the binary using fastboot to the part of the EMMC used to store U-Boot.
```
sudo fastboot flash uboot ./u-boot-with-spl_lpi4a.bin ;sudo fastboot reboot
sleep 10
sudo fastboot flash uboot ./u-boot-with-spl_lpi4a.bin ;sudo fastboot reboot
```

### IMPORTANT
Make sure you do not have a UART adapter connected to the UART pins on the board! The board will fail to enter burning mode and will throw an error message over the UART connection.

Also make sure you do not have an SD card inserted into the slot! The board will also fail to go into burning mode.

## Insert SD Card, Attach UART, and Power On
### Insert SD Card
On the back of the device under the USB-C connector is the SD Card slot. You may now insert the SD card into this slot.

### Attach UART
The GPIO pins for the board are on the backside. The same side as the USB-C connector.

KEY:
BOTTOM = Closest to the bottom of the board/surface the board sits on.
TOP = Closest to the top of the board/where you see the SOC module.

GPIO 
Pin 2 BOTTOM :   GND
Pin 5 TOP    :   TX
PIN 5 BOTTOM :   RX

Connect GND on the UART cable to Pin 2 BOTTOM.
Connect RX on the UART cable to Pin 5 TOP.
Connect TX on the UART cable to Pin 5 BOTTOM.

### Connecting To UART
To connect to the UART you may use screen or mini-com. To keep this example simple however I'll give the screen example.
```
screen /dev/ttyUSB0 115200
```

## Power On The Board
You should now see the board boot into Fedora! Hooray!


