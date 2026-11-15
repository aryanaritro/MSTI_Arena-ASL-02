# System Architecture

## 1. Overview

The Smart Home MQTT Controller uses an ESP32 to receive local pushbutton inputs and remote MQTT commands. It drives two output channels represented by LEDs in the Wokwi simulation.

The system has three main functional blocks:

1. **Input handling:** reads the two button states from ESP32 GPIO input registers and applies debounce logic.
2. **MQTT communication:** connects to Wi-Fi, connects to the public MQTT broker, subscribes to the command topic, and processes received messages.
3. **Output control:** updates the two LED output pins according to local button events or MQTT commands.

## 2. Functional behavior

### Local button control

- Button 1 is connected to GPIO27 and toggles LED 1 on GPIO26.
- Button 2 is connected to GPIO34 and toggles LED 2 on GPIO25.
- Each button operates independently.
- A debounce interval of 30 ms is used to filter short transitions caused by mechanical button bounce.

### MQTT control

The ESP32 subscribes to:

`asl/msti-arena/ch02/ak-7f3a/command`

The callback interprets the received payload:

| Payload | Action |
|---|---|
| `ON` | Set both LED outputs HIGH |
| `OFF` | Set both LED outputs LOW |

The MQTT command is a global command and therefore affects both output channels together.

## 3. Input register handling

The firmware reads the ESP32 GPIO input registers using:

- `REG_READ(GPIO_IN_REG)` for GPIO pins in the lower GPIO register range.
- `REG_READ(GPIO_IN1_REG)` for GPIO pins in the higher GPIO register range.

The button state is extracted from the appropriate bit position in the register value. This satisfies the register-level input-reading requirement without using `digitalRead()`.

GPIO27 is read from `GPIO_IN_REG` using bit 27. GPIO34 is read from `GPIO_IN1_REG` using bit 2, corresponding to GPIO34's position in the upper input register.

## 4. Connectivity flow

1. The ESP32 starts and configures its input and output pins.
2. It connects to the `Wokwi-GUEST` Wi-Fi network.
3. The MQTT client is configured with the HiveMQ public broker.
4. The firmware attempts to connect to the broker and subscribes to the command topic.
5. The main loop services the MQTT client and checks the GPIO input registers.
6. Local button events and received MQTT messages update the output states.

The MQTT reconnection routine retries after a waiting interval when the client is disconnected. Extended network-loss and recovery behavior was not included in the completed test scope.

## 5. Architecture summary

```text
                 +----------------------+
                 |       MQTT Client    |
                 |  broker.hivemq.com   |
                 +----------+-----------+
                            |
                     ON / OFF messages
                            |
+----------------+   +------v-------+   +------------------+
| Button 1       |-->|              |-->| LED 1 - GPIO26   |
| GPIO27         |   |              |   +------------------+
+----------------+   |     ESP32    |
                     |              |   +------------------+
+----------------+   |              |-->| LED 2 - GPIO25   |
| Button 2       |-->|              |   +------------------+
| GPIO34         |   +--------------+
+----------------+    GPIO register
                      reads + debounce
```

## 6. Design considerations

- GPIO34 is input-only and has no internal pull-up or pull-down resistor. An external 10 kΩ pull-up to 3.3 V is used.
- GPIO27 uses the internal pull-up configuration.
- The MQTT broker is public and does not provide private topic isolation for this project. The topic should be changed to a unique value when deploying another device.
- LEDs are simulation representations of relay-controlled loads. The simulated outputs do not establish that mains or other real-world loads have been tested.
