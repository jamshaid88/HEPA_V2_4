# 🌬️ Smart Air Purifier Firmware

### Intelligent Air Quality Controller with EEPROM State Persistence

![Platform](https://img.shields.io/badge/platform-Arduino-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/build-stable-brightgreen)

---

## 🧠 Overview

This firmware powers a smart air purifier capable of automatically adjusting fan speed based on real-time air quality (PM2.5).  
It provides AQI-based LED visualization, persistent state recovery via EEPROM, and multiple user modes (Auto, Manual, Sleep, Timer).

---

## ⚙️ Features

- ✅ Auto / Manual / Sleep / Timer Modes
- ✅ EEPROM Save/Restore of last operating state
- ✅ AQI-based fan speed control
- ✅ Dynamic LED color transitions
- ✅ HEPA filter status detection
- ✅ Smooth non-blocking operation (no delay loops)

---

## 🪛 Hardware Requirements

- ATmega32A / Arduino-compatible MCU
- PMS5003 particulate sensor
- PCF8574 I/O Expander
- FastLED-compatible RGB LEDs
- Display (TFT or OLED with Adafruit_GFX support)
- ACS current sensor for HEPA filter monitoring

---

## 🧩 Firmware Structure

| File                        | Description                  |
| --------------------------- | ---------------------------- |
| `src/main.cpp`              | Core logic and event loop    |
| `docs/firmware_overview.md` | Full documentation           |

---

## 🧠 EEPROM Logic

| Mode        | Stored                   | Restored                   |
| ----------- | ------------------------ | -------------------------- |
| Power OFF   | `MODE_OFF`               | Stays off after power loss |
| Auto Mode   | `MODE_AUTO`              | Auto resumes               |
| Manual Mode | `MODE_MANUAL`, fan speed | Manual resumes same speed  |

---

## 🛠️ Setup & Compilation

1. Install required libraries:
   - `FastLED`
   - `Adafruit_GFX`
   - `Wire`
   - `EEPROM`
   - `PCF8574`
2. Open `src/main.cpp` in platformIO.
3. Select the correct board and COM port.
4. Upload firmware.

---

## 📚 Documentation

- [Full Firmware Overview](docs/firmware_overview.md)

---

## 🧾 License

MIT License – free for personal and commercial use.

---

## 🧑‍💻 Author

**Jamshaid** – PCB designer & embedded systems developer
