---
title: "Week 09 Progress Report"
date: 2024-07-26T23:29:15+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the ninth week of my Google Summer of Code (GSoC) project, I made significant strides in implementing the CARLA-CAN-File playback feature within the Automotive Grade Linux (AGL) ecosystem. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.


### # Successfully built the Demo Control Panel image and tested it on RPi4 and Qemu.

This week involved building and texting the AGL Demo Control Panel on actual hardware and testing it's compatibility with other AGL apps. 

### # Minor Bug fixes and Refactoring.

- Fix Playback File selection dropdown not populating when no recorded file is found.
- Fix Crash for when playback mode is not specified in `config.ini`
- Started Refactoring the `vehicle_simulator` module to simulate `tyre pressure` readings; CARLA does not provide this data vai APIs.

### # Code Merged:
- [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): Add Python Script to Convert CARLA data into CAN messages:

```bash
V1:
    - Add carla_to_CAN.py script to convert CARLA data into CAN messages
    - Add README and requirements.txt

V2:
    - Add script to record and playback messages from can interface
    - Fix mappings to agl-vcar.dbc file

V3:
    - Fix playback feature for record_playback.py
    - Update requirements.txt
    - Update README to explain setup and usage of Scripts with CARLA

V4:
    - Add file playback feature to Demo Control Panel
    - Remove dependency on numpy to calculate 
           vehicle speed, use math lib instead
    - record_playback.py can now be imported and also be 
           used in standalone mode
    - Fix: Now data is sent to CAN interface only when it 
           is updated
    - Fix: Delay is now based on previous timestamp
           and not the starting timestamp
    - Fix: Send correct Gear messages, compatible 
           with the agl-vcar signals
```

- [30107](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30107): Pre-compile resources and remove unused dependencies:

```bash
V1:
    - Use rcc to compile qrc resource file on host, and 
      remove post install step to compile on target.
    - Remove PySide6 as dependency for demo control panel 
      as python3-qtwidgets is no longer used to provide 
      toggle button, and pyside6-rcc is not provided.

V2:
    - Replace PySide6 with PyQt6 at the resouce compile 
      step
    - Add dynamic layer for meta-qt6 to provide 
      QtWidgets and QtSvg
    - Update runtime dependencies to provide bash and
      python3-rich for CLI interface
```


## Next Week Tasks:

- Work with Justin from ICS to improve the UX/UI elements.
- Continue working on the New Page to display tyre pressure data.
- Work on keypad features.