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

|  SBC Dev Board tested  |    NanoPi A64          |   NanoPi A64          |
|------------------------|------------------------|-----------------------|
| kernel version         |       4.20.17          |    5.2.0-rc6          |
| gcc version            |       7.3.0            |      7.3.0            |
| display                |       hdmi             |      hdmi             |
| graphical interface    |       CLI              |      CLI              |
| KODI version           | 19.0-alpha1 / 18.3-rc1 |                       |
| idle Temp ºC / freq    |   40 ºC / ~120 Mhz   * |  40 ºC / ~120 Mhz     |
| full Temp ºC / freq    |   75 ºC / 1.15 GHz   * |  75 ºC / 1.15 GHz     |
| RAM memory usage (avg) |      75   Mbytes       |      78   Mbytes      |
| i2c                    |       yes              |      ?                |
| spi                    |                        |      ?                |
| Camera                 |   OV5640 (640x480 pix) |      ?                |
| Wifi                   |       8189es           |   not yet             |
| BT                     |       none             |    none               |
| ethernet               |       Gbps / 100Mbps   |    Gbps               |
| sound                  |   hdmi-sound           |   hdmi-sound ?        |
| ir                     |      yes               |     ?                 |
| linux-cedrus           |      yes               |     yes               |
| mali-utgard            |      Mali-400          |     ?                 |
|------------------------|------------------------|-----------------------|
| issues                 |   spdif setup not works|    POC                |
|                        |                        |                       |

# Pre-Release

A pre-release image for testing is here: https://github.com/avafinger/nanopi-a64-kodi/releases/tag/v1.0

# Release

The IMG file with Kodi on A64 is here: https://github.com/avafinger/nanopi-a64-kodi/releases/tag/v1.1

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


# Mainline Kernel 5.2.0-rc6

Bootlog: https://gist.github.com/avafinger/a7b0164e42eac5722d9a6c03965ddd41

Mainline Kernel 5.2.0-rc6 deb package:
  https://github.com/avafinger/nanopi-a64-kodi/releases/tag/v1.2


Kodi 18.3-rc1
![Kodi 2](https://github.com/avafinger/nanopi-a64-kodi/raw/master/kodi.jpg)

Screen Shot Kodi 18.3-rc1
![Kodi 1](https://github.com/avafinger/nanopi-a64-kodi/raw/master/nanopi-a64-kodi.jpg)

  * Enable Wlan0
  
        sudo apt-get install wpasupplicant

    Edit **/etc/network/interfaces** and add/change
    
    
        allow-hotplug wlan0
        iface wlan0 inet dhcp
            wpa-ssid "AP_SSID"
            wpa-psk "PASSWORD"
        # Disable power saving on compatible chipsets (prevents SSH/connection dropouts over WiFi)
        #wireless-mode Managed
        wireless-power off

    Reboot:
    
        sudo reboot
        

    Check if wlan is up:
    
        ubuntu@nanopi-a64:~$ dmesg| grep wlan
        [   23.897094] IPv6: ADDRCONF(NETDEV_CHANGE): wlan0: link becomes ready    

    Wlan0 is lazy to accquire the IP from the DHCP, it takes a few seconds.
    Check wlan0 IP:
    
        ip addr
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
               valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host 
               valid_lft forever preferred_lft forever
        2: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc mq state DOWN group default qlen 1000
            link/ether 02:ba:4d:d2:14:5b brd ff:ff:ff:ff:ff:ff
            inet 192.168.254.100/16 brd 192.168.255.255 scope global eth0
               valid_lft forever preferred_lft forever
        3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
            link/ether 34:c3:d2:13:f1:f0 brd ff:ff:ff:ff:ff:ff
            inet 192.168.254.101/16 brd 192.168.255.255 scope global wlan0
               valid_lft forever preferred_lft forever
            inet 192.168.254.102/16 brd 192.168.255.255 scope global secondary wlan0
               valid_lft forever preferred_lft forever
            inet6 fe80::36c3:d2ff:fe13:f1f0/64 scope link 
               valid_lft forever preferred_lft forever
    

     
        ip route
        default via 192.168.254.254 dev wlan0 
        192.168.0.0/16 dev wlan0 proto kernel scope link src 192.168.254.101 


# Credits

* Bootlin (cedrus and mali)
* Kwiboo (ffmpeg v4l2)
* jernejsk (LibreElec)
* KODI (Kodi)
