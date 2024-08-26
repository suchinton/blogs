---
title: "Week 12  Progress Report"
date: 2024-08-17T21:20:14+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the twelfth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the Keypad features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # (WIP) New QML UI

This week I continued working on the QML-based Engine RPM  and Vehicle Speed Gauges and updated the colour scheme and UI elements for the `AGL-Demo-Control-Panel`.

These new widgets are written in QML, being more flexible and versatile will lead to the UI elements being more expressive.

Here is the reference design shared by the **ICS** team amd **Justin Noel** which I'm using as refernce for the new UI.

![image](./ICS_Design.png)

The `AGL Demo Control Panel` will still use PyQt6 for the main application while the new QML elements will be handled by separate modules to be rendered as child QObjects.


### # Implemented keypad features, to execute commands via ssh, added target script bindings.

This week I concluded my work on implementing the Keypad Page, where the keypad buttons makes calls using "ssh root@<IP> -c 'wrapper-script-xyz.sh' " on remote host by splicing out the actual commands from momikey into separate
scripts that momikey (new) and the ssh calls invoke.


## Next Week Tasks:

- Continue working on the new UI elements.
- Build and test images for the new UI and features.
- Push changes to Gerrit for review by mentors.