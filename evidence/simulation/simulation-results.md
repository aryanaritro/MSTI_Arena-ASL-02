# Wokwi Simulation Evidence

This folder contains screenshots documenting the working Wokwi simulation for Challenge #02.

| Screenshot | Evidence |
|---|---|
| `circuit-overview.png` | Complete ESP32 circuit and component wiring |
| `button-control.png` | Independent output control using the two pushbuttons |
| `mqtt-on.png` | Both LED outputs ON after publishing MQTT payload `ON` |
| `mqtt-off.png` | Both LED outputs OFF after publishing MQTT payload `OFF` |

## Scope

These images document simulator behavior only. They do not represent physical hardware photographs or physical-device test results.

The MQTT command topic used during testing was:

`asl/msti-arena/ch02/ak-7f3a/command`

The expected command payloads were `ON` and `OFF`.
