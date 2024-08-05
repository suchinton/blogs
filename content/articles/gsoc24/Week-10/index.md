---
title: "Week 10 Progress Report"
date: 2024-08-03T17:07:16+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---

## Tasks Completed:

During the ninth week of my Google Summer of Code (GSoC) project, I made significant strides in implementing the CARLA-CAN-File playback feature within the Automotive Grade Linux (AGL) ecosystem. Below, I provide detailed information about the tasks completed, documentation updates, and plans for the upcoming week.


### # Successfully built the Demo Control Panel image and tested it on RPi4 and Qemu.

This week involved building and texting the AGL Demo Control Panel on actual hardware and testing it's compatibility with other AGL apps.

To replicate the build follow the steps to set up your build environment from the [AGL Docs](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/01_Build_Process_Overview/).

Then follow the steps below to build the new Qt6 `AGL-Demo-Control-Panel` image.

```bash
$ source meta-agl/scripts/aglsetup.sh -m qemux86-64 -b demo-control-panel-qemu agl-demo-control-panel
$ ln -sf $AGL_TOP/site.conf conf/
$ source agl-init-build-env
$ bitbake agl-demo-control-panel
```

### # Working with ICS to improve/ refactor the UX/UI elements in QML.

This week myself and Justin (from ICS) had a discussion in regards to improving the UX/UI for the `AGL-Demo-Control-Panel` by refactoring part(s) of it to be more legible, aesthetic and intuitive for the end user.

QML, being more flexible and versatile will lead to the UI elements being more expressive.

The `AGL Demo Control Panel` will still use PyQt6 for the main application while the new QML elements will be handled by separate modules to be rendered as child QObjects.

### # Initial work on keypad features, to execute commands via ssh.

This week I also started work on implementing the Keypad Page, where the keypad page calls "ssh root@<IP> -c 'wrapper-script-xyz.sh' " on remote host by splicing out the actual commands from momikey into separate
scripts that momikey (new) and the ssh calls invoke.

## Next Week Tasks:

- Continue working on the new UI elements and keypad page.