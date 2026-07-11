# Cosmos-001 - My Handheld AI Terminal

Hey, I'm Jainil! This is my project called Cosmos-001. It's basically a pocket-sized handheld machine that I built inside a clear plastic project box. I wanted to make a portable terminal where I can chat with AI models on the go, check live system dashboard feeds, and use a built-in magnetometer to scan for hidden screws or metal studs behind a wall. 

I made this for Hack Club Macondo. The whole thing is completely open, hand-wired, and has a custom keyboard made out of physical metal screws that I literally drilled straight through the front of the plastic casing.

---

## 🎥 Demo Videos & Project Files
I recorded a full video showing the device working and put it inside this Google Drive folder along with my actual hand-drawn wiring diagrams, schematics, and the PCB/production files:

👉 **[watch the demo videos](https://drive.google.com/drive/folders/16vVppNZ9o0LRGfaXj37Rme1s5aD400nf?usp=drive_link)**

---

## What It Actually Does

* **Dual screens:** I hooked up two separate SSD1306 OLED displays on the same I2C bus. One screen is entirely dedicated to streaming back my text conversations with the AI (using an external browser bridge), and the other screen shows my system logs or runs retro micro-games.
* **Screw-Pad Keyboard:** I used an MPR121 capacitive touch controller. Instead of buying a normal keyboard component, I drilled metal screws through the clear plastic box and wired them up inside. Touching the screw heads registers as a keypress.
* **Switching Modes:** I added an 8-way vertical DIP switch on the chassis. Flipping the switches lets me swap keyboard maps instantly from lowercase to uppercase, symbols, or custom macros for coding.
* **Metal Finder:** I paired a QMC5883L magnetometer sensor with an 8x8 LED matrix on top. When you move the box over a wall, the matrix lights up like a signal strength bar to help locate hidden metal screws or structural frames.
* **Haptic Buzz:** There is a tiny 5V coreless vibration motor driven by a 2N2222 transistor inside that gives a quick buzz feedback whenever a touch key registers or a prompt goes through.

---

## Mistakes I Made (Read this so you don't break your stuff)

I spent over 40 hours building, testing, and dealing with annoying bugs. Here is what I learned:

1. **Don't melt the box:** When you are building the screw keyboard, you have to solder your hookup wires directly to the backs of the metal screws while they are sitting inside the plastic casing. Work fast! If you hold your soldering iron on the screw for more than 2 seconds, the plastic box around it softens and melts instantly.
2. **The touch Enter key was a bad idea:** I originally tried to make the "Enter" key a touch pad just like the letters. It was way too sensitive and kept double-triggering ghost presses, which sent incomplete prompts over the network and wasted my API credits. I ripped it out and put a solid, physical tactile push button there instead. The mechanical click is way safer.
3. **Leave some slack on your wires:** Space inside a tight, hand-wired clear project box disappears immediately. I cut my internal wires too short to make it look neat, but when I shoved the ESP32 into place, the tension pulled a supply wire loose. It created an uninsulated bridge across two transistor pins and crashed the whole I2C bus. Give yourself slack and use tape to keep things isolated.
4. **Isolate your magnetometer:** Keep the haptic vibration motor as far away from the magnetometer module as humanly possible. The permanent magnets inside the tiny motor will completely throw off your sensor readings every single time the engine vibrates to confirm a keystroke.

---

## Credits & Thanks
This project was built during the Hack Club Macondo event. 
* Big thanks to the community for inspiration on the custom open hardware setup!