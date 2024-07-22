---
title: "Week 08 Progress Report"
date: 2024-07-21T11:48:05+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the eighth week of my Google Summer of Code (GSoC) project, I made significant strides in implementing the CARLA-CAN-File playback feature within the Automotive Grade Linux (AGL) ecosystem. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # Fixed file playback bug for the Control Panel

This week, I refactored the `file playback` toggle, such that now the playback of recorded CAN messages is executed on a separate `QThread`, and once the demo is toggled off, the thread process is killed.

This was implemented to avoid GUI freezing due to the App and playback being run on a single thread.

### # Implemented the Skeletal Tyre pressure Page

This week I also implemented the new `Tyre Pressure` Page for the `AGL Demo Control Panel`.

While it is not currently hooked to any `VSS` signal, once the signal has been added to the `vspec` file, I will map the relevant signals to the various widgets on the page.

### # Fixed the "Out of Range" error for the Transmission signal when CARLA is run in manual mode.

I submitted code changes to Gerrit for review and integration into the AGL demo control panel repository. The specific change can be found here :- [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): Add Python Script to Convert CARLA data into CAN messages

The `carla_to_CAN.py` script can be run run alongside an existing CARLA simulation to fetch data and convert it into CAN messages based on the [agl-vcar.dbc](https://git.automotivelinux.org/src/agl-dbc/plain/agl-vcar.dbc) file.

_NOTE_: This does *not* require the CARLA server to be running.

Bug fixes this week:
- This week's work addressed the edge case where the CARLA hero car is in manual transmission, and the provided `int` value goes out of range for the `1`: Drive signal.


    `0` = Neutral (N)
    
    `-1` = Reverse gears (R) 
    
    `126` = Park (P)
    
    `127` = Drive (D)

    `1/2/3/..` = **Manual Forward gears (M)**

- [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): Add Python Script to Convert CARLA data into CAN messages

### # Updated Settings Page.

- Improved the alignment of toggle toggle elements in the settings page.
- Added a new dropdown menu to select the playback file.

### # Resource Compilation

Since there’s no `pyrcc6` tool for PyQt6 and `pyside6-rcc` is not available with *PySide6* on the Host and Target while building AGL, We can instead use Qt’s own rcc tool to convert the `assets/res.qrc` file.

The `rcc` tool is available by default in a full Qt6 installation. Infact, the PySide6 tool , `pyside6-rcc` locates and runs the Qt6 rcc tool via subprocess, resulting in the same output.

To convert the `.qrc` file, we run the following command:

```bash
$ /usr/lib64/qt6/libexec/rcc -g python assets/res.qrc | sed '0,/PySide6/s//PyQt6/' > res_rc.py
```

Changes pushed to `meta-agl-demo` include:

- Use rcc to compile qrc resource file on host, and remove post install step to compile on target.
- Remove PySide6 as dependency for demo control panel as
      python3-qtwidgets is no longer used to provide toggle button, and pyside6-rcc is not provided.

- [30107](https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-demo/+/30107): Pre-compile resources and remove unused dependencies

## Next Week Tasks:

- Continue working on the New Page to display tyre pressure data.
- Build the Demo Control Panel image and test.
- Start work on keypad features.