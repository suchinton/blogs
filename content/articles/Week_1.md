---
title: "Week 1 Progress Report"
date: 2023-06-03
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week1/GSOC Report IMG.png
---

## # Topics To Be Covered In This Post
- What I did this week
	- Communication with Kuksa.val
	- Skeleton code for GUI using PyQt5
	- Navigation widget for IC 
- What I plan to do next 

---


##  1) Communication With Kuksa.val

Continuing with the progress made last week regarding communication with Kuksa-val-server using a bridged network, I tried to enable IP routing to make the traffic at 10.10.10.204 also accessible at localhost 127.0.0.1, but that did not work.

Therefore, we run the kuksa-val-server on IP address 0.0.0.0, which is a special IP address known as the “default route” or “unspecified address”. It is used to indicate that traffic should be sent to all interfaces on a machine, regardless of their individual IP addresses.

This allows us to sidestep the hassle of setting a dedicated IP address to communicate with Kuksa. Based on these developments, I then modified the Kuksa-client config in my demo application, [AGL-Kuksa.val-Visualiser](https://github.com/suchinton/AGL-Kuksa.val-Visualiser/tree/main/Demo_1) for testing its functionality, The changes made in `main.py` were:

```python
.
.
.

config = {
"ip": '10.10.10.203',
"port": "8090",
'protocol': 'ws',
'insecure': True,
}
.
.
.
```

### Running AGL Image :

We then run the QEMU instance the same as before (this time without a VNC viewer).

```bash
sudo qemu-system-x86_64 -device \
virtio-net-pci,netdev=net0,\
mac=52:54:00:12:35:02 -netdev bridge,br=br0,id=net0 -drive \
file=agl-cluster-demo-platform-flutter-qemux86-64.ext4 \
,if=virtio,format=raw -usb -usbdevice tablet -device \
virtio-rng-pci -snapshot -vga virtio -soundhw hda \
-machine q35 -cpu kvm64 -cpu qemu64,+ssse3,+sse4.1,+sse4.2,+popcnt -enable-kvm -m 2048 \
-serial mon:vc -serial mon:stdio -serial null \
-kernel bzImage -append 'root=/dev/vda rw console=tty0 \
mem=2048M ip=dhcp oprofile.timer=1 console=ttyS0,115200n8 verbose fstab=no'
```

### Running Kuksa-val-server in QEMU: 

```bash
kuksa-val-server --address 0.0.0.0 --insecure --log-level VERBOSE
```

###  Host Machine Command For Kuksa-client:

To verify and establish a connection with Kuksa-val-server running inside the QEMU instance, I made the [kuksa-client]() listen on the VM's public IP address

```bash
# IP is the QEMU instance IP addr
kuksa-client --ip 10.10.10.203 --insecure
```

Next, we authorize the client using a `JWT` access token, usually found at `$HOME/.local.local/lib/python3.10/site-packages/kuksa_certificates/jwt/`. Upon successfully connecting to the server,

here is a look at how the [AGL-Kuksa.val-Visualiser](https://github.com/suchinton/AGL-Kuksa.val-Visualiser) application interacts with the [IC](https://github.com/aakash-s45/ic) demo application.
<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week1/IC_control.gif" height="auto" width="100%" style="border-radius: 10px;">
</div>

## 2) Navigation Input For IC Demo App

This is a work-in-progress widget to be added to the Instrument cluster tab of our GUI application, and seeks to provide the functionality to,
- set current location (from)
- set destination location (to)
- provide suggestions for address
- start navigation

It makes use of [Folium](https://python-visualization.github.io/folium/),  which is a Python wrapper for leaflet.js which is a javascript library for plotting interactive maps. It enables both the binding of data to a map for choropleth visualizations as well as passing rich vector/raster/HTML visualizations as markers on the map. 

The program has several methods such as 
- `search_address`
- `fetch_address_suggestions`
- `show_suggestions`
- `show_location`
- `update_map` and more.

These methods are used for fetching suggestions from [OpenStreetMap](https://www.openstreetmap.org/#map=5/21.843/82.795), displaying suggestions, displaying the location on the map, updating the map, and creating HTML code for the map.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week1/navigation.png" height="auto" width="100%" style="border-radius: 10px;">
</div>

The implementation of this feature can be improved in the coming weeks for setting the marker/ address more accurately.

## 3) Application Preview

Finally, I got to start working on the skeletal code of the main application using Qt5. This is still a fairly bare-bones preview of how the interface would look and is highly subject to change in future revisions. 

All new widgets created for the AGL demo apps will be added as separate modules to increase the customization and maintainability  of the whole application.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week1/design_preview.png" height="auto" width="100%" style="border-radius: 10px;">
</div>

## What Next?

Here are some of my targets for next week,
- Improve implementation of Kuksa-client 
- add support for signals subscribed by the IC Application
- improve the navigation widget
- finalize the design for the main application with my mentors

## References:

- https://github.com/eclipse/kuksa.val/blob/master/kuksa-client/docs/examples/threaded.md
- [https://www.howtogeek.com](https://www.howtogeek.com/225487/what-is-the-difference-between-127.0.0.1-and-0.0.0.0/#:~:text=In%20the%20context%20of%20servers%2C%200.0.0.0%20means%20all,will%20be%20reachable%20at%20both%20of%20those%20IPs.)
- https://python-visualization.github.io/folium/
- https://www.openstreetmap.org/help

<br>

[Week 2 Report →]() To be updated!

[← Community Bonding Period](Community-Bonding-Period.md)