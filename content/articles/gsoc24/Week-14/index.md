---
title: "Week 14 Progress Report"
date: 2024-09-01T12:55:00+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the fourteenth week of my Google Summer of Code (GSoC) project, I continued working on refactoring developing the UI elements and the Keypad features for the `AGL-Demo-Control-Panel`. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.

### # Keypad Features

This week I pushed code for review via [30231: Abstract Momikey Input Events using Individual Key Scripts](https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/30231) ; where I proposed to abstract the events triggered by momikey input by splitting them into individual scripts.

This would have allowed external users/apps (**Demo Control Panel**: Keypad) to trigger input events by simply running the scripts over cli/ssh session.

<video src="./Keypad.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

However, maintainer: **Naoto Yamaguchi San** recommended using the momiweb interface APIs to change guest by command.

```bash
wget http://localhost:8080/cgi-bin/flutter.cgi
wget http://localhost:8080/cgi-bin/qt.cgi
wget http://localhost:8080/cgi-bin/momi.cgi
wget http://localhost:8080/cgi-bin/bomb.cgi
```

This has been accomplished using the `Paramiko` library.

The keys execute the following command on the host system, for reference; [momikey.sh](https://git.automotivelinux.org/AGL/meta-agl-devel/tree/meta-agl-ic-container/recipes-demo/momikey/files/momikey.sh)


### # (WIP) New QML UI

This week I continued working on the QML-based UI elements for the `AGL-Demo-Control-Panel`.

#### # New Gauge for Vehicle Speed and Engine RPM

<video src="./VS_Gauge.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

_Bug_: Integrating new QML widget into PyQt6 Application leads to the issue of Scaling and Widget transparency, I will be addressing these in the coming week.

#### # Continued working on Coolant and Fuel Gauges: These widgets are still WIP.


## Next Week Tasks:

- Continue working on the new UI elements.
- Push changes to Gerrit for review by mentors.