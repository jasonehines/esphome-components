# jasonehines's esphome custom components

This repository contains a collection of my custom components
for [ESPHome](https://esphome.io/).

## 1. Usage

Use latest [ESPHome](https://esphome.io/) (at least v1.18.0)
with external components and add this to your `.yaml` definition:

```yaml
external_components:
  - source: github://jasonehines/esphome-components
```

This plugin allows to emulate TPLink HS105 type of plug
using LAN protocol with ESPHome. Especially useful where you
want to use existing software that supports these type of plugs,
but not others.

```yaml
tplink_plug:
  plugs:
    voltage: my_voltage
    current: my_current
    total: my_total
    state: relay

# Example config for Gosund SP111

sensor:
  - id: my_daily_total
    platform: total_daily_energy
    name: "MK3S+ Daily Energy"
    power_id: my_power

  - platform: hlw8012
    sel_pin:
      number: GPIO12
      inverted: true
    cf_pin: GPIO05
    cf1_pin: GPIO04
    current:
      id: my_current
      name: "MK3S+ Current"
      expire_after: 1min
    voltage:
      id: my_voltage
      name: "MK3S+ Voltage"
      expire_after: 1min
    power:
      id: my_power
      name: "MK3S+ Power"
      expire_after: 1min
      filters:
        - multiply: 0.5
    energy:
      id: my_total
      name: "MK3S+ Energy"
      expire_after: 1min
      filters:
        - multiply: 0.5
    change_mode_every: 3
    update_interval: 15s
    voltage_divider: 748
    current_resistor: 0.0012

    # {"PowerSetCal":10085}
    # {"VoltageSetCal":1581}
    # {"CurrentSetCal":3555}

binary_sensor:
  - platform: gpio
    pin:
      number: GPIO13
      mode: INPUT
      inverted: true
    name: "MK3S+ Button"
    on_press:
      - switch.toggle: relay

switch:
  - platform: gpio
    id: relay
    name: "MK3S+ Switch"
    pin: GPIO15
    restore_mode: RESTORE_DEFAULT_OFF
    icon: mdi:power-socket-eu
    on_turn_on:
      - output.turn_on: led
    on_turn_off:
      - output.turn_off: led

status_led:
  pin:
    number: GPIO00
    inverted: true

output:
  - platform: gpio
    pin: GPIO02
    inverted: true
    id: led
```

## 3. Author & License

Orginal Author: Kamil Trzciński, MIT, 2019-2021
