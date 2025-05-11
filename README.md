# gogonce

**gogonce** is a lightweight Arduino library for detecting logic level changes on digital input pins with internal pull-up resistors (`INPUT_PULLUP`).  
It applies debounce filtering and returns a **single integer value only when a change occurs** — ideal for communication with environments like **Max/MSP**, **TouchDesigner**, or **PureData**.

---

## 🔧 Features

- Monitors logic state changes (HIGH ↔ LOW) on a specified pin
- Returns an integer **only when a new stable state is detected**
- Debounce filter included (default: 5 ms, configurable)
- Does **not** send anything to Serial — you handle the result however you want

---

## 🎛️ Return behavior

For each monitored pin, `update()` will return:

- `PIN` → when logic level becomes **HIGH**
- `PIN × 10` → when logic level becomes **LOW**
- `-1` → if no change was detected

This makes it easy to route values through systems like Max/MSP or MIDI/OSC mappings with no ambiguity.

---

## 📦 Example

```cpp
#include <GogOnce.h>

// Define pins and debounce durations
GogOnce input4(4, 5);     // pin 4, debounce 5 ms
GogOnce input10(10, 10);  // pin 10, debounce 10 ms
GogOnce input12(12, 15);  // pin 12, debounce 15 ms

void setup() {
    input4.begin();
    input10.begin();
    input12.begin();
}

void loop() {
    int value;

    value = input4.update();
    if (value != -1) {
        // value is 4 (HIGH) or 40 (LOW)
    }

    value = input10.update();
    if (value != -1) {
        // value is 10 or 100
    }

    value = input12.update();
    if (value != -1) {
        // value is 12 or 120
    }
}
