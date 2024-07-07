---
title: "Week 06 Progress Report"
date: 2024-07-06T15:39:36+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the sixth week of my Google Summer of Code (GSoC) project, I made significant strides in implementing the CARLA-CAN-File playback feature within the Automotive Grade Linux (AGL) ecosystem. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # CARLA Playback

I submitted code changes to Gerrit for review and integration into the AGL demo control panel repository. The specific change can be found here :- [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): Add Python Script to Convert CARLA data into CAN messages

#### Conversion of CARLA data to CAN messages

The `carla_to_CAN.py` script can be run run alongside an existing CARLA simulation to fetch data and convert it into CAN messages based on the [agl-vcar.dbc](https://git.automotivelinux.org/src/agl-dbc/plain/agl-vcar.dbc) file.

_NOTE_: This does **not** require the CARLA server to be running.

To convert CARLA data into CAN messages, follow these steps:

```bash
$ python -u carla_to_CAN.py
# OR
$ python -u carla_to_CAN.py --host <carla_server_ip> --port <carla_server_port>
```

#### Implemented Recording of CAN messages and File Playback

The `record_playback.py` script is responsible for recording amd playing back the CAN data for later sessions.

To use the recording and playback features, execute the script:

```bash
$ python -u record_playback.py
# OR
$ python -u record_playback.py --interface (or) -i can0 # default vcan0
```

CLI Options:

- `1`: Captures CAN messages and writes them into 'can_messages.txt' 
- `2`: Replays captured CAN messages
- `3`: Exit

### # Updated Documentation

In addition to code contributions, I focused on enhancing the AGL documentation related to CARLA and the demo control panel. The following updates were made:

1. AGL Demo Control Panel Documentation:
    - I revised the existing documentation to reflect the transition to Qt6.
    - Clarified usage instructions and provided examples for better understanding.
2. CARLA Integration with AGL Documentation:
    - I created new documentation specifically addressing the integration of CARLA with AGL.
    - Highlighted use cases and provided step-by-step setup instructions.

The code changes related to documentation updates can be found here :- [30097](https://gerrit.automotivelinux.org/gerrit/c/AGL/documentation/+/30097): GSoC '24 Add and Update Documentation for CARLA and Demo Control Panel 

## Next Week Tasks:

- Continue Adding support in AGL Demo Control Panel to Playback CAN Dump files.
- Continue working on New Page to display tyre pressure data.
- Test CARLA Playback on AGL Apps.