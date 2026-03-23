# Allwinner/Radxa A733 BSP for Linux 6.18

## Build Instructions
This repo has already simplified the build process.

Please download the official Linux 6.18.y code from tar or git to a parallel folder `linux-6.18.y`. And follow the general steps here:
```sh
git clone https://github.com/alexcaoys/allwinner-bsp.git -b linux-6.18.y --single-branch allwinner-bsp
cd linux-6.18.y
ln -s ../allwinner-bsp/ ./bsp/
git apply ./bsp/patches/*.patch
cp ./bsp/configs/linux-6.18/*.dts* arch/arm64/boot/dts/allwinner/
cp ./bsp/configs/linux-6.18/defconfig .config

# This is for native ARM64 compile, if you need CROSS_COMPILE, please also refer to other online resources.
export BSP_TOP=$PWD/bsp/
make menuconfig
make
```

## Feature Matrix
Everything here is tested on Radxa Cubie A7Z, I only have this device so I didn't really worked on device tree for A7A and A7S, **PR welcome here.**

This table adapts this page from linux-sunxi: https://linux-sunxi.org/Linux_mainlining_effort

|Driver |       |device-tree    |Status |Note           |
|-      |-      |-              |-      |-              |
|**A733**|      |               |       |               |
|ADC    |GPADC  |gpadc0         |BSP    |               |
|       |LRADC  |lradc          |OFF    |               |
|       |Thermal|ths            |BSP    |               |
|Audio  |sound  |sunxi-snd\*    |BSP    |Untested       |
|Camera |vin    |sunxi-vin\*    |UNK    |[1]            |
|Clocks |CCUs   |*ccu           |BSP    |MAIN WIP[6]    |
|CPUFreq|       |               |PATCH  |               |
|CPUIdle|       |               |UNK    |No drivers     |
|Crypto |       |sunxi-ce       |OFF    |               |
|DRM    |DE     |display-engine |BSP    |               |
|       |DI     |deinterlace    |BSP    |               |
|       |DSI    |dsi\*(combophy)|UNK    |               |
|       |EDP    |sunxi-edp      |BSP    |               |
|       |HDMI   |sunxi-hdmi     |BSP    |               |
|       |LVDS   |lvds\*         |OFF    |               |
|       |RGB    |rgb\*          |OFF    |               |
|       |TCON   |tcon-\*        |BSP    |               |
|DMA    |       |dma-v106       |BSP    |               |
|ETH    |GMAC   |gmac\*         |OFF    |               |
|GPU    |PowerVR|img-bxm-4-64   |**MAIN**|[2]           |
|HW Spinlocks|  |hwspinlock     |OFF    |               |
|I2C    |       |sun55i-a523-i2c|**MAIN**|Same as A523  |
|IOMMU  |       |iommu-v20      |BSP    |               |
|IR     |IR RX  |irrx           |UNK    |               |
|       |IR TX  |irtx           |OFF    |               |
|LEDC   |       |sunxi-leds     |BSP    |               |
|MsgBox |       |msgbox         |OFF    |               |
|NPU    |       |npu            |BSP    |[3]            |
|NSI    |       |sunxi-nsi      |BSP    |What's this?   |
|PCIE   |       |pcie           |BSP    |Untested       |
|Pinctrl|       |\*-pinctrl     |BSP    |MAIN WIP[7]    |
|PMU    |A55&A76|\*-pmu         |**MAIN**|              |
|       |PCK600 |pck600         |**MAIN**|7.1[10]       |
|PWM    |       |sunxi-pwm\*    |BSP    |               |
|RTC    |       |rtc\*          |BSP    |MAIN WIP[8]    |
|SERDES |       |cadence-\*     |BSP    |USB DP PCIE PHY|
|SID    |EFUSE  |sunxi-sid      |BSP    |Untested       |
|SPI    |       |sunxi-spi\*    |BSP    |Untested       |
|Storage|SD/MMC |sunxi-mmc\*    |BSP    |[4]            |
|       |UFS    |sunxi-ufs\*    |BSP    |Untested       |
|Timer  |SOC    |soc_timer0     |BSP    |               |
|UART   |       |uart\*         |**MAIN**|              |
|USB    |USB    |\*hci\*        |BSP    |               |
|       |USB OTG|sunxi-udc/otg  |BSP    |               |
|       |USB 3.0|dwc3           |BSP    |[5]            |
|VE     |       |sunxi-cedar-ve |BSP    |Untested       |
|Watchdog|      |wdt-v103       |BSP    |Untested       |
|**A7z Specific**||             |       |               |
|USB-C  |PHY Switcher|phy_switcher|BSP  |               |
|       |Controller|et7304      |**MAIN**|7.1[11]       |
|AXP PMU|       |axp8191        |BSP    |MAIN WIP[9]    |

**Status**: 
- BSP: BSP drivers ported for 6.18.y
- MAIN: Mainline 6.18 drivers or backported 7.x drivers (in patch)
- PATCH: Off mainline patch
- OFF: Disabled on Radxa config and/or Radxa device tree
- UNK: Unknown, disabled now, but enabled on Radxa config/dt

[1] Didn't port VIN Drivers since I don't have any camera to test. \
[2] Mainline Driver for BXM works, Mesa works but nothing display, maybe DE needs more works. \
[3] Mainline vivante,gc can be detected but crashes. Need investigation. \
[4] Mainline allwinner,sun55i-a523-mmc exists, might need more work. \
[5] Mainline dwc exists, please check linux-sunxi website.

[6] https://lore.kernel.org/linux-sunxi/20260121-a733-rtc-v1-0-d359437f23a7@pigmoral.tech/ \
[6] https://lore.kernel.org/linux-sunxi/20260310-a733-clk-v1-0-36b4e9b24457@pigmoral.tech/ \
[7] https://lore.kernel.org/linux-sunxi/20250821004232.8134-1-andre.przywara@arm.com/ \
[8] https://lore.kernel.org/linux-sunxi/20260121-a733-rtc-v1-0-d359437f23a7@pigmoral.tech/ \
[9] https://lore.kernel.org/linux-sunxi/20250813235330.24263-1-andre.przywara@arm.com/

[10] https://lore.kernel.org/linux-sunxi/20260305-b4-pck600-a733-v2-0-ba6bbed7d253@gmail.com/ \
[11] https://lore.kernel.org/linux-usb/20260220-et7304-v3-0-ede2d9634957@gmail.com/ \
[11] https://lore.kernel.org/linux-usb/20260318-husb311-v4-0-69e029255430@flipper.net/

