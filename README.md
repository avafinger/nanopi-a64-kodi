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

# IR

ir has been tested with this procedure:

Enable the right protocols for your TV control, in this case **NEC**

`
ubuntu@nanopi-a64:~$ sudo su
root@nanopi-a64:/home/ubuntu# echo nec > /sys/class/rc/rc0/protocols
root@nanopi-a64:/home/ubuntu# exit
exit
ubuntu@nanopi-a64:~$ 
`
Testing:

`
ir-keytable -t
Testing events. Please, press CTRL-C to abort.
2031.476557: lirc protocol(nec): scancode = 0x18
2031.528357: lirc protocol(nec): scancode = 0x18 repeat
2034.773938: lirc protocol(nec): scancode = 0x5e
2034.825705: lirc protocol(nec): scancode = 0x5e repeat
2036.602136: lirc protocol(nec): scancode = 0x5e
2036.651578: lirc protocol(nec): scancode = 0x5e repeat
2106.761334: lirc protocol(nec): scancode = 0x5e repeat
2109.565189: lirc protocol(nec): scancode = 0x18
2109.617021: lirc protocol(nec): scancode = 0x18 repeat
2112.232368: lirc protocol(nec): scancode = 0x18
2113.582986: lirc protocol(nec): scancode = 0x18
2114.407440: lirc protocol(nec): scancode = 0x18
2114.459277: lirc protocol(nec): scancode = 0x18 repeat
2114.568423: lirc protocol(nec): scancode = 0x18 repeat
2114.677493: lirc protocol(nec): scancode = 0x18 repeat
2114.787119: lirc protocol(nec): scancode = 0x18 repeat
2114.895715: lirc protocol(nec): scancode = 0x18 repeat
2115.004819: lirc protocol(nec): scancode = 0x18 repeat
2116.119229: lirc protocol(nec): scancode = 0x5e
2117.288802: lirc protocol(nec): scancode = 0xd
2121.278938: lirc protocol(nec): scancode = 0x5a
2121.330503: lirc protocol(nec): scancode = 0x5a repeat
2122.939595: lirc protocol(nec): scancode = 0x5e
2122.991345: lirc protocol(nec): scancode = 0x5e repeat
2123.100414: lirc protocol(nec): scancode = 0x5e repeat
2126.369606: lirc protocol(nec): scancode = 0x4a
2129.019405: lirc protocol(nec): scancode = 0x42
2129.071248: lirc protocol(nec): scancode = 0x42 repeat
2129.931740: lirc protocol(nec): scancode = 0x52
`

# Credits

* Bootlin (cedrus and mali)
* Kwiboo (ffmpeg v4l2)
* jernejsk (LibreElec)
* KODI (Kodi)
