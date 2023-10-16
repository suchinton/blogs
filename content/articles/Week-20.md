---
title: Week 20 Progress Report
date: 2023-10-16T14:34:21+05:30
images:
  - https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week20/GSOC Report IMG.png
---
### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code to Gerrit.
	- Added drop-down in settings to pick configs.
	- Refactored `config.py` and `settings.py` to handle user preferences better.
- What I plan to do next 

---

## # Pushing to Gerrit

This week, major work was completed for the AGL Demo Control Panel and commits were made for review on Gerrit. The following changes have been pushed so far:

Changes pushed so far:
- 29255: agl-demo-control-panel: Update and add new assets | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29255 (merged)
- 29270: agl-demo-control-panel: Add grpc support for databroker | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29270 (merged)
- 29279: agl-demo-control-panel: Refactor Settings, Config and UI scaling | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29279

## User preference

The settings page saw the inclusion of new parameters for the user to configure including
- Port: No longer hard-coded into the application.
- AGL's CA.pem (Toggle): Optional parameter if the user wants to test on local builds of databroker and kuksa-val-server.
- Drop-down combo box to pick default configs specified in `config.ini`.
## Config

The default configuration is no longer loaded into the settings page by the `config.py` module, instead, it acts as a relay module to parse the user's preferred configuration(s) specified in the `config.ini` file. 

The new implementation allows the user to reconnect to a different instance of the kuksa server(s) seamlessly, as the new config is loaded and fed to the `kuksa_instance.py` module based on the state of the parameters of the settings page. 
## UI Tweaks

The main UI of the `AGL Demo Control Panel` saw some quality and functional changes this week, including:
- Grab handle is now available at the bottom-right of the main window to help resize the window.
- Settings page was revised to follow a 2 column layout to make the preferences more accessible and legible.

<div style="display: flex; flex-direction: column; align-items: center;">
	<img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week20/IMG1.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<video src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week20/VID1.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

---

## # What Next?

- Improve handling for script toggle for IC page.
- Update docugit push mentation.
- Continue pushing code to Gerrit and issue patched as required.

---

[Week 21 Report →]() To be Updated!

[← Week 19 Report](/articles/week-19)