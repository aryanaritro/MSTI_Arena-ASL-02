# Pinout and Wiring

## 1. Pin assignment

| Component | GPIO | Direction | Connection / purpose |
|---|---:|---|---|
| Button 1 (gray) | 27 | Input | Button 1 signal; toggles LED 1 |
| Button 2 (green) | 34 | Input | Button 2 signal; toggles LED 2 |
| LED 1 | 26 | Output | Output channel 1 |
| LED 2 | 25 | Output | Output channel 2 |

## 2. Button wiring

### Button 1 — GPIO27

- Connect one side of the pushbutton to GPIO27.
- Connect the other side to GND.
- Configure GPIO27 using `INPUT_PULLUP`.
- The input is HIGH when released and LOW when pressed.

### Button 2 — GPIO34

- Connect one side of the pushbutton to GPIO34.
- Connect the other side to GND.
- Connect a 10 kΩ resistor between GPIO34 and 3.3 V.
- Configure GPIO34 as an input.
- The external resistor keeps the input HIGH when the button is released; pressing the button pulls it LOW.

**Important:** GPIO34 is an input-only ESP32 pin and does not support the internal pull-up/pull-down configuration. Do not omit the external pull-up resistor.

## 3. LED wiring

Each LED is connected to its assigned GPIO output through a current-limiting resistor:

- LED 1 signal: GPIO26.
- LED 2 signal: GPIO25.

Follow the polarity and resistor arrangement shown in the Wokwi circuit diagram. In the simulation, the LEDs represent the two controlled output channels.

## 4. Input logic

Both buttons use active-low logic:

| Button condition | Register bit |
|---|---|
| Released | HIGH (`1`) |
| Pressed | LOW (`0`) |

The firmware detects a press from the corresponding GPIO input-register bit and applies debounce logic before toggling the associated output.

## 5. Wiring evidence

See `../evidence/simulation/circuit-overview.png` for the complete Wokwi circuit layout.
