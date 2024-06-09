---
title: "Week 02 Progress Report"
date: 2024-06-09T18:38:31+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

### # Changes Pushed to Gerrit this Week

This week I made a few revisions to last week's commit [29969: Port AGL Demo Control Panel to Qt6](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29969) based on code review in two patches:

- [Patch 2](https://gerrit.automotivelinux.org/gerrit/gitweb?p=src%2Fagl-demo-control-panel.git;a=commit;h=7dafc52e2cc8ca90ee997052c01933eace4c03e2)
```
	- Refactored set_icon function in Dashboard module to make use
	  of QIcon directly instead of using the QtSvg library 
	  (Invalid in PyQt6)
	- Syntax changes in UI_Handeler to use PyQt6
```

- [Patch 3](https://gerrit.automotivelinux.org/gerrit/gitweb?p=src%2Fagl-demo-control-panel.git;a=commit;h=3f78eac37beca55769d468d888f2fc922ad4d37c)
```
	- Update gitignore
	- Remove dependency on qtpy
```
### # Pulling Data from CARLA APIs for Recording and Playback Sessions.

This week, I created the `carla_to_CAN.py` script, which works in parallel with the [manual_control.py](https://github.com/carla-simulator/carla/blob/dev/PythonAPI/examples/manual_control.py) or [start_replaying.py](https://github.com/carla-simulator/carla/blob/dev/PythonAPI/examples/start_replaying.py) scripts. The script listens to the CARLA server, fetches the required ego vehicle data, and prints it to the console.

I referred to the existing CARLA Python API [example scripts](https://github.com/carla-simulator/carla/tree/dev/PythonAPI/examples) and the [official documentation](https://carla.readthedocs.io/en/latest/python_api/#python-api-reference) for the various APIs used in this script.

The data currently being pulled and reported to the console are:
- Vehicle Speed
- Throttle Data
- Left Indicator Status
- Right Indicator Status
- Gear Selection

## Tasks in Progress: 

### # Write out data file for playback  

This part of `carla_to_CAN.py` is still a work in progress. We are converting the extracted data into equivalent CAN signals and writing it into a CAN-dump file along with its timestamp for playback purposes.

This CAN dump file will be used by the [AGL Demo Control Panel](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/) to feed data into the CAN interface using `python-can`.

For converting the required data into CAN Frames, I'm using the [agl-vcar.dbc](https://git.automotivelinux.org/AGL/meta-agl-demo/tree/recipes-connectivity/kuksa-val/kuksa-dbc-feeder/agl-vcar.dbc) file as a reference.

### # Update Docker File

I have also started updating the Docker File to run the Qt6 port of the Control Panel properly, resolving all dependencies.

## Next Week Tasks:

- Continue work on file playback using CARLA
- Push updated Docker File
- Add new signals to fetch tire pressure