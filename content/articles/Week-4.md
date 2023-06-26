---
title: "Week 4 Progress Report"
date: 2023-06-23
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week4/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Added HVAC signals, limited to 4
	- Toggle for Secure and Insecure Mode
	- Added animations for navigation and improved window handles
	- New theme for UI
	- Reading from the config file
- What I plan to do next 

---

## # HVAC signals

This week I started adding signal inputs for the HVAC page, currently only 4 VIS signals are supported as the demo apps have not been patched to support other signals and mostly unresponsive on the Qt5 & Flutter images of AGL. 

The signals that were added this week are:
1. **Left Temp**: "Vehicle.Cabin.HVAC.Station.Row1.Left.Temperature"
2. **Left Fan Speed**: "Vehicle.Cabin.HVAC.Station.Row1.Left.FanSpeed"
3. **Right Temp**: "Vehicle.Cabin.HVAC.Station.Row1.Right.Temperature"
4. **Right Fan Speed**: "Vehicle.Cabin.HVAC.Station.Row1.Right.FanSpeed"

More signals can be added after further discussion with my mentors.

## # Animations & Window handles

<video src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week4/Demo.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

## # Config

Configurations will now be handled by the `config.py` module and help in retaining past preferences and configurations by reading the config file.

### Secure & Insecure Mode

I have also added bindings for enabling and disabling the secure and insecure modes (SSL) of connection with `Kuksa-val-server` and tested both for verification as well.  

## # New Theme

This week I also added a new theme to add more contrast to each element and make the text and icons more legible.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week4/IMG3.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week4/IMG1.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week4/IMG2.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

---
## # What Next?

- Start working on Dashboard and Steering wheel Signals
- Revisit the optional Navigation widget
- Improve GitHub README and documentation

---
[Week 5Report →]() To be Updated!

[← Week 3 Report](/articles/week-3)
