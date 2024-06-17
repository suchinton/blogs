---
title: "Week 03 Progress Report"
date: 2024-06-16T18:38:31+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

### # Updated Docker File

This week I Updated the Dockerfile to support the PyQt6 version of the  *`AGL Demo Control Panel`*.

[29992](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29992): Update Dockerfile for PyQt6 Compatibility

- Update packages required to run PyQt6 apps in Debian base.
- Update set up script to compile resources.
- Use Python venv to resolve requirements to avoid native package conflicts.

### # Replaced the Animated Toggle Button

I also replaced the Animated Toggle button with a new checkbox like button to provide the `On` and `Off` states of the toggle controls for `Settings Page` & the `IC Page` (Demo Toggle)

### # WIP: Using CARLA APIs to Write Out CAN files  

Last week, I created the `carla_to_CAN.py` script, which works in parallel with the [manual_control.py](https://github.com/carla-simulator/carla/blob/dev/PythonAPI/examples/manual_control.py) or [start_replaying.py](https://github.com/carla-simulator/carla/blob/dev/PythonAPI/examples/start_replaying.py) scripts. It listens to the CARLA server, fetches the required ego vehicle data, and prints it to the console.

I referred to the existing CARLA Python API [example scripts](https://github.com/carla-simulator/carla/tree/dev/PythonAPI/examples) and the [official documentation](https://carla.readthedocs.io/en/latest/python_api/#python-api-reference) for the various APIs used in this script.

The data currently being pulled and reported to the console are:
- Vehicle Speed
- Throttle Data
- Left Indicator Status
- Right Indicator Status
- Gear Selection

This week I was able to convert the extracted data into equivalent CAN signals and push it into the local can0 interface.

Next week I will continue working on and writing it into a CAN-dump file along with its timestamp for playback purposes.

This CAN dump file will be used by the [AGL Demo Control Panel](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/) to feed data into the CAN interface using `python-can`.

For converting the required data into CAN Frames, I'm using the [agl-vcar.dbc](https://git.automotivelinux.org/AGL/meta-agl-demo/tree/recipes-connectivity/kuksa-val/kuksa-dbc-feeder/agl-vcar.dbc) file as a reference.

## Next Week Tasks:

- Continue work on file playback using CARLA.
- Push changes made to Animated Toggle button on Gerrit.
- Add new signals to fetch tyre pressure.