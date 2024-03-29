---
title: Week 17 Progress Report
date: 2023-09-25T16:57:31+05:30
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---

### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code to Gerrit.
	- Fixing scaling of Dashboard icons.
	- Completed initial draft of documentation.
- What I plan to do next 

---

## # Pushing to Gerrit

This week saw major documentation work for the `AGL Demo Control Panel` and partial commits were made for review on Gerrit. 

Changes pushed so far:

- 29237: Update UI files | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29237
- 29243: agl-demo-control-panel: Upload Draft 1 documentation | https://gerrit.automotivelinux.org/gerrit/c/AGL/documentation/+/29243
## # Testing Qt IVI

This week when testing the Qt IVI image on the Pi I noticed that the can0 interface does not show when using the shell, however, I am still able to configure the can0 interface and cannelloni and kuksa by ssh-ing into the image. 
## # Fixing the scaling issue

This week I also made a minor fix for the scaling issue of the Dashboard icons for the control panel by using QPainter to scale up the images. 

---
## # What Next?

- Continue testing
- Add databroker support
- Continue pushing code to Gerrit and issue patched as required.