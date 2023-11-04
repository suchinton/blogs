---
title: "Week 7 Progress Report"
date: 2023-07-13
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week7/GSOC Report IMG.png
---


### # Topics To Be Covered In This Post
- What I did this week
	- Finalized Steering Wheel Layout
	- Tested AGL_Control_Panel against AGL image on RPi 4
	- Added Installation script for Ubuntu/ Debian
- What I plan to do next 

---

## # Finalized Steering Wheel Layout

This week I was able to finalize the layout and VSS paths to be used for the Steering Wheel Page, this shall remain subject to further improvements and revisions.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week7/IMG1.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## # AGL_Demo_Control_Panel Against AGL Image on RPi 4

This week I also tested the `AGL_Demo_Control_Panel` against the [IC application](https://github.com/aakash-s45/ic) by running the `agl-cluster-demo-platform-flutter` image on a Raspberry Pi 4 B model.

### Ready-made Images

While I did not use this particular image for testing purposes, I found this method more convenient in case I need a prebuilt image in the future. 

```bash
wget -r -np -nH --cut-dirs=7 https://download.automotivelinux.org/\
AGL/snapshots/master/latest/raspberrypi4/deploy/images/raspberrypi4-64/
```

### Building for RPi 4

For building the `agl-cluster-demo-platform-flutter` image for the Raspberry Pi 4 I followed the following steps after setting up my [build environment](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/03_Downloading_AGL_Software/).

```bash
source meta-agl/scripts/aglsetup.sh -f \
-m raspberrypi4 -b build-flutter-cluster agl-demo agl-devel

source agl-init-build-env

bitbake agl-cluster-demo-platform-flutter
```

_Note:_ Copying the build on my local machine, I perform this step only because I built the image on a remote server 

```bash
scp -r rs5:AGL/master/build-flutter-cluster/tmp/deploy/\
images/raspberrypi4-64/ $HOME/AGL_Images/IC_rasp/
```

### Flashing Image to SD Card

While the documentation provides instructions on how to flash the AGL image onto an SD card, I was unable to get it to boot using the provided steps. It is possible that it was a human error on my part, however, I found it much simpler to use the Raspberry Pi to flash the image for now.

### Connecting to the Pi using Ethernet

Since the IC Flutter demo app does not provide a GUI method to find the IP address of the Pi, and also does not connect to the WiFi by default, I went on to enable communication using a LAN cable.

To enable the Ethernet port to find the Pi on my network, I followed the following steps in the Gnome Settings Panel, similar settings should be available in other network configuration tools.

- Settings -> Network -> Wired ->  IPv4
- IPv4 Method -> Shared to other computers

```bash
# Find IP addr of the ethernet
ip address show eno1
ping <ip-address-for-eno1>

# Resolve all available IP addresses on network, 
# recommend turning off WiFi
arp -a

ssh root@<ip-address-raspberrypi>
```

After successfully `ssh`-ing into the session, we kill the existing `kuksa` service (since it runs on localhost) and restart `kuksa-val-server` on special IP address `0.0.0.0`.

```bash
pkill kuksa
kuksa-val-server --address 0.0.0.0 --insecure 

# optional flag "--log-level VERBOSE"
```

## # Added Installation Script for Ubuntu

To cut a long tale short, we can no longer do `pip install x`. The command does not work with Ubuntu 23.04 due to a deliberate policy change to avoid conflicts between the Python package manager (`pip`) and Ubuntu's underlying APT. `pip` can now only be used in a virtual environment created with `venv` or by using [`pipx`](https://manpages.ubuntu.com/manpages/lunar/en/man1/pipx.1.html), Python local installer.

To overcome this we can run the `install_ubuntu.sh` script to resolve the dependencies on Ubuntu. The script is yet to be tested on other versions of Ubuntu and only serves to make installation easier later on, it is still recommended to manage these dependencies manually on Ubuntu.

```bash
#!/bin/bash

sudo apt-get update
sudo apt-get install -y python3-pyqt5
sudo apt-get install -y python3-pyqt5.qtwebengine

echo 'PATH="$HOME/.local/bin/:$PATH"' >>~/.bashrc
source ~/.bashrc

if ! command -v pipx &> /dev/null; then
	echo "pipx not found. Installing pipx..."
	sudo apt-get install -y pipx
fi

pipx install kuksa-client
pipx install folium --include-deps  

echo "Installation complete!"
```

---

## # What Next?

- Push repo to Gerrit.
- Fix address selection for the navigation page.
- Make Visibility of Controls optional and provide specific configuration for each page.

---
## # References:

-  [AGL Docs: AGL Software](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/03_Downloading_AGL_Software/).
-  [AGL Docs: Building for RPi](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/08_Building_for_Raspberry_Pi_4/)
-  [AGL Docs: IC App](https://docs.automotivelinux.org/en/master/#01_Getting_Started/03_Build_and_Boot_guide_Profile/02_Flutter_Instrument_Cluster_%28qemu-x86%29/)

---

[Week 8 Report →](/articles/week-8)

[← Week 6 Report](/articles/week-6)