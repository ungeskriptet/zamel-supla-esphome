# zamel-supla-esphome
Minimal ESPHome configs for Supla SBW-01 and SRW-01 devices by [Zamel](https://zamel.com)

## GPIO configuration
SBW-01:
 * Relay: GPIO5
 * IN1: GPIO14
 * IN2: Unknown, maybe GPIO13?

SRW-01:
* Relay 1 (Blinds down): GPIO12
* Relay 2 (Blinds up): GPIO5
* IN1 (Down button): GPIO14
* IN2 (Up button): GPIO13

## Flashing
### Prerequisites:
* USB to TTL UART adapter. I recommend [this adapter](Resources/ft232rl.jpg), as it supports all common voltage levels.
* Computer with Python installed

### Guide:
1. Install [esptool.py](https://docs.espressif.com/projects/esptool)
2. Disassemble the Supla device. To open it up, insert a plastic card between the terminal blocks and the bottom plastic part and then try to push the plastic card to the side until the housing opens completely.
3. Solder some thin wires to the test pads on the back. A pinout can be found in the folder named according to the device. Only GND, 3V3, RXD and TXD need to be soldered
4. Connect the Supla device to the UART adapter (Don't connect the UART adapter to the computer yet). Make sure to connect GND first and that there is no external power being supplied to the device.
5. Short IO0 to GND while plugging in the UART adapter into the computer. This will boot the ESP8266 chip into flash mode. Holding down the 'CONFIG' button while connecting the UART adapter might also boot the device into flash mode.
6. Backup the firmare:
    ```
    $ esptool.py read_flash 0 ALL supla-backup.bin
    ```
    * Make sure to keep this file saved in a secure place, in case you want to revert back to the original firmware.
    * This file also contains sensitive information like your Wi-Fi SSID and password, so ensure to keep this file private.
7. Flash ESPHome (or Tasmota):
    ```
    $ esptool.py write_flash -fm dout -e 0x0 esphome.bin
    ```
8. Once it connects to your Wi-Fi, unsolder the wires from the testpads and reassemble the Supla device. The case might not hold up like before, so use some electrical tape to keep the case together.
