---
title: Week 21 Progress Report
date: 2023-10-23T14:48:24+05:30
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---
### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code and bug fixes to Gerrit
	- Tweaked UI to simplify and make text more legible.
	- Fixed SVG icon scaling issues in the dashboard (Updated Docker file to resolve dependencies)
	- Refactored subscription updates (UI updates are blocked when sending values to Kuksa).
	- Update documentation and Readme file.
- What I plan to do next 

---
## # Pushing to Gerrit

This week, major work was completed for the AGL Demo Control Panel and commits were made for review on Gerrit. The following changes have been pushed so far:

Changes pushed so far:
- 29279: agl-demo-control-panel: Refactor Settings, Config and UI scaling | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29279 (merged)
- 29291: agl-demo-control-panel: Fix Svg icons scaling on Dashboard | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29291
- 29292: agl-demo-control-panel: Update documentation | https://gerrit.automotivelinux.org/gerrit/c/AGL/documentation/+/29292

## Bug Fixes

- This week I patched a small bug where setting the temperature values would cause an exception/crash when going beyond the min/max values available. The bug was reported in [SPEC-4939](https://jira.automotivelinux.org/browse/SPEC-4939).
- I also patched the scaling issue of the SVG icons for the dashboard of the control panel using the QtSvg module of PyQt5, however on Debian-based systems, this sub-module is only available through the `python3-PyQt5.QtSvg` package. 
- Fixed a bug where `new-config` would not be created due to a *key error* when stating the client in the `AGL-kuksa-val-server` config.
## UI Tweaks

- Increased DejaVu font sizes for `QLabels` and `QPushButtons` to make the text more legible.
- Removed QScrollbar components to better adjust to smaller screens and remove clipped borders in QStacked widget frames.
- Start/Stop button was redone to help manage sessions of the client more easily.
- The client is now stops when `AGL_Demo_Control_Panel` is closed using the window controls.
## Refactored Kuksa Subscription

This week I also refactored the way subscribed updates are handled in the control panel, previously the application would both subscribe to any changes made to a signal and also set the updated values, and since the new values set on the control panel trigger a `send_values` function when an event occurs, the values would be set twice.

This week I refactored by making use of `pyqtsignals` and slotted functions which not only help in sending interpreters communication between modules but also threads. Now whenever the values are being set by the control panel, all updates made via the subscription module are blocked temporarily. This helps remove the jittery feeling in the sliders when setting new values. 

## Documentation and README

This week I also updated the documentation and README to include updated screen-shots, installation, and setup processes. This included,
- Update installation steps.
- Steps to including/defining custom config files (.ini) and where to place them.
- Fix typos in the assets directory name.

---

## # What Next?

- Issue bug fixes as required.
- Continue pushing code and documentation to Gerrit.
- Prepare for the final submission of the GSoC project evaluation.