---
title: "Week 07 Progress Report"
date: 2024-07-13T20:55:15+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the seventh week of my Google Summer of Code (GSoC) project, I made significant strides in implementing the CARLA-CAN-File playback feature within the Automotive Grade Linux (AGL) ecosystem. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # CARLA Playback

I submitted code changes to Gerrit for review and integration into the AGL demo control panel repository. The specific change can be found here :- [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): Add Python Script to Convert CARLA data into CAN messages

#### Conversion of CARLA data to CAN messages

The `carla_to_CAN.py` script can be run run alongside an existing CARLA simulation to fetch data and convert it into CAN messages based on the [agl-vcar.dbc](https://git.automotivelinux.org/src/agl-dbc/plain/agl-vcar.dbc) file.

_NOTE_: This does *not* require the CARLA server to be running.

This weeks work addressed the incorrect Gear signals. The new values compatible with the agl-vcar are transmission gear ('P', 'R', 'N', 'D'). Where, 

- 0 = Neutral (N)
- 1/2/.. = Forward gears (Applicable when CARLA hero car is in manual transmission)
- -1/-2/.. = Reverse gears (R) 
- 126 = Park (P)
- 127 = Drive (D)

```python
def send_gear(self, gear):
    if gear == "P":
        data = self.gear_message.encode({'Gear': 126})
    elif gear == "R":
        data = self.gear_message.encode({'Gear': -1, 'Gear': -2})
    elif gear == "N":
        data = self.gear_message.encode({'Gear': 0})
    elif gear == "D":
        data = self.gear_message.encode({'Gear': 127})
    message = can.Message(
        arbitration_id=self.gear_message.frame_id, data=data)
    self.can_bus.send(message)
```

Bug fixes this week:
- Fix: Now data is sent to CAN interface only when it is updated
- Fix: Send correct Gear messages, compatible with the agl-vcar signals
- Remove dependency on numpy to calculate vehicle speed, use math lib instead

<video src="./CARLA_CAN.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

To convert CARLA data into CAN messages, follow these steps:

```bash
$ python -u carla_to_CAN.py
# OR
$ python -u carla_to_CAN.py --host <carla_server_ip> --port <carla_server_port>
```

#### Implemented File Playback via Demo Control Panel

The `record_playback.py` module now allows file playback using the Control Panel from the ICPage Demo Toggle.

This feature can be enabled using the `config.ini` file for now, with UI option in settings page soon to be implemented.

The toggle, by default checks for the recorded CAN signals in the can_messages.txt file.

- Fix: Delay is now based on previous timestamp and not the starting timestamp
- record_playback.py can now be imported and also be used in standalone mode

<video src="./FilePlayback.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

### # Updated Resource Compilation Step

Since there’s no `pyrcc6` tool for PyQt6 and `pyside6-rcc` is not available with *PySide6* on the Host and Target while building AGL, We can instead use Qt’s own rcc tool to convert the `assets/res.qrc` file.

The `rcc` tool is available by default in a full Qt6 installation. Infact, the PySide6 tool , `pyside6-rcc` locates and runs the Qt6 rcc tool via subprocess, resulting in the same output.

To convert the `.qrc` file, we run the following command:

```bash
$ /usr/lib64/qt6/libexec/rcc -g python assets/res.qrc | sed '0,/PySide6/s//PyQt6/' > res_rc.py
```

## Next Week Tasks:

- Continue Adding support in AGL Demo Control Panel to Playback CAN Dump files.
- Continue working on New Page to display tyre pressure data.
- Test CARLA Playback on AGL Apps.