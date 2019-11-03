# megaCUL 868 MHz
RF-Gerät zum Empfangen und Senden verschiedener 868 MHz Funkprotokolle.

![Ansicht](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/ansicht_oben.jpg)
![Ansicht](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/ansicht_unten.jpg)

## Schaltplan

![Schaltplan](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/schaltplan.png)

## Platine
[Gerberdaten](https://github.com/damianmelson/megaCUL-868MHz/tree/master/gerber) für [PCBWay](https://www.pcbway.com) und [ALLPCB](https://www.allpcb.com).

![Platine oben](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/pcb_oben.png)
![Platine unten](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/pcb_unten.png)

[Bauelemente](https://github.com/damianmelson/megaCUL-868MHz/tree/master/docs) von [reichelt elektronik](https://www.reichelt.de) und [LCSC](https://lcsc.com).

## Bootloader
[Optiboot Bootloader](https://github.com/damianmelson/megaCUL-868MHz/tree/master/bootloader) für ATmega1284P (3.3 Volt, 8 MHz, 57600 Baud).<br>
ATmega1284P Fuses:

Fuse | Wert
---- | ----
LOW  | 0xFF
HIGH | 0xDE
EXT  | 0xFD
LOCK | 0x3F

Bootloader brennen:<br>
`avrdude -c usbtiny -p m1284p -U flash:w:optiboot_atmega1284p.hex:i -U lfuse:w:0xFF:m -U hfuse:w:0xDE:m -U efuse:w:0xFD:m -U lock:w:0x3F:m`<br>

Option -c ist anzupassen an den aktuell benutzten ISP-Programmer.

## Firmware
[a-culfw](https://github.com/heliflieger/a-culfw/tree/master/culfw/Devices/megaCUL) flashen:<br>
`avrdude -p m1284p -c arduino -P /dev/ttyUSBx -b 57600 -U flash:w:megaCUL_868MHZ.hex:i`<br>

USBx ist anzupassen an die aktuell benutzte Schnittstelle.

## esp-link
[esp-link](https://github.com/jeelabs/esp-link) flashen:<br>
- GPIO0 am PSF-A85 mit GND verbinden.<br>
- 3V3, TX, RX und GND am PSF-A85 mit einem USB zu TTL Adapter (3.3V Logikpegel) verbinden.<br>
- [Transparent Bridge Firmware](https://github.com/jeelabs/esp-link/releases) flashen:<br>
`esptool.py --port /dev/ttyUSBx --baud 230400 write_flash --flash_size 1MB --flash_mode dout --flash_freq 40m 0x00000 boot_v1.6.bin 0x01000 user1.bin 0xFC000 esp_init_data_default.bin 0xFE000 blank.bin`<br>
USBx ist anzupassen an die aktuell benutzte Schnittstelle.<br>
- Verbindungen entfernen.<br>

[esp-link](https://github.com/jeelabs/esp-link) Konfiguration für megaCUL:<br>
- Home, Pin assignment

![Pin assignment](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/pin_assignment.png)

- µC Console

![µC Console](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/µc_console.png)

- Debug log

![Debug log](https://github.com/damianmelson/megaCUL-868MHz/blob/master/images/debug_log.png)

## FHEM
megaCUL in [FHEM](https://wiki.fhem.de/wiki/CUL) anlegen:<br>
`define megaCUL CUL /dev/ttyUSBx@38400 1234`<br>

USBx ist anzupassen an die aktuell benutzte Schnittstelle.<br>

megaCUL im WLAN-Modus anlegen:<br>
`define megaCUL CUL <IP>:23 1234`<br>

IP ist anzupassen an die aktuell benutzte IP-Adresse.