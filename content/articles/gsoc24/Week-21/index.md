---
title: "Week 21"
date: 2024-10-20T15:18:11+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the twenty-first week of my Google Summer of Code (GSoC) project, I pushed the last changes for `AGL-Demo-Control-Panel` to. 
Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

#### Change log:

- Fix Visual Bugs and Add options for Keypad input

    - Fixed spin wheel input alignment for HVAC controls
    - Minor tweaks to Gauge input, Added new tick marks and improved gradient
    - Adding option(s) in config to handle Keypad input settings
    - Reconnect QML signals to enable two way input for Speed, RPM and other QML elements
    - Refactor and Add CLI option to start and stop playback.
    - Make Tire Pressure Dock into floating window and align to screen center.
    - Update resources to include keypad icons.

- Bump SRCREV

    - Bump SRCREV for agl-demo-control-panel.
    - Add cantools as runtime dependency.

- Updated Documentation
    - Explain new Config options
    - Provide Tips and Workarounds to CARLA playback steps
    - Update Steps required to run Qt6 version of Demo Control Panel

### # Code pushed for review: 
- [30437](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30437): Fix Visual Bugs and Add options for Keypad input
- [30438](https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-demo/+/30438): agl-demo-control-panel: Bump SRCREV
- [30444](https://gerrit.automotivelinux.org/gerrit/c/AGL/documentation/+/30444): (WIP) agl-demo-control-panel: Update Documentation

### # Code Merged this week:
- [30401](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30401): Add Tire Pressure, Keypad elements and misc. UI Changes.


## What I plan to do next:
- Finalize report for GSoC '24.
- Demo Setup (Video).