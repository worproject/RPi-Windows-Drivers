# Windows on ARM(64) device drivers for the Raspberry Pi platform
This repository is currently used only for publishing binary releases: https://github.com/worproject/RPi-Windows-Drivers/releases

The source code for some of the prebuilt drivers can be found here: https://github.com/driver1998/bsp

## Notes
* most of the drivers require the *Test Signing mode* to be enabled.

* **if using a Raspberry Pi 4 board, RAM must be limited to 1 GB for some devices to work correctly**.

* ARM32 drivers can't be installed on an ARM64 image or vice versa.

* drivers included in the latest release that can't be found in the "bsp" repository above were either extracted from old builds of Windows 10 IoT Core (ARM32 binaries) or recompiled for ARM64 by their developers (thanks to [MCCI](https://mcci.com) and [Microchip](https://www.microchip.com)).

## Status

### Raspberry Pi 4 (ARM64)
The SoC on this model shares some peripherals with older models. 
The ones that are not working on the Pi 3 are not working here either. In addition to that, the following drivers are known to cause issues: 
* **bcmauxspi.sys** (unknown reason)
* **rpisdhc.sys** (the SD2.0 Host Controller is gone)

The rest of the inherited devices that work on the previous model should work on the Pi 4 too. 

Note that some devices have DMA issues on more than 1 GB of RAM, so it should be limited by setting the `truncatememory` BCD parameter.

USB-C host support is provided by the **mcci_dwchsotg** driver and it does work, but is affected by the issue above.

As for the new peripherals:
* the front USB-A ports don't currently work due to an issue with the built-in xHCI Windows driver.

* Broadcom GENET (Ethernet adapter) and all other new peripherals don't work due to missing device drivers.

### Raspberry Pi 3 (ARM64)

|Device|Driver|Status|Additional information|
| --- | --- | --- | --- |
|Arasan SD/SDIO Host Controller|bcm2836sdhc.sys|**Working**||
|SD2.0 Host Controller|rpisdhc.sys|**Working**||
|GPIO|bcmgpio.sys|**Working**||
|SPI|bcmspi.sys|**Working**||
|AUXSPI|bcmauxspi.sys|**Working**||
|I2C|bcmi2c.sys|**Working**||
|PWM|bcm2836pwm.sys|**Working**||
|Audio Jack (PWM-driven)|rpiwav.sys|**Working**||
|Mini UART|pi_miniuart.sys|**Working**||
|PL011 UART|SerPL011.sys|_Not working_||
|VC4 Mailbox Interface|rpiq.sys|**Working**||
|VC4 Host Interface Queue|vchiq.sys|_Not working_||
|VC4 GPU (Graphics)|roskmd.sys|_Not working_|the driver loads, but it doesn't do much as it's unfinished|
|HDMI Audio|No driver available|_Not working_||
|Basic Display Adapter (frame buffer)|MSBDD (Inbox)|**Working**||
|DesignWare HS USB 2.0 OTG Controller|mcci_dwchsotg_hcd.sys, mcci_dwchsotg_hub.sys|**Working**||
|LAN9514 USB Ethernet Adapter|lan9500-arm64-n650f.sys|**Working**|Ethernet support for RPi 3 B|
|LAN7515 USB Ethernet Adapter|lan7800-arm64-n650f.sys|**Working**|Ethernet support for RPi 3 B+|
|BCM43438 Wireless LAN|No driver available|_Not working_|WLAN support for RPi 3 B|
|BCM43455 Wireless LAN|No driver available|_Not working_|WLAN support for RPi 3 B+|
|BCM43438/BCM43455 UART Bluetooth|No driver available|_Not working_||

### Raspberry Pi 3 (ARM32)

Same as the ARM64 version, with some differences:

|Device|Driver|Status|Additional information|
| --- | --- | --- | --- |
|PL011 UART|SerPL011.sys|**Working**||
|VC4 Host Interface Queue|vchiq.sys|_Partially working_|crashes after some usage (tested using a few userland apps ported by Microsoft)|
|DesignWare HS USB 2.0 OTG Controller|dwchsotg_hcd.sys, dwchsotg_hub.sys|**Working**||
|LAN9514 USB Ethernet Adapter|lan9500-arm-n650f.sys|**Working**|Ethernet support for RPi 3 B|
|LAN7515 USB Ethernet Adapter|lan7800-arm-n650f.sys|**Working**|Ethernet support for RPi 3 B+|
|BCM43438 Wireless LAN|bcmdhd63.sys|**Working**|WLAN support for RPi 3 B|
|BCM43438 UART Bluetooth|BtwSerialH5Bus.sys|**Working**|depends on the PL011 UART driver|
