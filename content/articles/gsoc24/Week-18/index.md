---
title: "Week 18"
date: 2024-09-29T08:31:42+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the eighteenth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the CARLA playback features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # CARLA Playback in Demo Mode

This week I implemented the playback of the recorded 'can-messages.txt' file, allowing the `agl-demo-control-panel` to playback the same CAN data on its UI elements.

This includes:
- Speed Gauge
- Engine RPM Gauge
- Coolant Temp Gauge
- Fuel level Gauge
- Indicator buttons
- and Drive modes

These signals have been mapped out of the carla client and translated into their CAN message format by the `carla_to_CAN.py` module.

I also implemented the `file-playback-path` and the `can-interface` options to the `config.ini` file to allow for default configurations to be specified during the build process.

### # Gradient Effect and Progress Ticks on Gauge

<video src="./Vid.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>


### # Bug fixes:
- Incorrect encoding of Indicator states in `carla_to_CAN.py`.
- (WIP): Garbage values displayed on Half-Gauge widgets when Demo playback is enabled.

### # Code pushed for review:
- [30275](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30275): Add New Custom Gauges and CARLA playback refactored
- [30324](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30324): AGL Demo Control Panel: Enable Networking and QtQuick
- [30327](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30327): (WIP) Update Carla Playback Mode


## What I plan to do next:
- Continue working on the new UI for the Demo Control Panel.
- Bug fixing and pushing code to Gerrit.
