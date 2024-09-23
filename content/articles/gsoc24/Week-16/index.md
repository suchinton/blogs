---
title: "Week 16"
date: 2024-09-15T23:20:51+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the fifteenth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the Keypad features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # (WIP) New QML UI

This week I continued working on the QML-based UI elements for the `AGL-Demo-Control-Panel`.  Custom gauges for Engine RPM, Speed, fuel level and coolant temp. have been implemented.

Fixes: No transparent background for QML elements; for  the time being, matching the background color of the parent window.
Fixes: Widget not scaling to parent window size

<video src="./Gauge.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

#### # Continued working on Coolant and Fuel Gauges: These widgets are still WIP.


## Next Week Tasks:

- Add a floating menu for Tyre pressure UI.
- Revisit CARLA and Kuksa bindings.
- Refine playback implementation.