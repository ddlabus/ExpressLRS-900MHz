# ExpressLRS 4.0 for E32-900M30S (900MHz 1W)

Custom ELRS firmware for E32-900M30S module (SX1276, 1W output).

## Hardware
- **RX:** ESP-01F (ESP8285) + E32-900M30S
- **TX:** ESP32-WROOM-32E + E32-900M30S

## Frequency
- Band: FCC915 (902-928MHz)

## Download
See [Releases](https://github.com/ddlabus/ExpressLRS-900MHz/releases) for compiled firmware.

## Files
| File | Description |
|------|-------------|
| `ELRS_4.0_RX_ESP8285_FCC915.bin` | Receiver firmware |
| `ELRS_4.0_TX_ESP32_FCC915.bin` | Transmitter firmware |
| `E32-900M30S_RX.json` | RX hardware layout |
| `E32-900M30S_TX.json` | TX hardware layout |

## Flashing
Use [esptool](https://github.com/espressif/esptool) or ELRS Configurator.
