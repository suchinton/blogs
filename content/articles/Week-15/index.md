---
title: Week 15 Progress Report
date: 2023-09-09
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---

### # Topics To Be Covered In This Post
- What I did this week
	- First draft of the "AGL Demo Control Panel" documentation. 
	- Minor Adjustments to UI
	- Simplified Settings Page.
	- Refactoring the VSS subscription Module
- What I plan to do next 

---

## # First draft of the "AGL Demo Control Panel" documentation. 

I started writing the initial draft of the documentation for `AGL Demo Control Panel` by following the steps provided in the official [documentation](https://docs.automotivelinux.org/en/master/#07_How_To_Contribute/08_Adding_Documentation/) of AGL.
### cloning the docs
```bash
git clone "ssh://suchinton2001@gerrit.automotivelinux.org:29418/AGL/documentation" && scp -p -P 29418 suchinton2001@gerrit.automotivelinux.org:hooks/commit-msg "documentation/.git/hooks/"
```

## Building a local site

1. Change into the directory

    ```bash
    $ cd documentation
    ```

2. Install MkDocs and rtd-dropdown theme
    
    ```bash
    $ sudo pip install -r requirements.txt
    ```
    
3. Serve locally (default rendered at `127.0.0.1:8000/`):
    
    ```bash
    $ sudo mkdocs serve
    ```

The documentation for `AGL Demo Control Panel` will be found under `06 Component Documentation` after it has been reviewed and merged.
## # Minor Adjustments to UI

1. Sliders for the Instrument Cluster page were made wider and spaced out to enable easier touch controls.
2. QSS styling for widgets was redone to better show hower, click, and push events.
3. Resolved dependency naming and path in the `res.qrc` file.
## # Simplified Settings Page.

This week I also simplified the steps to enable a connection with the server to just two buttons where the `Reconnect` and `Refresh` buttons have now been merged into one and the UI no longer hangs while the client connects to the server.
## # Refactoring VSS subscription Module

This week I also refactored the VSS subscription module to use the `subscribe` method from the `kuksa-client` and updated the control panel widgets to show values accurately whenever a change is registered.

VSS Paths the are subscribe to are:

1. **Vehicle Speed**: "Vehicle.Speed"
2. **Engine RPM**: "Vehicle.Powertrain.CombustionEngine.Speed"
3. **Fuel Level**: "Vehicle.Powertrain.FuelSystem.Level"
4. **Coolant Temp**: "Vehicle.Powertrain.CombustionEngine.ECT"
5. **Left Temp**: "Vehicle.Cabin.HVAC.Station.Row1.Left.Temperature"
6. **Left Fan Speed**: "Vehicle.Cabin.HVAC.Station.Row1.Left.FanSpeed"
7. **Right Temp**: "Vehicle.Cabin.HVAC.Station.Row1.Right.Temperature"
8. **Right Fan Speed**: "Vehicle.Cabin.HVAC.Station.Row1.Right.FanSpeed"

---
## # What Next?

- Complete Documentation.
- Continue pushing code to Gerrit for review.