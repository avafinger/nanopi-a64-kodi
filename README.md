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

This is a WiP (Work in Progress) and intended for learning purpose, what does not work today may work tomorrow but you can consider the kernel very stable.

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

# Pre-Release

A pre-release image for testing is here: https://github.com/avafinger/nanopi-a64-kodi/releases/tag/v1.0


# IR

ir has been tested with this procedure:

Enable the right protocols for your TV control, in this case **NEC**

    ubuntu@nanopi-a64:~$ sudo su
    root@nanopi-a64:/home/ubuntu# echo nec > /sys/class/rc/rc0/protocols
    root@nanopi-a64:/home/ubuntu# exit
    exit
    ubuntu@nanopi-a64:~$ 
  
  
Testing:

  
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
  
# Camera (OV5640)

Camera is enabled by default to work with latest OV5640 and sun6i_csi mainline kernel and you will need a special version of ffmpeg to be able to use HW encoder. Encoding via xlib64 has poor results. Be careful if you build any **ffmpeg** that can break **KODI**, KODI is highly dependent on a special ffmpeg version done by Kwiboo. Build any **ffmpeg** statically if you need ffmpeg.

    ffmpeg -video_size 640*480 -pixel_format yuv420p -i /dev/video0 out.mp4


Using **cap** also need a tweak to work with sun6i_csi.

V4L2 works somehow but there are some compatibility issues i would say.

    v4l2-ctl --list-devices
    cedrus (platform:cedrus):
      /dev/video1

    sun6i-csi (platform:csi):
      /dev/video0



Check video0 capabilities: 


    v4l2-ctl -d /dev/video0 -l

    User Controls

                           contrast 0x00980901 (int)    : min=0 max=255 step=1 default=0 value=0 flags=slider
                         saturation 0x00980902 (int)    : min=0 max=255 step=1 default=64 value=64 flags=slider
                                hue 0x00980903 (int)    : min=0 max=359 step=1 default=0 value=0 flags=slider
            white_balance_automatic 0x0098090c (bool)   : default=1 value=1 flags=update
                        red_balance 0x0098090e (int)    : min=0 max=4095 step=1 default=0 value=0 flags=inactive, slider
                       blue_balance 0x0098090f (int)    : min=0 max=4095 step=1 default=0 value=0 flags=inactive, slider
                           exposure 0x00980911 (int)    : min=0 max=65535 step=1 default=0 value=885 flags=inactive, volatile
                     gain_automatic 0x00980912 (bool)   : default=1 value=1 flags=update
                               gain 0x00980913 (int)    : min=0 max=1023 step=1 default=0 value=152 flags=inactive, volatile
                    horizontal_flip 0x00980914 (bool)   : default=0 value=0
                      vertical_flip 0x00980915 (bool)   : default=0 value=0
               power_line_frequency 0x00980918 (menu)   : min=0 max=3 default=1 value=1

    Camera Controls

                      auto_exposure 0x009a0901 (menu)   : min=0 max=1 default=0 value=0 flags=update

    Image Processing Controls

                       test_pattern 0x009f0903 (menu)   : min=0 max=4 default=0 value=0


mpjpg-streamer

    ./mjpg_streamer -i "./input_uvc.so -y 3 -r 640x480 -f 30 -q 90 -n" -o "./output_http.so -w ./www"
    MJPG Streamer Version: svn rev: 
     i: Using V4L2 device.: /dev/video0
     i: Desired Resolution: 640 x 480
     i: Frames Per Second.: 30
     i: Format............: YUV - 0x59565955
     i: JPEG Quality......: 90
     o: www-folder-path...: ./www/
     o: HTTP TCP port.....: 8080
     o: username:password.: disabled
     o: commands..........: enabled


![mpjpg-streamer](https://github.com/avafinger/nanopi-a64-kodi/raw/master/mpjpg-streamer.png)


**Limitations**

  640x480 window size only, 30 FPS or 60 FPS. Quality below average.

# Sound

  hdmi-sound works very nice, spdif / analog (jack) i could not figure out how to make it work.


# CEDRUS and Mali

This work's done by Maxime from Bootlin. 


# KODI

  Kodi 18.3 RC1 has been working fine with kernel 4.20.17 (unfortunatelly EOL) as of the tests.

  To run **Kodi** type in shell:

    kodi

  **Limitations**

  * H265 does not work. (fixed)
  * 10 bit not works. (fixed)
  * No 4K(yet)


# Patch set

  Patches has been applied manually in order to make it usefull. Contact the authors if you need help.


# Credits

* Bootlin (cedrus and mali)
* Kwiboo (ffmpeg v4l2)
* jernejsk (LibreElec)
* KODI (Kodi)
