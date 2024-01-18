# OpenIPC Wiki
[Table of Content](../README.md)

Jienuo JN-107AR-D-WIFI
--------------------

### Device info

| System | Description                            |
|--------|----------------------------------------|
| SoC      | T31L                                 |
| Sensor   | GalaxyCore GC2083                    |
| Flash    | 8MB                                  |
| WiFi     | ZTop ZT9101U                         |
| Ethernet | 100MBit                              |

### Flashing
U-boot can be interrupted with Ctrl+C. The password is **HI2105CHIP**. Flash normally over tftp using instructions for T31L with 8MB NOR.

### GPIO
| #      | Function           |
|--------|--------------------|
| 58     | IRCUT1             |
| 57     | IRCUT2             |
| 61i    | USB_ENABLE         |

### Wifi

This camera uses a ZTop ZT9101U USB wifi module. There is no source code available, but the driver from the original firmware can be used.
GPIO 61 needs to be set LOW to provide the usb module with power:
```
root@openipc-t31:~# gpio clear 61
Setting GPIO-51 to LOW
root@openipc-t31:~# lsusb
Bus 001 Device 002: ID 350b:9101
Bus 001 Device 001: ID 1d6b:0002
```

Copy `ZT9101xV20.ko`, `wifi.cfg` and `fw/ZT9101_fw_r2685.bin` to your overlay space (e.g. `/root`).
```
root@openipc-t31:/# cd /root/zt9101-wifi/
root@openipc-t31:~/zt9101-wifi# ls
ZT9101xV20.ko  fw             wifi.cfg
```
From within this directory, run:
```
modprobe cfg80211
insmod ./ZT9101xV20.ko cfg=/root/zt9101-wifi/wifi.cfg
ifup -v wlan0
```

If this worked, it can be automated by adding the following to `/etc/wireless/usb`:
```
if [ "$1" = "zt9101-t31-camhipro" ]; then
        set_gpio 61 0
        modprobe cfg80211
        DRV_PATH=/root/zt9101-wifi
        cd $DRV_PATH
        insmod $DRV_PATH/ZT9101xV20.ko cfg=$DRV_PATH/wifi.cfg
        exit 0
fi
```
Set your `wlandev` with: `fw_setenv wlandev zt9101-t31-camhipro`, as well as `wlanssid` and `wlanpass`, then reboot.
