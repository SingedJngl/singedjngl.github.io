---
layout: post
title: "DroneLoad: running an autonomous drone project as a team of five"
ref: droneload
lang: en
permalink: /en/log/droneload/
status: "Ongoing"
cover: /assets/img/droneload.svg
cover_alt: "The DroneLoad airframe during assembly"
cover_caption: "Replace with a photo of the airframe during assembly."
excerpt_text: "A set course, a payload to carry, ground targets to detect. I'm the project lead on DroneLoad and I also own the embedded electronics. Here's how we broke the problem down."
stack: ["ArduPilot", "Pixhawk", "C++", "Python"]
---

DroneLoad is a student autonomous drone competition. The aircraft has to fly a set
course with no pilot, carry a payload and detect targets on the ground. It's my
fourth-year project at ECE Paris; we're a team of five and I'm the project lead.

So two hats: coordinating, and owning a technical scope. That balance turned out to
be harder than I expected.

## Breaking the problem down

We didn't touch any hardware in the first week. We listed what the mission actually
requires, then split it into four packages one person can carry alone:

- **Airframe and propulsion** — frame, motors and props chosen against total mass,
  payload included
- **Embedded electronics and navigation** — flight controller, sensors, firmware
- **Vision** — ground target detection
- **Release mechanism** — holding and dropping the payload

What I underestimated: these packages aren't independent. The mass the propulsion
group settles on directly constrains the release mechanism, and the energy budget
constrains everyone. We ended up enforcing a shared table of mass, current draw and
footprint, updated every time a decision is made. Not glamorous, but it saved us two
or three round trips.

## My scope: electronics and navigation

I own the flight controller and the navigation firmware, on a Pixhawk / ArduPilot
base. Choosing ArduPilot over a home-grown stack wasn't much of a debate: writing an
attitude controller from scratch would have eaten the whole semester, and the value
of this project sits in the mission, not in the inner loop.

What's left on my side:

- full configuration and calibration of the IMU and compass;
- defining the flight plan and mission logic;
- the link between the vision computer and the flight controller, so that detecting
  a target triggers an action;
- fallback modes: what the aircraft does if it loses GPS, the radio link, or both.

<!-- TODO: add the architecture diagram (flight controller / vision computer / telemetry) -->

## Still open

<!-- TODO: keep filling this in — it's the section that makes the log worth reading -->

Two questions are unresolved. The first is the endurance-versus-payload trade: every
gram of battery eats into what we can carry, and we have no in-flight current
measurements yet, so we're guessing. The second is positioning accuracy at the
moment of release, which decides whether we aim for a zone or a point.

Next step: first flights in stabilised mode to characterise real consumption, before
moving to autonomous mode.
