# KMeterISO support for ESPHome

**This project is now mothballed, because the ESPHome
project has accepted it as a contribution.  Please
just use the latest stable release of ESPHome if you
want to use your KMeterISO device with an ESP32 chip.**

This project integrates the
[KMeterISO sensor from M5Stack](https://docs.m5stack.com/en/unit/KMeterISO%20Unit)
into ESPHome.

Note: this component may soon be available directly in ESPHome:
https://github.com/esphome/esphome/pull/5170 .  As such, the
repository will be archived once that is the case.

## How do I program my ESPHome device to use the sensor?

Use your own version of this sample ESPHome sketch:

```yaml
esphome:
  name: thermometer
  friendly_name: Thermometer

external_components:
  - source:
      type: git
      url: https://github.com/Rudd-O/kmeteriso
      ref: master
    components:
    - kmeteriso

esp32:
  board: nodemcu-32s
  framework:
    type: esp-idf

# Enable logging
logger:
  level: debug

# Enable Home Assistant API
api:
  encryption:
    key: !secret api_key

ota:
  password: !secret ota_password

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

button:
- platform: restart
  name: Restart
  entity_category: diagnostic
  icon: mdi:restart
- platform: safe_mode
  name: Safe mode restart
  entity_category: diagnostic
  icon: mdi:restart-alert

i2c:
  sda: GPIO21
  scl: GPIO22
  scan: true
  id: bus_a
  frequency: 100kHz

sensor:
- platform: kmeteriso
  temperature:
    name: Temperature
  internal_temperature:
    name: Internal temperature
    entity_category: diagnostic
- platform: wifi_signal
  name: "Wi-Fi signal strength"
  update_interval: 10s
  entity_category: "diagnostic"
```
