---
title: "Week 10 Progress Report"
date: 2023-08-06
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---

### # Topics To Be Covered In This Post
- What I did this week
	- Resolved the assets licenses and removed unused assets (icons).
	- Fixed the window resizing issue.
	- Increased the navigation button sizes.
	- Added toggle button to switch between kuksa to CAN signals for Steering wheel controls
- What I plan to do next 

---

## # Assets

This week I resolved licenses for the various assets used in the project,

- fan.svg: [fan.svg](https://www.iconfinder.com/icons/3328905/cooling_fan_hotel_service_icon) by [Alpár-Etele Méder](https://www.iconfinder.com/pocike)
- IC.svg: [IC.svg](https://freeicons.io/seo-icons/speed-optimization-icon-32691) by [Fasil](https://freeicons.io/profile/722) under Creative [Creative Commons(Attribution 3.0 unported)](https://creativecommons.org/licenses/by/3.0/)
- Temps: [TempUP.svg](https://freeicons.io/weather-4/weather-tforecast-hermometer-temperature-summer-hot-icon-44794) & [TempDown.svg](https://freeicons.io/weather-4/weather-forecast-thermometer-temperature-winter-cold-icon-44752) by [www.wishforge.games](https://freeicons.io/profile/2257) 
- Cruise.svg: Vectors and icons by <a href="https://github.com/Esri/calcite-ui-icons?ref=svgrepo.com" target="_blank">Esri</a> in MIT License via <a href="https://www.svgrepo.com/" target="_blank">SVG Repo</a>
- Voice.svg: Vectors and icons by <a href="https://github.com/carbon-design-system/carbon?ref=svgrepo.com" target="_blank">Carbon Design</a> in Apache License via <a href="https://www.svgrepo.com/" target="_blank">SVG Repo</a>
- feather icons:  

## # Window Resizing

I resolved the window resizing issues, by using custom handles for the `QMainWindow` in the `UI_Handler` module.  I implemented the resize logic by subclassing `QWidget` and overriding the mouse events and the `resizeEvent` method. I also needed to create some custom widgets that act as grips for resizing the window from different edges or corners.

## # Navigation buttons

I added a new `Menu Page` to the Control Panel for easier navigation to the various controls and also increased the sizes of the `Navigation` buttons at the bottom to increase the responsive hit area when used on a touch display. I also switched the `Navigation` buttons to a `Pressed` event from the previously used `Click` event.

## # Toggle Kuksa to CAN

I also added a toggle to switch between Kuksa signals and CAN bus signals, It will soon be mapped to its relevant function.

---
## # What Next?

- Build an image with agl-demo-preload & agl-demo-cluster-support.
- Implement the module responsible for sending CAN frames for steering wheel controls.
- Test on the target machine using CAN bus adaptors.