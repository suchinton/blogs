---
title: "Week 5 Progress Report"
date: 2023-07-01
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week5/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Refactored the IC Acceleration function 
	- IC page script (Toggle)
	- Initialized Steering wheel Signals Controls (Not yet functional)
	- Navigation Page (Revised Style sheet and improved operations
- What I plan to do next 

---


## # IC Acceleration Button

This week I updated the Acceleration button to operate on a `Pressed` and `Released` event instead of the previously used `Clicked` event, this allows us to implement both `Acceleration` and `Deceleration` proportionally  for the vehicle's **Speed** and the **Engine RPM**. 

Triggers for this operation can be done by the following inputs:
- Touch & Hold (On a touch-friendly device)
- Click and hold
- Space bar (Can be mapped to a different key as well)

<video src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week5/Demo_Acc.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

## # Script Mode

I also started work on the Script mode which feeds relevant values to the demo applications when a user is not interacting with the [AGL_Demo_Control_Panel](https://github.com/suchinton/AGL_Demo_Control_Panel). 

Next week, I will extensively work on this to present exciting sequences not just for the demo apps but also for the control panel itself.

## # Steering Wheel Controls

This week also saw the inclusion of dummy inputs for the Steering Wheel Controls, i.e.: 
- Volume Up
- Volume Down
- Next Track
- Prev Track
- Stop/ Play
- Accept Call/ Decline Call

These Buttons are yet to be assigned any actions and are highly subject to change after a discussion with my mentors but it serves to showcase the various operations the control panel can provide.

## # Navigation Page

The Navigation Page was revisited to improve its usability and operations, It can now provide recommendations for `Source` and  `Destination` addresses for navigation, live as they are being typed.

<video src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week5/Demo_Nav.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

---
## # What Next?

- Make Kuksa Subscription Scripts to view the changes being made. Since not all demo applications subscribe to their corresponding VSS signals.
- Introduce some better error handling for when the server is not connected or found on the specified IP address.
- Improve GitHub README and documentation


---
## # References:

- https://github.com/eclipse/kuksa.val/tree/master/kuksa-val-server
- https://github.com/hritik-chouhan/HVAC_dashboard
- [Stackoverflow: qss-border-radius-problem](https://stackoverflow.com/questions/59186106/qss-border-radius-problem-on-widget-when-zooming-in)
- https://github.com/aakash-s45/ic

---

[Week 6 Report →](/articles/week-6)

[← Week 4 Report](/articles/week-4)