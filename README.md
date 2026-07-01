# Cosmos-001: Handheld Dual-Display AI Terminal

Cosmos-001 is a pocket-sized, dual-display handheld hardware platform built inside a custom clear enclosure. It serves as a portable AI terminal, an environmental magnetic scanner, and a modular development tool. 

The system features a dual-OLED multitasking array, a hand-wired capacitive touch screw-matrix keyboard, haptic feedback integration, and a physical magnetometer coupled with an 8x8 LED matrix for visual metal detection.

---

## Specifications

| Item | Details |
| :--- | :--- |
| **MCU 1 (Master)** | ESP32 NodeMCU (Dual-Core 32-bit) |
| **MCU 2 (Sensors)** | ESP-12S (ESP8266 Wi-Fi) |
| **Primary Display** | SSD1306 I2C OLED (128x64) |
| **Secondary Display**| SSD1306 I2C OLED (128x64) |
| **Sensor Display** | MAX7219 8x8 LED Matrix |
| **Logic Voltage** | 3.3V |
| **Sensors** | QMC5883L 3-Axis Magnetometer |
| **Input Interface** | MPR121 Capacitive Matrix + 1x Tactile Button |

---

## Features

* **Dual-OLED Multitasking:** Run core prompt streaming arrays on the primary terminal display while monitoring hardware logs or micro-games on the secondary display.
* **Capacitive Screw Keyboard:** Custom key matrix utilizing physical metal chassis screws drilled directly through the face of the clear enclosure, decoded via the MPR121.
* **8-Way Mode Layering:** A physical vertical DIP switch swaps keyboard mapping configurations instantly between lowercase, uppercase, symbols, and custom macro modes.
* **Subsurface Magnetic Finder:** Offloaded processing cycle reads raw magnetic fields to visualize hidden screws or structural metal points directly onto the 8x8 matrix.
* **Haptic Pulse Engine:** Low-latency micro-vibration feedback maps satisfying tactical clicks to valid character inputs and data transmissions.

---

## Repository Structure

* `codes/` — Main loop execution, keyboard maps, and terminal UI source code.
* `codes/` — Secondary sensor parsing loop and MAX7219 display multiplexer.
* `codes/` — Circuit pinout maps and physical structural mounting details.
* `codes/ai integration` — Web-console local API endpoint bridge script layout.

---

## Bill of Materials (BOM)

| Item Qty | Value/Part Name | Designator | Package Type | Estimated Unit Cost (USD) |
| :--- | :--- | :--- | :--- | :--- |
| 1 | ESP32 NodeMCU | U1 | DevBoard Modules | 3.50$ |
| 1 | ESP-12S Module | U2 | SMD-Wireless | 1.70$ |
| 2 | SSD1306 OLED Screen | DISP1, DISP2 | I2C Module 0.96" | 2.20$ |
| 1 | MAX7219 8x8 Matrix | DISP3 | LED-Matrix-Module | 1.50$ |
| 1 | MPR121 Touch Sensor | U3 | I2C Breakout Board | 1.30$ |
| 1 | QMC5883L Magnetometer| U4 | Sensor Breakout | 1.10$ |
| 1 | 8-Way Vertical DIP | SW1 | DIP-8 Through-Hole | 0.40$ |
| 1 | Tactile Push Button | SW2 | 6x6x5mm Tact Button | 0.05$ |
| 1 | Micro Vibration Motor| MOT1 | Coreless Cylinder | 0.60$ |

---

## Crucial Build Notes & Hardware Gotchas

These are the real lessons learned over 40+ hours of physical prototyping and debugging. Read these carefully before building your own unit:

1. **The Screw Solder Problem:** When attaching wire leads to the chassis keyboard screws inside the box, you must work incredibly fast with a hot iron. Leaving the tip on the screw head for more than 2-3 seconds will melt the clear enclosure instantly.
2. **The "Enter Key" Shift:** Do not map the final execution 'Enter' trigger to a capacitive touch electrode. It is too sensitive, leading to ghost double-triggers that swamp the web socket and waste API credits. Use a physical tactile push button instead.
3. **Internal Wire Slack:** Space inside a tight, hand-wired clear project box gets restricted immediately. Do not cut wire lengths exactly to size. Shifting internal components can stretch short wires, causing them to break or create invisible shorts across components. Give yourself slack.
4. **Magnetic Field Separation:** Keep the haptic vibration motor as physically isolated from the magnetometer module as possible. The permanent fields inside the motor casing will skew sensor calibration cycles whenever haptic pulses activate.

---

## Contributing

Contributions, software forks, and structural remixes are welcome! Please read the `CONTRIBUTING.md` guide to get started.

## Credits

![Hack Club](https://img.shields.io/badge/Hack_Club-Macondo-red)
![Designed In](https://img.shields.io/badge/Designed_In-Hand__Wired-blue)
![Cased In](https://img.shields.io/badge/Cased_In-Clear__Box-orange)

This project was created during the Hack Club Macondo event.

* **jainil91210** - Core firmware design, hardware prototyping, and chassis mapping.
* Inspiration and readme structure styles adapted from the community open-source devboard design layouts.