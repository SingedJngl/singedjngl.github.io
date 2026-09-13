---
layout: post
title: "Taking control of a Gaggia Classic with an ESP32"
ref: gaggia-pid
lang: en
permalink: /en/log/gaggia-esp32-pid/
status: "Ongoing"
cover: /assets/img/gaggia.svg
cover_alt: "The Gaggia Classic opened up, with the ESP32 board next to it"
cover_caption: "Replace with a photo of the machine opened up, board visible."
excerpt_text: "Out of the box, a Gaggia Classic regulates temperature with a bimetallic thermostat: roughly ±10 °C around setpoint. I replaced it with a PID loop on an ESP32, a PT1000 probe and a solid-state relay."
stack: ["ESP32", "MAX31865", "PT1000", "PID", "MQTT", "Grafana"]
---

The Gaggia Classic is a single-boiler espresso machine, well known for being solid
and easy to open up. Its weak point is equally well known: temperature is regulated
by a bimetallic thermostat that opens and closes the heating circuit over a band of
about ±10 °C. For hot water that's fine; for extraction it isn't — contact
temperature directly affects what ends up in the cup.

So the project is easy to state: measure properly, and drive properly.

## The measurement chain

The original thermostat is replaced by a **PT1000** probe screwed into the boiler,
read through a **MAX31865** over SPI. Two reasons for that over a thermocouple: the
PT1000 is more linear across the range I care about (90–150 °C), and the MAX31865
handles a three-wire connection, which compensates for lead resistance.

One thing I learned the hard way: the probe measures boiler body temperature, not
the temperature of the water going through the group head. There's an offset, and
it isn't constant — it depends on how long it's been since the last shot.

<!-- TODO: add the measured curve, boiler temperature vs group head output -->

## The control loop

The heating element is driven by a **solid-state relay**, switched by the ESP32.

A few things I hadn't anticipated when writing the PID:

- **Anti-windup.** At power-on the error is huge and the integral term charges up
  during the entire warm-up. Without clamping I, the machine overshoots the setpoint
  by a wide margin before settling.
- **Dual setpoint.** Brewing and steaming don't want the same temperature; moving
  between them is a transition to handle explicitly, not just a variable swap.
- **Software watchdog.** Firmware that crashes while leaving the relay closed, on a
  heating element, is not a harmless bug. The watchdog cuts the heater if the loop
  stops running.

<!-- TODO: state the Kp/Ki/Kd gains chosen and how they were tuned -->

## Instrumenting the shot

Beyond temperature, I'm adding a pressure sensor and a flow meter sampled during
extraction. The goal isn't closed-loop control — not yet — but observation: being
able to compare two shots and understand what changed.

Telemetry goes out over **MQTT** and is plotted in **Grafana**. That's overkill for
a coffee machine, and I'll own that: it was also a chance to build a collection
pipeline properly.

## What's left

The firmware runs on the Arduino framework. Still to do: a repeatable extraction
profile, and an interface slightly less rudimentary than a serial link.
