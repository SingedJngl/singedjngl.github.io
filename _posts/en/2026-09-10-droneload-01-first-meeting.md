---
layout: post
title: "DroneLoad #01 — Setting the frame"
date: 2026-09-10
lang: en
ref: droneload-01
categories: [droneload]
tags: [droneload, ardupilot, project-management, log]
permalink: /en/log/droneload-01-first-meeting/
cover: /assets/img/droneload.svg
cover_alt: "The whiteboard with the work packages, from the first team meeting"
excerpt_text: "First week as project lead: splitting the work into packages, assigning the team, putting the tools in place."
---

## The context

DroneLoad is our fourth-year project at ECE, a student autonomous drone competition. The drone has to fly a set course on its own, carry a payload and detect targets on the ground. There are five of us on the project and I'm the lead.

I'm starting this log to keep a record of what we do week by week. A final report rebuilds everything after the fact and smooths the mistakes out; I want the version that actually happened, including what's stuck. This is also the first time I've led a team. I'm going to get things wrong several times, so I may as well write it down as it happens.

## Splitting the project up

I got the team together this week. The goal: nobody leaves the room without knowing what they're working on.

We started by listing everything the drone has to do on the board. The split into four work packages came out of that list, each package being a block one person can carry alone:

- **P1 — Mechanics, propulsion, payload release**: design the airframe, size the propulsion chain, build the release mechanism, hold the mass budget. CAD, 3D printing, eCalc, workshop.
- **P2 — Low-level avionics**: make the drone fly stably and safely. Pixhawk 6 flight controller running ArduPilot, calibrations, PID loops, MAVLink link to the companion computer.
- **P3 — Computer vision**: detect the ground target from the drone and give navigation the lateral offset in meters. Python, OpenCV, ArUco markers or a lightweight YOLO, optimization on Raspberry Pi or Jetson.
- **P4 — GPS-denied navigation and ROS 2**: hold position without GPS and run the mission. Optical flow, LiDAR and EKF fusion, ROS 2 software architecture, mission state machine, SITL and Gazebo simulation.

These packages aren't independent. The mass P1 settles on constrains the release mechanism, and the energy budget constrains everyone. So we'll keep a shared table of mass, current draw and footprint, updated at every decision, otherwise each package moves on assumptions the others don't know about.

For the assignment, I asked everyone what they wanted to work on and followed the preferences. I don't have the criteria yet to judge anyone's level, so handing out the packages myself would amount to drawing lots. Everybody starts out motivated. In exchange, motivation doesn't guarantee competence, and I won't see that for a few weeks. That risk looks smaller to me than putting someone on a package they don't care about for six months.

There are two of us on P1, I'm alone on P2, and I add coordination on top. P2 is my comfort zone: Pixhawk, ArduPilot and PID loops are what I've been doing on personal projects for a while, so I can get the bulk of it done quickly. P1 is the long commitment: the mechanics and the propulsion will keep moving until the end, as flight tests come in and the frame and the release mechanism get iterated. The other three share P3 and P4.

## My place in the team

I'm still a fourth-year student like them, with a technical package to carry. I don't want the "I hand out tasks and supervise" mode.

In practice, decisions get discussed before they're made, and once made we move on without reopening them. Otherwise we go over the same subjects indefinitely and nothing moves. I don't know whether the balance is right. The risk I can already see is not managing to push through an unpopular decision the day I have to.

## Housekeeping

Two tools, in place from this week:

- a shared Drive for everything that isn't code: documents, CAD, meeting notes, the competition rules, photos;
- a GitHub repository for the code, organized by work package.

If it isn't set up in week 1, it never will be: everyone settles into their own habits and we spend the rest of the project looking for files.

## Making contact

I wrote to the project supervisor on the school side and to the competition organizer. Two questions: what hardware is already available or provided, and what the real dates are, intermediate milestones and competition date. Until I have those two answers, I can't build a schedule, cost a budget, or know whether we're starting from an existing base or a blank sheet.

On hardware, we're inheriting the electronics from the DroneLoad project of two years ago: a Pixhawk 6 flight controller, a Holybro PM03D v1.1 power module, four T-Motor 20 A ESCs and four T-Motor 1000KV motors. That takes a load off P2 and lightens the budget. It all still has to be checked and recalibrated after two years in a cupboard, before the base can be considered sound.

That base comes from another project, it wasn't sized for our mission. The ESC current rating and the motor KV constrain the prop and voltage pairing we can use, so P1 has to validate the propulsion chain against the target loaded mass before drawing anything. If it doesn't work out, the whole propulsion chain goes back to purchasing, and the budget with it.

The competition is sending its first information on September 19. The schedule stays on hold until then.

## What worries me

- I don't know my teammates' real technical level. On paper everyone's up for it, I'll know at the first real deliverable.
- The schedule stays empty until I have the competition dates.
- I know the part numbers of the inherited electronics, not their condition or what's missing around them. The budget waits on that.
- I've never led a team. I don't know what I'm doing wrong right now.

## Goals for week 2

- Take delivery of the inherited hardware and inspect it: condition of the four motors and the four ESCs, compatibility of the PM03D with the Pixhawk 6.
- Get the competition's information on September 19 and build a backward schedule from it.
- Write a short brief with each package: objective, deliverables, first deadlines.
- Choose the drone's overall architecture: frame base, sensors, companion computer.
