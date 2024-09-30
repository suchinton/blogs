---
title: "Week 17"
date: 2024-09-22T17:08:54+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the seventeenth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the Keypad features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # (WIP) New QML UI

This week I continued working on the QML-based UI elements for the `AGL-Demo-Control-Panel`.  Custom gauges for Engine RPM, Speed, fuel level and coolant temp. have been implemented.

Fixes: Units and Values not updating when Parent is main application for QML elements.
Fixes: Widget not scaling to parent window size

<video src="./V1.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

Code pushed for review: [30275: (WIP) Add New Custom Gauges and CARLA playback refactored](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30275)

#### # Continued working on Coolant and Fuel Gauges: These widgets are still WIP.

<video src="./V2.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

## Next Week Tasks:

- Continue working on the new UI for the Demo Control Panel.
- Bug fixing and pushing code to gerrit for review.
