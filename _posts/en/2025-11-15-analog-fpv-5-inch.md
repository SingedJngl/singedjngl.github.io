---
layout: post
title: "Building and tuning a 5-inch analog FPV drone"
ref: fpv-5in
lang: en
permalink: /en/log/analog-fpv-5-inch/
cover: /assets/img/build_fpv_final.jpg
cover_alt: "The finished 5-inch FPV drone"
cover_caption: "The finished 5-inch FPV drone"
excerpt_text: "Design, assembly and configuration of a 5-inch freestyle FPV drone under Betaflight."
stack: ["Betaflight", "4-in-1 ESC", "Analog video"]
---

This drone is a five-inch freestyle build for bando flying, put together part by
part, on analog video transmission. I could have gone digital, but analog is still
cheaper, more forgiving when the signal degrades, and plenty to learn on.

## Overview
Design, assembly and configuration of a 5-inch freestyle FPV drone under Betaflight.

## Goals
- Build an FPV drone that is reliable and easy to maintain
- Optimize stability in flight
- Configure the flight controller, the ESC, the radio receiver and the VTX
- Understand PID tuning and filtering

## Parts used

| Component | Part |
|-----------|-----------|
| Frame | MotorRiot Tanq2 |
| Flight controller | Mamba MK4 H743 V2 |
| ESC | Diatone 4-in-1 F55 128K |
| Motors | Velox V2207 V2 1750KV |
| FPV camera | Foxeer T-Rex mini |
| Video transmitter (VTX) | SpeedyBee TX800 |
| Radio receiver | RadioMaster Nano ELRS RP1 2.4 GHz V2 |
| Battery | LiPo Tattu 6S 1300 mAh |
| Buzzer | Vifly Finder 2 (self-powered buzzer) |

## System architecture
The drone is built around an H7 flight controller wired to a 4-in-1 ESC, four
brushless motors, an ExpressLRS receiver, an FPV camera and an analog video
transmitter.

The wiring diagram below sums up the main power and signal connections.
<img src="{{ '/assets/img/diatone-mamba-h7-fc-flight-controller-manual-instructions-wiring.webp' | relative_url }}" alt="Wiring diagram of the FPV drone" width="700">

Main connections:
- The LiPo battery feeds the 4-in-1 ESC directly.
- The ESC powers the flight controller and talks to it.
- The motors are driven by the ESC over the DShot protocol.
- The ExpressLRS receiver talks to the flight controller in CRSF over UART.
- The FPV camera is wired to the flight controller for the OSD overlay.
- The VTX takes the video output from the flight controller and is configured in
  IRC Tramp over UART.

## Software configuration
- Firmware: Betaflight
- Video system: analog, transmitter driven over IRC Tramp
- ESC protocol: DShot 600
- Radio protocol: CRSF

On the serial ports, only two UARTs are actually in use: UART1 as *Serial RX* for the
ExpressLRS receiver, and UART3 as a *VTX (IRC Tramp)* peripheral, so channel and
output power can be set straight from the OSD. Everything else is left disabled,
which avoids conflicts at boot.
<img src="{{ '/assets/img/bf-uart.webp' | relative_url }}" alt="Betaflight Ports tab, UART1 on Serial RX and UART3 on VTX IRC Tramp" width="700">

## Tuning

### Failsafe
This is the first thing I configured, before even spinning the motors. Stage 1 puts
the roll/pitch/yaw/throttle channels back to *Auto* as soon as the signal goes
invalid, and the AUX channels stay on *Hold*. If the loss lasts more than 1.5 s,
stage 2 triggers the *Drop* procedure: the drone cuts the motors and falls on the
spot. On a freestyle field that's safer than an approximate return-to-home with no
GPS.
<img src="{{ '/assets/img/bf-failsafe.webp' | relative_url }}" alt="Betaflight Failsafe tab, stage 1 and stage 2 configured" width="700">

### PID
I started from the defaults and worked mostly with the sliders rather than touching
each term by hand. The *Master Multiplier* is up at 1.50 to compensate for the
inertia of the machine, which is on the heavy side at 720 g. The effective values
show up underneath: 67/120/49 on roll, 70/126/56 on pitch, and D at 0 on yaw as it
should be.
<img src="{{ '/assets/img/bf-pid.webp' | relative_url }}" alt="Betaflight PID Tuning tab with the sliders and the PID values" width="700">

### Gyro filters
This is the part that took the most back and forth. The RPM filter is on (3 harmonics,
120 Hz minimum) since bidirectional DShot reports motor speeds back, which allows
fairly light filtering elsewhere: a single PT1 gyro lowpass at 650 Hz and a dynamic
notch between 150 and 350 Hz. Multipliers at 1.30 on the gyro and 1.10 on the D term
— enough margin to keep the motors from running hot, without adding too much latency.
<img src="{{ '/assets/img/bf-filter.webp' | relative_url }}" alt="Betaflight Filter Settings tab, RPM filter and dynamic notch enabled" width="700">

### Rates
Rates are set to *Actual*, which has the advantage of reading directly in degrees per
second: 180 °/s of sensitivity around center, 670 °/s at full stick and 0.60 of expo
on all three axes. It stays soft around neutral for straight lines while leaving
enough to chain flips.
The "The Zone" simulator and its developer's tutorial on rates are what let me land
on the rates that suit my flying.
<img src="{{ '/assets/img/bf-rates.webp' | relative_url }}" alt="Betaflight Rate Profile Settings tab with Actual rates" width="700">

### OSD
The OSD is deliberately minimal: battery voltage, average voltage per cell, mAh drawn,
altitude, flight timer and the *LOW VOLTAGE* warning. Metric units, capacity alarm at
1300 mAh. In flight there's no time to read fifteen pieces of information, so anything
that doesn't help me know when to come back has been unchecked.
<img src="{{ '/assets/img/bf-osd.webp' | relative_url }}" alt="Betaflight OSD tab with the preview of the displayed elements" width="700">

### Miscellaneous
- Flight modes: ACRO
- Blackbox enabled, to read the logs back after tuning sessions

## Tests carried out
- Electrical continuity test
- Motor rotation direction check
- Failsafe test
- First hover
- Stability tests
- Filter and PID adjustments

## Results
- Final weight: 720 g
- Flight time: around 5 minutes of freestyle
- Behavior in flight: low latency, good response from the 1750KV motors, even if the
  weight means the inertia is noticeable
- Future improvements: I could model and print a mount for an action camera, to get a
  good-quality video record of my flights
