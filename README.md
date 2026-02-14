# ExpressLRS for E32-900M30S (900MHz 1W)

Custom ELRS firmware for E32-900M30S module (SX1276, 1W output).

![E32-900M30S Module](https://www.cdebyte.com/u_file/2010/photo/7ba4b2da2f.png)

## Hardware

### Receiver (RX)
- **MCU:** ESP-01F (ESP8285)
- **Radio:** E32-900M30S (SX1276, 30dBm/1W)

![ESP-01F](https://www.cdebyte.com/u_file/2212/photo/4b67b3f116.png)

### Transmitter (TX)
- **MCU:** ESP32-WROOM-32E
- **Radio:** E32-900M30S (SX1276, 30dBm/1W)

## Frequency
- Band: FCC915 (902-928MHz)

---

## Wiring Diagrams

### RX: ESP-01F + E32-900M30S

```
ESP-01F (ESP8285)          E32-900M30S
┌─────────────────┐        ┌─────────────────┐
│                 │        │                 │
│ GPIO3 (RX) ─────┼────────┼─ TX (to FC)     │
│ GPIO1 (TX) ─────┼────────┼─ RX (from FC)   │
│                 │        │                 │
│ GPIO4  ─────────┼────────┼─ DIO0           │
│ GPIO5  ─────────┼────────┼─ DIO1           │
│ GPIO12 ─────────┼────────┼─ MISO           │
│ GPIO13 ─────────┼────────┼─ MOSI           │
│ GPIO14 ─────────┼────────┼─ SCK            │
│ GPIO15 ─────────┼────────┼─ NSS            │
│                 │        │                 │
│ GPIO0  ─────────┼────────┼─ RXEN           │
│ GPIO2  ─────────┼────────┼─ TXEN           │
│                 │        │                 │
│ GPIO16 ─────────┼────────┼─ LED (optional) │
│                 │        │                 │
│ 3V3    ─────────┼────────┼─ VCC (3.3V)     │
│ GND    ─────────┼────────┼─ GND            │
└─────────────────┘        └─────────────────┘
```

| ESP-01F Pin | E32-900M30S Pin | Function |
|-------------|-----------------|----------|
| GPIO3 | - | Serial RX (to FC) |
| GPIO1 | - | Serial TX (from FC) |
| GPIO4 | DIO0 | Radio interrupt |
| GPIO5 | DIO1 | Radio interrupt |
| GPIO12 | MISO | SPI data in |
| GPIO13 | MOSI | SPI data out |
| GPIO14 | SCK | SPI clock |
| GPIO15 | NSS | SPI chip select |
| GPIO0 | RXEN | PA RX enable |
| GPIO2 | TXEN | PA TX enable |
| GPIO16 | - | LED (optional) |
| 3V3 | VCC | Power 3.3V |
| GND | GND | Ground |

### TX: ESP32-WROOM-32E + E32-900M30S

```
ESP32-WROOM-32E            E32-900M30S
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

## Flashing Firmware on Virgin ESP

### Requirements
- USB-TTL adapter (CH340, CP2102, FT232)
- Python 3 + esptool: `pip install esptool`

### RX (ESP-01F / ESP8285)

**Wiring for flashing:**
```
USB-TTL          ESP-01F
─────────        ───────
TX      ──────── GPIO3 (RX)
RX      ──────── GPIO1 (TX)
3V3     ──────── VCC + CH_PD (EN)
GND     ──────── GND + GPIO0 + GPIO15
```

**Important:** GPIO0 must be LOW during power-on to enter bootloader mode.

**Flash command:**
```bash
esptool.py --port /dev/ttyUSB0 --baud 460800 \
    write_flash 0x0 ELRS_*_RX_ESP8285_FCC915.bin
```

### TX (ESP32-WROOM-32E)

**Wiring for flashing:**
```
USB-TTL          ESP32
─────────        ─────
TX      ──────── GPIO3 (RX)
RX      ──────── GPIO1 (TX)
3V3     ──────── 3V3
GND     ──────── GND + GPIO0
```

**Important:** GPIO0 must be LOW during power-on to enter bootloader mode.

**Flash command:**
```bash
esptool.py --port /dev/ttyUSB0 --baud 460800 \
    --chip esp32 write_flash 0x0 ELRS_*_TX_ESP32_FCC915.bin
```

### After Flashing
1. Disconnect GPIO0 from GND
2. Power cycle the module
3. Connect to WiFi AP: `ExpressLRS RX` or `ExpressLRS TX`
4. Configure via web UI at `10.0.0.1`

---

## Download

See [Releases](https://github.com/ddlabus/ExpressLRS-900MHz/releases) for compiled firmware.

## Files
| File | Description |
|------|-------------|
| `ELRS_*_RX_ESP8285_FCC915.bin` | Receiver firmware |
| `ELRS_*_TX_ESP32_FCC915.bin` | Transmitter firmware |
| `E32-900M30S_RX.json` | RX hardware layout |
| `E32-900M30S_TX.json` | TX hardware layout |

---

## Links
- [E32-900M30S Datasheet](https://www.cdebyte.com/products/E32-900M30S)
- [ESP-01F Datasheet](https://www.cdebyte.com/products/ESP-01F)
- [ExpressLRS Documentation](https://www.expresslrs.org/)
