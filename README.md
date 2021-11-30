# Windows on ARM(64) device drivers for the Raspberry Pi platform
This repository is currently used only for publishing binary releases: https://github.com/worproject/RPi-Windows-Drivers/releases

The source code for some of the prebuilt drivers can be found here: https://github.com/raspberrypi/windows-drivers

## Notes
* most of the drivers require the *Test Signing mode* to be enabled.

* ARM32 drivers can't be installed on an ARM64 image or vice versa.

* drivers included in the latest release that can't be found in the "bsp" repository above were either extracted from old builds of Windows 10 IoT Core (ARM32 binaries) or recompiled for ARM64 by their developers (thanks to [MCCI](https://mcci.com) and [Microchip](https://www.microchip.com)).

## Status

### Raspberry Pi 4 / 400 (ARM64)

|Device|Driver|Status|Additional information|
| --- | --- | --- | --- |
|eMMC2 SDHCI|sdbus.sys (bcmemmc2.inf)|**Partially working**|a faster SD controller meant to replace SDHOST. No DMA, HS200/HS400 and UHS-I support at the moment.|
|Arasan SD/SDIO Host Controller|bcm2836sdhc.sys|**Working**||
|SD2.0 Host Controller|rpisdhc.sys|Untested|SDHOST can no longer be routed to the SD card slot (but it's available on the GPIO header)|
|GPIO|bcmgpio.sys|**Working**||
|SPI|bcmspi.sys|**Working**||
|AUXSPI|bcmauxspi.sys|**Working**||
|I2C|bcmi2c.sys|**Working**||
|PWM|bcm2836pwm.sys|**Working**||
|Audio Jack (PWM-driven)|rpiwav.sys|**Working**||
|Mini UART|pi_miniuart.sys|**Working**||
|PL011 UART|SerPL011.sys|**Working**||
|VC4 Mailbox Interface|rpiq.sys|**Working**||
|VC4 Host Interface Queue|vchiq.sys|_Not working_||
|VC4 GPU (Graphics)|roskmd.sys|_Not working_|the driver loads, but it doesn't do much as it's unfinished|
|HDMI Audio|rpi4hdmiwav.sys, rpi4hdmiwavbridge.sys|**Partially working**|only the HDMI0 port (next to USB-C) is supported|
|Basic Display Adapter (frame buffer)|MSBDD (Inbox)|**Working**||
|DesignWare HS USB 2.0 OTG Controller|mcci_dwchsotg_hcd.sys, mcci_dwchsotg_hub.sys|**Partially working**|RAM must be limited to 1 GB|
|VIA VL805 XHCI Host Controller|rpiuxflt.sys (USBXHCI.SYS filter)|**Partially working**|workaround: UASP support is disabled as it prevents booting from USB 3.0 drives. The filter driver also reduces transfer speeds quite significantly.|
|Broadcom GENET Gigabit Ethernet Controller|bcmgenet_netadapterXX.sys|**Working**|due to the fact that the NetAdapterCx API is unstable, there are 3 versions of this driver: one for build 19041/2, one for builds 19536 up to 21296, and the last one for builds 21301 and newer|
|CYW43455 Wireless LAN|No driver available|_Not working_||
|CYW43455 UART Bluetooth|cywbtserialbus.sys|**Working**||

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
|PL011 UART|SerPL011.sys|**Working**||
|VC4 Mailbox Interface|rpiq.sys|**Working**||
|VC4 Host Interface Queue|vchiq.sys|_Not working_||
|VC4 GPU (Graphics)|roskmd.sys|_Not working_|the driver loads, but it doesn't do much as it's unfinished|
|HDMI Audio|No driver available|_Not working_||
|Basic Display Adapter (frame buffer)|MSBDD (Inbox)|**Working**||
|DesignWare HS USB 2.0 OTG Controller|mcci_dwchsotg_hcd.sys, mcci_dwchsotg_hub.sys|**Working**||
|LAN9514 USB Ethernet Adapter|lan9500-arm64-n650f.sys|**Working**|Ethernet support for RPi 3 B|
|LAN7515 USB Ethernet Adapter|lan7800-arm64-n650f.sys|**Working**|Ethernet support for RPi 3 B+|
|CYW43438 Wireless LAN|No driver available|_Not working_|WLAN support for RPi 3 B|
|CYW43455 Wireless LAN|No driver available|_Not working_|WLAN support for RPi 3 B+|
|CYW43438 UART Bluetooth|cywbtserialbus.sys|**Partially working**|Bluetooth support for RPi 3 B -> the bus speed is limited as the RTS/CTS lines are not exposed (the driver may crash regardless)|
|CYW43455 UART Bluetooth|cywbtserialbus.sys|**Partially working**|Bluetooth support for RPi 3 B+ -> the bus speed is limited until hardware flow control support is added in the PL011 driver|

### Raspberry Pi 3 (ARM32)

Same as the ARM64 version, with some differences:

|Device|Driver|Status|Additional information|
| --- | --- | --- | --- |
|VC4 Host Interface Queue|vchiq.sys|_Partially working_|crashes after some usage (tested using a few userland apps ported by Microsoft)|
|DesignWare HS USB 2.0 OTG Controller|dwchsotg_hcd.sys, dwchsotg_hub.sys|**Working**||
|LAN9514 USB Ethernet Adapter|lan9500-arm-n650f.sys|**Working**|Ethernet support for RPi 3 B|
|LAN7515 USB Ethernet Adapter|lan7800-arm-n650f.sys|**Working**|Ethernet support for RPi 3 B+|
|BCM43438 Wireless LAN|bcmdhd63.sys|**Working**|WLAN support for RPi 3 B|
|BCM43438 UART Bluetooth|BtwSerialH5Bus.sys|**Working**|depends on the PL011 UART driver|
