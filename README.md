# ExpressLRS for E32-900M30S (900MHz 1W)

Custom ELRS firmware for E32-900M30S module (SX1276, 1W output).

![E32-900M30S Module](https://raw.githubusercontent.com/ddlabus/ExpressLRS-900MHz/main/images/E32-900M30S.jpg)

## Hardware

### Receiver (RX)
- **MCU:** ESP32-C3 SuperMini
- **Radio:** E32-900M30S (SX1276, 30dBm/1W)

![ESP32-C3 SuperMini](https://raw.githubusercontent.com/ddlabus/ExpressLRS-900MHz/main/images/ESP32-C3.avif)

### Transmitter (TX)
- **MCU:** ESP32 WEMOS Lite
- **Radio:** E32-900M30S (SX1276, 30dBm/1W)

![ESP32 WEMOS Lite](https://raw.githubusercontent.com/ddlabus/ExpressLRS-900MHz/main/images/ESP32_WEMOS_LITE.jpg)

## Frequency
- Band: FCC915 (902-928MHz)

---

## Wiring Diagrams

### RX: ESP32-C3 SuperMini + E32-900M30S

```
ESP32-C3 SuperMini         E32-900M30S
┌─────────────────┐        ┌─────────────────┐
│                 │        │                 │
│ GPIO20 (RX) ────┼────────┼─ (to FC)        │
│ GPIO21 (TX) ────┼────────┼─ (from FC)      │
│                 │        │                 │
│ GPIO1  ─────────┼────────┼─ DIO0           │
│ GPIO2  ─────────┼────────┼─ DIO1           │
│ GPIO5  ─────────┼────────┼─ MISO           │
│ GPIO4  ─────────┼────────┼─ MOSI           │
│ GPIO6  ─────────┼────────┼─ SCK            │
│ GPIO7  ─────────┼────────┼─ NSS            │
│ GPIO3  ─────────┼────────┼─ RST            │
│                 │        │                 │
│ GPIO10 ─────────┼────────┼─ RXEN           │
│ GPIO0  ─────────┼────────┼─ TXEN           │
│                 │        │                 │
│ GPIO8  ─────────┼────────┼─ LED            │
│                 │        │                 │
│ 3V3    ─────────┼────────┼─ VCC (3.3V)     │
│ GND    ─────────┼────────┼─ GND            │
└─────────────────┘        └─────────────────┘
```

| ESP32-C3 Pin | E32-900M30S Pin | Function |
|--------------|-----------------|----------|
| GPIO20 | - | Serial RX (to FC) |
| GPIO21 | - | Serial TX (from FC) |
| GPIO1 | DIO0 | Radio interrupt |
| GPIO2 | DIO1 | Radio interrupt |
| GPIO5 | MISO | SPI data in |
| GPIO4 | MOSI | SPI data out |
| GPIO6 | SCK | SPI clock |
| GPIO7 | NSS | SPI chip select |
| GPIO3 | RST | Radio reset |
| GPIO10 | RXEN | PA RX enable |
| GPIO0 | TXEN | PA TX enable |
| GPIO8 | - | LED |
| 3V3 | VCC | Power 3.3V |
| GND | GND | Ground |

### TX: ESP32 WEMOS Lite + E32-900M30S

```
ESP32 WEMOS Lite           E32-900M30S
┌─────────────────┐        ┌─────────────────┐
│                 │        │                 │
│ GPIO26 ─────────┼────────┼─ DIO0           │
│ GPIO25 ─────────┼────────┼─ DIO1           │
│ GPIO19 ─────────┼────────┼─ MISO           │
│ GPIO23 ─────────┼────────┼─ MOSI           │
│ GPIO18 ─────────┼────────┼─ SCK            │
│ GPIO5  ─────────┼────────┼─ NSS            │
│ GPIO14 ─────────┼────────┼─ RST            │
│                 │        │                 │
│ GPIO13 ─────────┼────────┼─ RXEN           │
│ GPIO12 ─────────┼────────┼─ TXEN           │
│                 │        │                 │
│ GPIO22 ─────────┼────────┼─ LED            │
│ GPIO17 ─────────┼────────┼─ FAN (optional) │
│                 │        │                 │
│ 3V3    ─────────┼────────┼─ VCC (3.3V)     │
│ GND    ─────────┼────────┼─ GND            │
└─────────────────┘        └─────────────────┘
```

| ESP32 Pin | E32-900M30S Pin | Function |
|-----------|-----------------|----------|
| GPIO26 | DIO0 | Radio interrupt |
| GPIO25 | DIO1 | Radio interrupt |
| GPIO19 | MISO | SPI data in |
| GPIO23 | MOSI | SPI data out |
| GPIO18 | SCK | SPI clock |
| GPIO5 | NSS | SPI chip select |
| GPIO14 | RST | Radio reset |
| GPIO13 | RXEN | PA RX enable |
| GPIO12 | TXEN | PA TX enable |
| GPIO22 | - | LED |
| GPIO17 | - | FAN control (optional) |
| 3V3 | VCC | Power 3.3V |
| GND | GND | Ground |

---

## Flashing Firmware

### RX (ESP32-C3 SuperMini)

**Via USB-C (built-in bootloader):**
1. Hold BOOT button
2. Connect USB-C cable
3. Release BOOT button

```bash
esptool.py --port /dev/ttyACM0 --baud 460800 \
    --chip esp32c3 write_flash 0x0 ELRS_*_RX_ESP32C3_FCC915.bin
```

### TX (ESP32 WEMOS Lite)

```bash
esptool.py --port /dev/ttyUSB0 --baud 460800 \
    --chip esp32 write_flash 0x0 ELRS_*_TX_ESP32_FCC915.bin
```

### After Flashing
1. Power cycle the module
2. Connect to WiFi AP: `ExpressLRS RX` or `ExpressLRS TX`
3. Configure via web UI at `10.0.0.1`

---

## Download

See [Releases](https://github.com/ddlabus/ExpressLRS-900MHz/releases) for compiled firmware.

## Files
| File | Description |
|------|-------------|
| `ELRS_*_RX_ESP32C3_FCC915.bin` | Receiver firmware (ESP32-C3) |
| `ELRS_*_TX_ESP32_FCC915.bin` | Transmitter firmware |
| `ESP32C3_SuperMini_RX.json` | RX hardware layout |
| `E32-900M30S_TX.json` | TX hardware layout |

---

## Links
- [E32-900M30S Datasheet](https://www.cdebyte.com/products/E32-900M30S)
- [ESP32-C3 SuperMini](https://www.aliexpress.com/item/1005005967641936.html)
- [ExpressLRS Documentation](https://www.expresslrs.org/)
