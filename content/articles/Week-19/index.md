---
title: Week 19 Progress Report
date: 2023-10-09T16:13:53+05:30
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---
### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code to Gerrit.
	- Refactor acceleration and script for IC page.
	- Add compatibility for Databroker.
- What I plan to do next 

---

## # Pushing to Gerrit

This week, major work was completed for the AGL Demo Control Panel and commits were made for review on Gerrit. The following changes have been pushed so far:

Changes pushed so far:
- 29255: agl-demo-control-panel: Update and add new assets | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29255 (In review)
- 29270: agl-demo-control-panel: Add grpc support for databroker | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29270

## IC acceliration and Script

The IC script and the `acceleration` function triggered by the controls in the `IC` page were refactored to better show the acceleration and deceleration of a car. The triggers for the _Cruise controls_ for the `Steering Controls` page were also reworked. The `IC Widget` module now uses a Virtual car to simulate acceleration and deceleration, for the Instrument Cluster app when the `Demo Mode` is toggled.
## Databroker

This week, grpc/databroker support was added to the `AGL demo control panel`. This included:
- Using specific JWT tokens for grpc and ws protocols.
- Making grpc the default protocol for the control panel.
- Patching an issue where the client keeps disconnecting while using grpc.

---
## # What Next?

- Improve handling for script toggle for IC page.
- Update documentation.
- Continue pushing code to Gerrit and issue patched as required.