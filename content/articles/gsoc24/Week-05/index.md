---
title: "Week 05 Progress Report"
date: 2024-06-28T18:24:30+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

### # Code Merged this week:
- [29992](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29992): Update Dockerfile for PyQt6 Compatibility.
- [29969](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29969): Port AGL Demo Control Panel to Qt6.

### # Kuksa-client

This **`kuksa-client`** was bumped up from **0.4.2** to **0.4.3**.


### # carla_to_CAN
This week I pushed [30049](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/30049): (WIP) Add Python Script to Convert CARLA data into CAN messages.


The `carla_to_CAN.py` script can be run run alongside an existing CARLA simulation to fetch data and convert it into CAN signals.

## Steps to set up

```bash
python3.8 -m venv carlaenv
source carlaenv/bin/activate
pip3 install -r requirements.txt
```

## Steps to run

```bash
./vcan.sh # To set up the vcan0 interface
python -u carla_to_CAN.py

# OR
python -u carla_to_CAN.py --host <carla_server_ip> --port <carla_server_port>
```

<video src="./Demo.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

These CAN messages can now be written into the `Playback` text file, which will be read by the `AGL-Demo-Control-Panel` for the playback feature.

## Next Week Tasks:

- (WIP) New page for tyre pressure readings.
- Add support in AGL Demo Control Panel to Playback CAN Dump files.
- Test and debug the AGL Demo Control Panel.
- Prepare for mid-term evaluation.
