# Test Results

## 1. Test environment

| Item | Configuration |
|---|---|
| Platform | Wokwi ESP32 simulation |
| Board | ESP32 |
| Wi-Fi SSID | `Wokwi-GUEST` |
| MQTT broker | `broker.hivemq.com` |
| MQTT port | `1883` |
| MQTT topic | `asl/msti-arena/ch02/ak-7f3a/command` |

## 2. Verified tests

| Test | Action | Observed result | Status |
|---|---|---|---|
| GPIO input detection | Press and release both buttons | Both button inputs were detected using GPIO input-register reads | PASS |
| Button 1 control | Press gray button on GPIO27 | LED 1 on GPIO26 toggled independently | PASS |
| Button 2 control | Press green button on GPIO34 | LED 2 on GPIO25 toggled independently | PASS |
| Wi-Fi connection | Start the simulation | ESP32 connected to `Wokwi-GUEST` | PASS |
| MQTT connection | Start MQTT client | ESP32 connected to the HiveMQ broker | PASS |
| MQTT subscription | Connect to broker | Client subscribed to the configured command topic | PASS |
| MQTT ON command | Publish `ON` to the command topic | Both LEDs turned ON | PASS |
| MQTT OFF command | Publish `OFF` to the command topic | Both LEDs turned OFF | PASS |

## 3. Evidence

The following screenshots are included in `evidence/simulation/`:

- `circuit-overview.png` — complete simulated circuit.
- `button-control.png` — local button operation.
- `mqtt-on.png` — both outputs ON after an MQTT ON command.
- `mqtt-off.png` — both outputs OFF after an MQTT OFF command.

See [`../evidence/simulation/simulation-results.md`](../evidence/simulation/simulation-results.md).

## 4. Tests not performed

The following tests were not included in the completed test scope:

- Disconnecting the MQTT broker or network and measuring recovery.
- Verifying automatic reconnection after a prolonged outage.
- Physical hardware testing with an actual ESP32 board and relay modules.

These are intentionally reported as **not tested**. No physical-hardware results are claimed.

## 5. Result summary

The core functional requirements were verified in Wokwi: register-based button detection, independent local control of the two outputs, Wi-Fi/MQTT connectivity, and global MQTT ON/OFF control.
