---
layout: page
title: "Flux Light Adjustment"
description: "Instructions on how to have switches call command line commands."
date: 2016-06-01 17:41
sidebar: true
comments: false
sharing: true
footer: true
ha_category: Automation
ha_release: 0.21
logo: home-assistant.png
ha_qa_scale: internal
---

The `flux` switch platform will change the temperature of your lights similar to the way flux works on your computer, using circadian rhythm. They will be bright during the day, and gradually fade to a red/orange at night.

The component will update your lights based on the time of day. It will only affect lights that are turned on and listed in the flux configuration.

During the day (in between `start time` and `sunset time`), it will fade the lights from the `start_colortemp` to the `sunset_colortemp`.  After sunset (between `sunset_time` and `stop_time`), the lights will fade from the `sunset_colortemp` to the `stop_colortemp`. If the lights are still on after the `stop_time` it will continue to change the light to the `stop_colortemp` until the light is turned off. The fade effect is created by updating the lights periodically.

The color temperature is specified kelvin, and accepted values are between 1000 and 40000 kelvin. Lower values will seem more red, while higher will look more white.

If you want to update at variable intervals, you can leave the switch turned off and use automation rules that call the service `switch.<name>_update` whenever you want the lights updated, where `<name>` equals the `name:` property in the switch configuration.

To use the Flux switch in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: flux
    lights:
      - light.desk
      - light.lamp
```

{% configuration %}
lights:
  description: array list of light entities.
  required: true
  type: list
name:
  description: The name to use when displaying this switch.
  required: false
  default: Flux
  type: string
start_time:
  description: The start time.
  required: false
  type: time
stop_time:
  description: The stop time.
  required: false
  type: time
start_colortemp:
  description: The color temperature at the start.
  required: false
  default: 4000
  type: integer
sunset_colortemp:
  description: The sun set color temperature.
  required: false
  default: 3000
  type: integer
stop_colortemp:
  description: The color temperature at the end.
  required: false
  default: 1900
  type: integer
brightness:
  description: The brightness of the lights.
  required: false
  type: integer
disable_brightness_adjust:
  description: If true, brightness will not be adjusted besides color temperature.
  required: false
  type: boolean
mode:
  description: Select how color temperature is passed to lights. Valid values are `xy`, `mired` and `rgb`.
  required: false
  default: xy
  type: string
transition:
  description: Transition time in seconds for the light changes (high values may not be supported by all light models).
  required: false
  default: 30
  type: integer
interval:
  description: Frequency in seconds at which the lights should be updated.
  required: false
  default: 30
  type: integer
{% endconfiguration %}

Full example:

```yaml
# Example configuration.yaml entry
switch:
  - platform: flux
    lights:
      - light.desk
      - light.lamp
    name: Fluxer
    start_time: '7:00'
    stop_time: '23:00'
    start_colortemp: 4000
    sunset_colortemp: 3000
    stop_colortemp: 1900
    brightness: 200
    disable_brightness_adjust: True
    mode: xy
    transition: 30
    interval: 60
```
