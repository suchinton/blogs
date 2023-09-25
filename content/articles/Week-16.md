---
title: Week 16 Progress Report
date: 2023-09-16
images:
  - https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week16/GSOC Report IMG.png
---
### # Topics To Be Covered In This Post
- What I did this week
	- Pushed partial code to Gerrit for review.
	- Completed initial documentation.
	- Resolved minor bugs and UI tweaks.
	- Refactored code wherever needed.
- What I plan to do next 

---

## # Pushing code to Gerrit

This week saw major refactoring and documentation of the code for the `AGL Demo Control Panel` and partial commits were made for review on Gerrit. 

Changes pushed so far:

1. 29189: Update Resources and Requirements | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29189
2. 29196: Update extra modules | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29196
3. 29190: Update default config | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29190
## # Initial Documentation

I took the time to document each module in the code base, explaining its features and usage. The next week, these changes will be submitted to Gerrit.

I also began working on the documentation that would be released to AGL's official docs, to create a comprehensive resource that defines the project's overview, technical features, setup techniques, and usage instructions.

## # UI tweaks and Bug fixes

1. Sliders for the Instrument Cluster page were made wider and spaced out to enable easier touch controls.
2. QSS styling for widgets was redone to better show hower, click, and push events.
3. Resolved dependency naming and path in the `res.qrc` file.

## # VSS subscription

I also refactored the VSS subscription module by merging it with the `UI_Handler` module to resolve circular imports and avoid complications. The list of subscribed VSS Paths was also updated to include:

- "Vehicle.Speed"
- "Vehicle.Powertrain.CombustionEngine.Speed"
- "Vehicle.Body.Lights.DirectionIndicator.Left.IsSignaling"
- "Vehicle.Body.Lights.DirectionIndicator.Right.IsSignaling"
- "Vehicle.Body.Lights.Hazard.IsSignaling"
- "Vehicle.Powertrain.FuelSystem.Level"
- "Vehicle.Powertrain.CombustionEngine.ECT"
- "Vehicle.Powertrain.Transmission.SelectedGear"
- "Vehicle.Cabin.HVAC.Station.Row1.Left.Temperature"
- "Vehicle.Cabin.HVAC.Station.Row1.Left.FanSpeed"
- "Vehicle.Cabin.HVAC.Station.Row1.Right.Temperature"
- "Vehicle.Cabin.HVAC.Station.Row1.Right.FanSpeed"

After validating the app's functionality in the target environment, any changes issued to these signals will be reflected on the GUI.

_Note_: It is a known issue that every time the call-back function receives an update the value is updated again (double setting), but this will be resolved by simply adding flags in a later patch to the `UI_Handler` module.

---
## # What Next?

- Continue pushing code to Gerit and issue patched as required.
- Start preparing the final report for GSoC.

---

[Week 17 Report →](/articles/week-17)

[← Week 16 Report](/articles/week-15)