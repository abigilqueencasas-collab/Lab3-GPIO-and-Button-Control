# BCA188 - IoT Firmware Programming and Device I/O
## Laboratory Activity 3: GPIO and Button Control

---

## Task
Assemble the button and status LED circuit, run Example 2, then add a second LED that shows the opposite state.

## Materials
| Qty | Item |
|---|---|
| 1 | ESP32 DevKit (WROOM-32 family) |
| 1 | USB cable |
| 1 | Breadboard |
| 2 | LEDs |
| 2 | 330 Ω resistors |
| 1 | Pushbutton (normally open) |
| 5-6 | Jumper wires |


<img width="2720" height="1360" alt="lab3_gpio_button_circuit_diagram" src="https://github.com/user-attachments/assets/ae64c802-9b31-4d1d-96a7-f53f8d5c219a" />


## Observation table
| Button state | GPIO23 reading | LED 1 (GPIO18) | LED 2 (GPIO19) |
|---|---|---|---|
| Released | HIGH | OFF | ON |
| Pressed | LOW | ON | OFF |
| After reset (not pressed) | HIGH | OFF | ON |

## Questions

### 1. Explain the meaning of HIGH and LOW for the button.

HIGH and LOW are the two logic levels a digital pin can read. **HIGH** means the pin is at about 3.3 V (logic 1), and **LOW** means the pin is at about 0 V, or ground (logic 0).

In this circuit, the button is connected between GPIO23 and GND, and the pin is configured with `pinMode(BUTTON_PIN, INPUT_PULLUP)`. This turns on the ESP32's internal pull-up resistor, which pulls GPIO23 up to 3.3 V.

- **Released (HIGH):** the button is open, so nothing connects the pin to ground. The internal pull-up holds GPIO23 at 3.3 V, and `digitalRead(BUTTON_PIN)` returns HIGH.
- **Pressed (LOW):** the button closes and connects GPIO23 directly to GND. The pin is pulled down to 0 V, and `digitalRead(BUTTON_PIN)` returns LOW.

This arrangement is called an **active-low** input, because the button is "active" (pressed) when the pin reads LOW. That is why the code checks `digitalRead(BUTTON_PIN) == LOW` to detect a press.

The pull-up is also needed to keep the input stable. Without a pull resistor, an unconnected input would float and could read HIGH or LOW at random because of electrical noise, which would make the LEDs flicker while the button is untouched.

## Success check
- [✓] The two LEDs show opposite states
- [✓] The input does not change randomly when released
