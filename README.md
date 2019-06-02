# Nanopi-a64 running kodi

This is a prebuilt Ubuntu 18.04 minimal image with linux 4.20.17 to run Kodi.

Kernel features on NanoPi A64:

* camera (ov5640)
* cedrus (VPU)
* mali (GPU)
* hdmi
* hdmi-sound
* wifi
* eth 100 Mbps / Gbps
* ir
* v4l2

Tested with **Kodi 19.0 ALPHA1** and **Kodi 18.3 RC1**

# Brief description of the OS Image

**not too heavy not too light** would fit precisely how we want it to be. This is a CLI (Command Line Interface) image with minimal packages so you can run Kodi while you can still have a powerful Linux to extend and play with. Eventually, this will be upgraded to Kernel 5.2 when time permits.

# What works and what not (simple table)

This is a WiP (Work in Progress) and intended for learning pourpose, what does not work today may work tomorrow but you can consider the kernel very stable.

Here is a very simple table to show the state of this work.

|  SBC Dev Board tested  |    NanoPi A64          |   BananaPi M64        |
|------------------------|------------------------|-----------------------|
| kernel version         |       4.20.17          |                       |
| gcc version            |       7.3.0            |      7.3.0            |
| display                |       hdmi             |      hdmi             |
| graphical interface    |       CLI              |      CLI              |
| KODI version           | 19.0-alpha1 / 18.3-rc1 |                       |
| idle Temp ºC / freq    |   40 ºC / ~120 Mhz   * |  40 ºC / ~120 Mhz     |
| full Temp ºC / freq    |   75 ºC / 1.15 GHz   * |  75 ºC / 1.15 GHz     |
| RAM memory usage (avg) |      75   Mbytes       |      80   Mbytes      |
| i2c                    |       yes              |      yes              |
| spi                    |                        |                       |
| Camera                 |   OV5640 (640x480 pix) |      no               |
| Wifi                   |       8189es           |                       |
| BT                     |       none             |                       |
| ethernet               |       Gbps / 100Mbps   |                       |
| sound                  |   hdmi-sound           |   hdmi-sound          |
| ir                     |      yes               |                       |
| linux-cedrus           |      yes               |                       |
| mali-utgard            |      Mali-400          |                       |
|------------------------|------------------------|-----------------------|
| issues                 |   spdif setup not works|                       |
|                        |                        |                       |


# Credits

* Bootlin (cedrus and mali)
* Kwiboo (ffmpeg v4l2)
* jernejsk (LibreElec)
* KODI (Kodi)
