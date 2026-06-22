# Hardware Architecture & Pin Routing

When you look inside the clear enclosure of Cosmos-001, the wiring might look chaotic due to the dense layout. To make sense of it, here is the exact pin-to-pin mapping for all components across both microcontrollers.

## 1. ESP32 Master Controller (Main Terminal & Interface)

The ESP32 acts as the main brain. It reads inputs from your capacitive keyboard and the physical Enter button, tracks the configuration state of the DIP switch, drives the haptic feedback motor, and renders data to the two primary OLED displays.

| Component Module | Component Pin | ESP32 GPIO Pin | Connection Notes |
| :--- | :--- | :--- | :--- |
| **OLED Display 1** (AI Terminal) | VCC | 3V3 | Power rail |
| | GND | GND | Ground rail |
| | SCL | GPIO 22 | Shared I2C Clock (Address: `0x3C`) |
| | SDA | GPIO 21 | Shared I2C Data (Address: `0x3C`) |
| **OLED Display 2** (Dashboard) | VCC | 3V3 | Power rail |
| | GND | GND | Ground rail |
| | SCL | GPIO 22 | Shared I2C Clock (Address: `0x3D`) |
| | SDA | GPIO 21 | Shared I2C Data (Address: `0x3D`) |
| **MPR121 Touch Module** | VCC | 3V3 | Power rail |
| | GND | GND | Ground rail |
| | SCL | GPIO 22 | Shared I2C Clock (Address: `0x5A`) |
| | SDA | GPIO 21 | Shared I2C Data (Address: `0x5A`) |
| **Tactile Enter Button** | Pin 1 | GPIO 4 | Configured as `INPUT_PULLUP` |
| | Pin 2 | GND | Triggers LOW on press |
| **8-Way DIP Switch** | Switch 1 | GPIO 12 | Mode Bit 0 (`INPUT_PULLUP`) |
| | Switch 2 | GPIO 13 | Mode Bit 1 (`INPUT_PULLUP`) |
| | Switch 3 | GPIO 14 | Mode Bit 2 (`INPUT_PULLUP`) |
| | Switch 4 | GPIO 15 | Mode Bit 3 (`INPUT_PULLUP`) |
| | Ground Side | GND | All common pins connected to ground |
| **Vibration Motor Driver** | Base | GPIO 5 | Connected via 1k Ohm resistor to 2N2222 Transistor |
| | Collector | Motor (-) | Diode in parallel with motor to protect against spikes |
| | Emitter | GND | Ground line |

---

## 2. ESP8266 Secondary Processor (Sensors & Game Engine)

The ESP8266 runs parallel tasks to offload heavy lifting from the main I2C bus. It is responsible for sampling raw magnetic field variations and updating the metal detection visualization matrix.

| Component Module | Component Pin | ESP8266 GPIO Pin | Connection Notes |
| :--- | :--- | :--- | :--- |
| **Magnetometer Module** | VCC | 3V3 | Power rail |
| | GND | GND | Ground rail |
| | SCL | GPIO 5 (D1) | Dedicated Sensor I2C Clock |
| | SDA | GPIO 4 (D2) | Dedicated Sensor I2C Data |
| **MAX7219 8x8 LED Matrix**| VCC | 5V / VIN | Needs 5V for maximum brightness |
| | GND | GND | Ground rail |
| | DIN (Data In) | GPIO 13 (D7) | SPI MOSI Hardware Line |
| | CS / LOAD | GPIO 15 (D8) | SPI Chip Select |
| | CLK | GPIO 14 (D5) | SPI Clock Hardware Line |

---

## 3. Power Architecture

* **Primary Input:** Power is supplied via the USB interface on the ESP32 NodeMCU board.
* **Power Distribution:** The 5V / VIN pin from the ESP32 supplies power directly to the 8x8 LED matrix and the ESP8266 power input. The internal 3.3V regulators on the microcontrollers power the sensitive logic modules (OLED screens, MPR121, and Magnetometer) to keep noise floors minimal.