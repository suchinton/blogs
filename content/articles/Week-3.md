---
title: "Week 3 Progress Report"
date: 2023-06-16
images: 
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week3/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Added Settings Window to main Application
	- Completed Support for IC Application Signals
	- Added custom handles for window
	- Uploaded GitHub Readme.md
	- Building AGL-demo-platform Image 
- What I plan to do next 

---

## # AGL-Demo-Control-Panel

### Added Settings Window to main Application

The settings window provides various options to  configure the kuksa-client. functionalities are:

- Start Connection
- Create Network Bridge
- Reconnect
- Refresh
- Input IP Address
- Input JWT Token Path for Authorization
- Display Connection Status

### IC Kuksa Signals

This week I improved the style sheet for the IC page to better integrate with the  application. All visible elements in the [IC application]() are now supported by the [AGL-Demo-Control-Center](), including the Indicator buttons, Gear selector, Speed, RPM, Coolant temp, and Fuel level. However some of them do not respond to the changes made to the VSS signal values, this may be due to usage of outdated VSS paths or that the application simply doesn't subscribe to the signals. 

Signals being used on the IC Page are:
- **Vehicle Speed**: "Vehicle.Speed"
- **Engine RPM**: "Vehicle.Powertrain.CombustionEngine.Speed"
- **Left Indicator**: "Vehicle.Body.Lights.DirectionIndicator.Left.IsSignaling"
- **Right Indicator**: "Vehicle.Body.Lights.DirectionIndicator.Right.IsSignaling"
- **Hazard**: "Vehicle.Body.Lights.Hazard.IsSignaling"
-  **Fuel Level**: "Vehicle.Powertrain.FuelSystem.Level"
- **Coolant Temp**: "Vehicle.Powertrain.CombustionEngine.ECT"
- **Selected Gear**: "Vehicle.Powertrain.Transmission.SelectedGear"

### Custom Handles for Window

This week I also added custom handles for resizing the application since we do not use native window titles for the Application. This helps in resizing the window. I also added better animations for the window controls.

### HVAC Controls

Initial support for the HVAC signals was introduced, and the [HVAC Dashboard](https://github.com/hritik-chouhan/HVAC_dashboard) Application was used as a reference for the same.

### Bug Fixes

- `Kuksa_Instance` was updated to better handle client instances when reconnecting to the server.
- `settings` module now better reflects the connection status.
- Fixed the crash problem when the server is not found.

### # Demo
<video src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week3/DemoVideo.mp4" controls="controls" style="max-width: auto;">
</video>

## # GitHub Readme

The GitHub README.md was updated to showcase the application's features and also provide instructions to install on the system and configure it.

→ It can be found on the [GitHub repo](https://github.com/suchinton/AGL_Demo_Control_Panel)

## # Building AGL-demo-platform

I also built the [AGL-demo-platform](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Building_for_x86_%28Emulation_and_Hardware%29/) from the master branch using the documentation, and am in the process of building the  [Flutter IVI dashboard demo applications](https://docs.automotivelinux.org/en/master/#01_Getting_Started/03_Build_and_Boot_guide_Profile/03_IVI_Flutter_apps/). However, building that image gives me a single error for `flutter-gallery_git` package during the build process. I will work on that in the coming week.

---

## # What Next?

-  Start work on HVAC controls
-  Enable secure communication with kuksa (WS)
-  Save Configurations
-  Improve UI & UX
-  Test Application on hardware using LAN cable

---
## # References:

- [docs.automotivelinux.org](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Building_for_x86_%28Emulation_and_Hardware%29/)
- https://github.com/eclipse/kuksa.val/tree/master/kuksa-val-server
- https://github.com/hritik-chouhan/HVAC_dashboard
- https://github.com/hritik-chouhan/DashBoard-app
- https://github.com/aakash-s45/ic

---
[Week 4 Report →]() To be Updated!

[← Week 2 Report](articles/week-2)