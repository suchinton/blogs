---
title: "Week 01 Progress Report"
date: 2024-06-02T23:18:30+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "GSoC '24 WPR"]
---


## # Topics To Be Covered In This Post
- What I did this week
	- Migrated from PyQt5 to PyQt6/PySide6.
	- Patched Animated Toggle Module to work with PyQt6.
	- Updated the README to include new steps for compiling resources.
- Conclusion
- What I plan to do next 
---

##  # Migrated from PyQt5 to PyQt6/PySide6.

This week, our focus was on porting the UI framework of the AGL Demo Control Panel from Qt5 to Qt6. The migration process was relatively straightforward since the syntax changes between PyQt5 and PyQt6 are minimal.

## # Patched Animated Toggle Module to work with PyQt6.

In the previous version of the Control Panel, we relied on the ‘qtwidgets’ module to render animated toggle switches. However, this package did not support PyQt6 out of the box. To address this, I extracted the relevant module and patched it to work with the Qt6 framework. Instead of relying on qtpy (a Python library that abstracts various Qt APIs), we now use native PyQt6 components.

## # Updated the README.

I also updated the README file to include new instructions for compiling the PyQT6 resources. While this change was minor, I still need to update other instructions to fully reflect recent modifications.

## # Conclusion

All the code changes have been pushed to Gerrit. Additionally, We’ve created a new JIRA issue to track this year’s contributions under GSoC 2024.
- Gerrit: 29969: [Port AGL Demo Control Panel to Qt6]( https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29969)
- JIRA: SPEC-5161 - [GSoC: Extend AGL Demo Control Panel](https://jira.automotivelinux.org/browse/SPEC-5161)

## # What Next?

- Implement improved UX/UI.
- Utilize CARLA APIs for recording and playback sessions.
- Discuss the CARLA server setup intent with mentors.



