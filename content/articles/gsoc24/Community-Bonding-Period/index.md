---
title: "GSoC 2024 Community Bonding Period"
date: 2024-05-20T23:54:54+05:30
tags: ["GSoC 2024", "AGL", "The Linux Foundation", "Community Bonding Period"]
---


## # Topics To Be Covered In This Post
- What is the Community bonding period?
- What I did the past month
- Challenges faced
- Community interactions
- What I plan to do next 

---

## What is GSoC & What Have I Been Up To During The CBP?

Google Summer of Code (GSoC) is a global program that offers students an opportunity to contribute to open-source projects. The program is sponsored by Google and has been running since 2005. The program aims to bring together students and open-source organizations to work on real-world projects. The program has three phases: the community bonding period, the coding period, and the final evaluation period.

The community bonding period is the three weeks between GSoC student acceptance and the start of the coding date (1st - 26th May ). During this period I engaged with my GSoC mentors, [Jan-Simon Möller](mailto:jsmoeller@linuxfoundation.org) ,[Walt Miner](mailto:wminer@linuxfoundation.org), [Scott Murray](mailto:smurray@konsulko.com), [Marius Vlad](mailto:mvlad@collabora.com), [Justin Noel](mailto:justin@ics.com) and the rest of the AGL community. I used this period to get familiar with the various tools used in AGL and set up my work environment for the coding period. This also allowed me to better understand the requirements of my project.

I also participated in the  [AGL Weekly Dev Meet](https://wiki.automotivelinux.org/dev-call-info) events to better understand the work done at AGL and its various technical groups. 


## What's the Goal for GSoC '24?

I'm be participating in a Google Summer of Code (GSoC 2024) as a contributor again this year, where I will extend up my previous year's GSoC ‘23 project on the [AGL Demo Control Panel](https://gerrit.automotivelinux.org/gerrit/admin/repos/src/agl-demo-control-panel,general).

My tasks for this year's contribution include:
- Porting the app from Qt5 to Qt6
- Adding file playback capability using CARLA 
- Visualise signals (real-time and playback)
- Integrating new controls and keypad features
- Implementing an IC keypad, and developing a power-control feature

-> For a detailed review of my GSoC '24 Proposal, [click here](https://summerofcode.withgoogle.com/media/user/7727eb0be3f8/proposal/gAAAAABmU3pUuIt9F5ChR-_TYFPOXBtXiIz8kE18jMLc7uuhIDVODIrM2oLHgQ1IVLhIz6rxiNdfY6dXjpwTLNm30GEkT5rmwkaW6vtlJ3jsUsFG6eB20jY=.pdf).

## Setting Up

On our weekly GSoC meetings during the bonding period, we had detailed discussions regarding this years project, where we discussed:

- Qt6 support for AGL: Work in progress, I revied the [Sandboxed](https://git.automotivelinux.org/AGL/AGL-repo/log/?h=sandbox/jsmoeller/qt6 ) progress.
- ICS's contribution to the UI/UX side of the AGL Demo Control Panel
- POA of adding file playback using CARLA Python APIs
- Reviewing all the patches made to the control panel since GSoC '23 compleatoin:
	- [29910: Simplify server configuration](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29910)
 	- [29911: Add ability to disable HVAC and steering wheel pages](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29911)
 	- [29912: Improve vehicle simulator](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29912)

--- 
## CARLA 

During the CBP, my focus was on reviewing the documentation for CARLA and reviewing it's various Python APIs.

### Configuration

Running headless server:
```
./CarlaUE4.sh -quality-level=Low -prefernvidia -RenderOffScreen
```

## # What Next?

- Initial work on porting the AGL Demo Control Panel from Qt5 to Qt6.
- Start work on a demo CARLA script to pull ego vehicle data.