---
title: "Week 20"
date: 2024-10-13T15:18:11+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the twentieth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the CARLA playback features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # Tire Pressure, Keypad elements and misc. UI Changes

This week I integrated the Tire Pressure and Keypad widgets into the control panel, refactored some UI elements and backend code and fixed bugs and issues.

<video src="./Vid.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

#### Change log:
- Increased slider grab handle size
- Add floating menu for Tire Pressure UI
- Show errow in Playback toggle when CAN interface is not available
- Update Half gauges to show Unit and logo correctly
- Update App resources
- Add new tumbler input for HVAC temp
- Add new get function to KuksaClient to get Tire Pressure unit and Current value to increment

#### Fixes:
- Check for vcar dbc file at '/etc/kuksa-dbc-feeder/'
- Increase font size in Settings page
- Allow for tumbler/ spin wheel values to wrap around

### # Code pushed for review: 
- [30401](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30401): Add Tire Pressure, Keypad elements and misc. UI Changes.

### # Code Merged this week:
- [30327](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30327): (WIP) Update Carla Playback Mode.


## What I plan to do next:
- Bug fixing and update documentation as required.
- Start drafting the final report for GSoC '24.
- Demo Setup (Video).