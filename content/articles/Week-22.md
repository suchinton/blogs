---
title: Week 22 Progress Report
date: 2023-10-28
images:
  - https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week22/GSOC Report IMG.png
---
### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code and bug fixes to Gerrit.
	- Added flag in config file to start control panel in full-screen mode.
	- Added persistent user configurations.
	- Updated documentation and Readme file.  
	- Created the first draft of the final GSoC report.
- What I plan to do next 

---
## # Pushing to Gerrit

This week, major work was completed for the AGL Demo Control Panel and commits were made for review on Gerrit. The following changes and bug fixes were pushed this week:

- 29291: agl-demo-control-panel: Fix Svg icons scaling on Dashboard | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29291 (merged)
- 29300: agl-demo-control-panel: Fix circular import problem | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29300 (merged)
- 29302: agl-demo-control-panel: Save user preferences for next session | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29302 (merged)
- 29303: agl-demo-control-panel: Fix typo in docker installation script | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29303 (merged)
- 29311: agl-demo-control-panel: Add Fullscreen / maximized option | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29311 (merged)
- 29339: agl-demo-control-panel: Improve grpc connection | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29339
## Full-screen Mode

This week I added a flag in the default `config.ini` file to:
- Maximize the window at start up
- Hide the window controls since In the dedicated AGL and Docker image, the control panel will be the only application running, and it should not be possible to minimize or close it.
- Disable the curved window corners.  This avoids needing to worry about the background color showing up in the corners and potentially needing to get a custom Weston configuration file in place to avoid the default background. 

## User Preference

This week I also added the ability to save user preferences for the next session, thereby making the preferred configuration persist across sessions. The user preference is now saved in the format given below every time the user attempts a connection with the provided fields.

```python
   1 [default]
   2 preferred-config=user-session
   3 fullscreen-mode=true
   4 
   5 [user-session]
   6 ip = 
   7 port = 
   8 protocol = 
   9 insecure = 
  10 tls_server_name = 
  11 token = default
  12 cacert = default
  13 
  14 # [cutom-config-template]
  15 # ip=<ip address>
  16 # port=<port number>
  17 # protocol=<ws|grpc>     # ws/grpc -> kuksa-val-server, 
						      # grpc -> databroker
  18 # insecure=<true|false>  # Use insecure mode only if server
							  # is also running in insecure mode
  19 # cacert=<default|/path/to/CA.pem>
  20 # token=<default|/path/to/token>      
  21 # tls_server_name=<name>
```

---

## # What Next?

- Issue bug fixes as required.
- Final submission of the GSoC project for evaluation.
- Finalize the final report for GSoC.

---

[GSoC Final Report →]() To be Updated!

[← Week 21 Report](/articles/week-21)