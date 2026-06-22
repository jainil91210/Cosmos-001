# Cosmos-001: The Handheld Dual-Display AI Terminal

Hey! Welcome to the repository for Cosmos-001. 

This is a pocket-sized handheld machine I built inside a clear plastic project box. It features a dual-OLED layout for multitasking, an integrated custom keyboard, haptic feedback, and a built-in magnetometer paired with an 8x8 LED matrix for visual metal detection. 

I built this project for Hack Club. My goal here is to keep the project completely open, readable, and structured so that anyone can replicate it without running into the exact same hardware bottlenecks I dealt with.

---

## What It Actually Does

* **Dual-Screen Layout:** Runs two separate I2C OLED screens simultaneously. One display is completely dedicated to streaming text conversations with AI models via an external browser bridge, while the second acts as a system dashboard for status feeds or running retro games.
* **Capacitive Screw-Pad Keyboard:** Built using an MPR121 capacitive touch controller. Instead of using a bulky commercial keyboard, the inputs are mapped directly to physical metal screws drilled straight through the clear plastic casing.
* **8-Way DIP Switch Mode Selection:** Uses a physical vertical DIP switch to instantly swap keyboard map layers. You can toggle between lowercase, uppercase, numbers, special characters, or custom macros like a Python editor mode.
* **Visual Metal Detection:** Combines an internal magnetometer with a top-mounted 8x8 LED matrix. When you pass the device over walls or objects, the matrix lights up progressively like a signal-strength meter to help locate hidden metal screws or structural pieces.
* **Haptic Engine:** Features an internal mini vibration motor driven by a transistor circuit to provide sharp tactile feedback every time a touch key registers or an AI response comes in.

---

## Parts Inside the Box

If you want to piece this together yourself, here is exactly what I used:
* **Microcontrollers:** 1x ESP32 NodeMCU (handles the main loop, screens, and inputs) + 1x ESP8266 (offloaded to handle the magnetometer loop and matrix updates).
* **Displays:** 2x SSD1306 I2C OLED Displays (128x64) + 1x MAX7219 8x8 LED Matrix Display.
* **Sensors:** 1x MPR121 Capacitive Touch Module + 1x QMC5883L/HMC5883L Magnetometer.
* **Hardware & Discrete Components:** 1x 8-way vertical DIP switch, 1x standard tactile push button, 1x 5V coreless vibration motor, 1x 2N2222 transistor (for motor switching), 1x 1k Ohm resistor, a handful of small metal screws, and a clear plastic project enclosure.

---

## Things I Learned (Read This to Avoid Breaking Stuff)

I spent over 40 hours building, testing, and debugging this setup. Save yourself a few days of troubleshooting by reading these notes before you touch the soldering iron:

1. **Watch the plastic casing when soldering:** When mounting the custom screw-pad keyboard, you have to solder directly to the metal screws sitting inside the clear plastic box. Work fast and use high heat for short bursts. If you leave the iron on the screw for too long, the plastic box around it softens and melts instantly.
2. **The physical Enter key choice:** I originally tried using an electrode on the capacitive touch keyboard as the 'Enter' key. It was a bad idea—the touch pad was too sensitive and kept accidentally registering duplicate presses, which sent incomplete prompts over the network and wasted API credits. I stripped it out and put a reliable, physical tactile push button in its place. The tactile click gives way better control.
3. **Internal wire length matters:** Do not make your internal hookup wires exact or too short. Space gets tight inside the box very quickly. I originally cut mine short to keep things looking neat, but when I packed the ESP32 into place, the tension pulled a wire loose and created a tiny, invisible short between two transistor pins on the OLED circuit that crashed the entire I2C bus. Give yourself some slack and organize the wires with tape or ties instead.
4. **Isolate your magnetometer:** If you use a vibration motor for haptic feedback, keep it as far away from the magnetometer module as possible. The permanent magnets inside the tiny motor will completely throw off your magnetometer readings every single time the motor vibrates to confirm a keypress.

---

## Software Configuration

1. All core micro-controller configurations sit inside the `codes/` directory.
2. If you are using the Arduino IDE, make sure you install `Adafruit_SSD1306`, `Adafruit_MPR121`, and `LedControl` through the library manager first.
3. Make sure to hardcode different I2C addresses on your OLED screens (usually requires moving a physical jumper resistor on the back of one board from `0x3C` to `0x3D`) so they don't fight for the bus.
4. Flash the main ESP32 sketch first, then upload the helper matrix controller to your ESP8266.

If you get stuck trying to trace the lines inside the box, check out `hardware.md` for the exact pin tables!