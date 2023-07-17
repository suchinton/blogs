---
title: "Week 6 Progress Report"
date: 2023-07-06
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week6/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	-  Specified Steering wheel Controls 
	- Improved Settings page and error handling
	- Default settings 
	- Improved Navigation Page
	- Updated README and Troubleshooting steps
- What I plan to do next 

---

## # Steering Wheel Controls

This week I added the various signals for the Steering Wheel Controls, namely 

- Volume Up: "Vehicle.Cabin.SteeringWheel.Switches.VolumeUp"
- Volume Down: "Vehicle.Cabin.SteeringWheel.Switches.VolumeDown"
- Volume Mute: "Vehicle.Cabin.SteeringWheel.Switches.VolumeMute"
- Next Track: "Vehicle.Cabin.SteeringWheel.Switches.Next"
- Previous Track: "Vehicle.Cabin.SteeringWheel.Switches.Previous"
- Info: "Vehicle.Cabin.SteeringWheel.Switches.Info"
- Cruise Enable: "Vehicle.Cabin.SteeringWheel.Switches.CruiseEnable"
- Voice: "Vehicle.Cabin.SteeringWheel.Switches.Voice"
- Phone Call: "Vehicle.Cabin.SteeringWheel.Switches.PhoneCall"
- Phone Hangup: "Vehicle.Cabin.SteeringWheel.Switches.PhoneHangup"
- Horn: "Vehicle.Cabin.SteeringWheel.Switches.Horn"

The layout of these controls will be further improved by making use of custom icons and buttons to mimic the layout of a steering wheel more accurately.


<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week6/Screenshot%20from%202023-07-11%2003-26-26.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## # Settings Page and Error handling

I also revised the styling of the QLineEdit and QPushbuttons for the the settings page to make it look cleaner and make the text more legible. This also includes placeholder text for the input fields.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week6/Screenshot%20from%202023-07-11%2003-26-12.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## # Navigation Page

The navigation page received some major changes this week including, 
- Drop down suggestion for address being entered
- Addition of current location as `From Address`
- UI 

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week6/Screenshot%20from%202023-07-11%2003-26-02.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## # README & Troubleshooting

The [README](https://github.com/suchinton/AGL_Demo_Control_Panel#agl_demo_control_panel) file was updated to provide steps to install and troubleshoot the `AGL Demo Control Panel`.

One minor problem I encountered while using QtWebEngine during testing on systems other than Ubuntu 22.04 LTS, namely Fedora 38 and openSUSE was that the navigation map would not render at all or stay black even after the address was selected. 

This behavior can be rectified by following the steps by following the steps given below:

1. Uninstall the requirements

```bash
pip uninstall -r requirements.txt
```

2. Uninstall PyQtWebEngine-Qt5

```bash
pip uninstall PyQtWebEngine-Qt5
```

3. Remove sandboxing for QtWebEngine

```bash
export QTWEBENGINE_CHROMIUM_FLAGS="--no-sandbox"
```


---

## # What I plan to do next 

- Tweak the visuals to better reflect the states of  focus/ check and click events
- Add custom icons/ images to replicate the steering wheel layout
- Test the AGL Demo Control Panel alongside AGL images on target machine, Raspberry pi 4 B

---
## # References:

-  https://www.reddit.com/r/Fedora/comments/qtyf3x/fedora_35_pyqtwebengine_blank_page/
- https://git.automotivelinux.org/AGL/meta-agl-demo/tree/recipes-connectivity/vss/vss-agl/agl_vss_overlay.vspec

---

[Week 7 Report →](/articles/week-7)

[← Week 5 Report](/articles/week-5)