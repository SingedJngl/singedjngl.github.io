---
layout: post
title: "Building and tuning a 5-inch analog FPV drone"
ref: fpv-5in
lang: en
permalink: /en/log/analog-fpv-5-inch/
cover: /assets/img/fpv.svg
cover_alt: "The finished 5-inch FPV drone on a workbench"
cover_caption: "Replace with a photo of the finished aircraft."
excerpt_text: "A five-inch racing quad built part by part, on analog video. Assembly isn't the hard part — tuning and clean wiring are."
stack: ["Betaflight", "4-in-1 ESC", "Analog video"]
---

This is a standard five-inch racing quad, built part by part, on analog video
transmission. I could have gone digital, but analog is cheaper, degrades more
gracefully when the signal drops, and is plenty to learn on.

## The build

<!-- TODO: list the exact frame / motors / ESC / FC / VTX / camera -->

Nothing spectacular about the assembly itself. Two things genuinely make the
difference over time:

- **Soldering.** Clean joints on the ESC and motors are what prevent intermittent
  failures — the kind you never reproduce on the bench.
- **Wire routing.** A wire crossing the frame in the wrong place ends up in a prop,
  or injects noise into the video line.

I learned that second one by chasing interference bars in the image for a while,
which turned out to be a power lead running too close to the VTX.

## Tuning in Betaflight

This is where the time actually goes. The starting point is always the same: check
motor rotation direction and channel mapping before touching anything else — a
reversed motor is hard to spot on the ground and very easy to spot on takeoff.

Then, in order:

1. **Filtering.** Find the airframe's own vibration frequencies and filter just
   enough. Too little and the motors run hot; too much and the response goes mushy.
2. **PID.** The attitude tuning proper. You feel the result immediately, which is
   very satisfying after hours of filter work.
3. **Rates.** Stick sensitivity — purely a matter of flying taste.

<!-- TODO: add a Blackbox capture before/after filtering, it makes the point best -->

## What I took from it

This project invented nothing — the parts exist, the firmware exists. What it gave
me is a physical intuition for a control loop: understanding what "it's oscillating"
means when you feel it in the sticks, before even looking at the traces. That
carried over directly to [DroneLoad](/en/log/droneload/).
