---
title: "Week 9 Progress Report"
date: 2023-07-30
images:
- /images/WPR/Week9/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Refined the navigation bar
	- Improved Acceleration function
	- Dummy triggers for Steering Wheel controls 
- What I plan to do next 

---


##  # Navigation Bar

This week I switched the navigation bar from a vertical layout to a horizontal layout to make navigation more intuitive and accessible. I am currently working on revising the animation of the button sizes.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="/images/WPR/Week9/IMG.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## # Acceleration Function

I also revisited the acceleration function to include a `shift up` and `shift down` operation to change the gear ratios as the car accelerates, this helps sell the acceleration as more authentic. This will also help in facilitating the script mode for the IC application. 

## # Steering Wheel Trigger

This week, using the [python-can](https://python-can.readthedocs.io/en/stable/) library, I also added dummy triggers for the Steering Wheel controls. For now, they emit random CAN signals, and can also switch to kuksa-client. I still need to map the toggle to a button on the GUI. 

For testing this on actual hardware, my mentors have arranged for a USB to CAN adapter, which I hope to receive between the 9th and 12th of August (Week 11).

---
## # What Next?

- Resolve the asset licenses.
- Fix window resizing issues.
- Add toggle for switching between kuksa to CAN signals for Steering wheel controls.

---

[Week 10 Report →](/articles/week-10)

[← Week 8 Report](/articles/week-8)