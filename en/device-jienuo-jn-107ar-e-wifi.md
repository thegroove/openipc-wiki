# OpenIPC Wiki
[Table of Content](../README.md)

Jienuo JN-107AR-E-WIFI
--------------------

### Device info

| System | Description                            |
|--------|----------------------------------------|
| SoC      | T31X                                 |
| Sensor   | SmartSens SC5235                     |
| Flash    | 16MB (MD25Q128SIG)                   |
| WiFi     | RTL8731BU                            |
| Ethernet | 100MBit                              |

### Flashing
U-boot can be interrupted with Ctrl+C. The password is **HI2105CHIP**. Flash normally over tftp using T31X with 16MB NOR instructions.

### GPIO
| #      | Function           |
|--------|--------------------|
| 58     | IRCUT1             |
| 57     | IRCUT2             |
