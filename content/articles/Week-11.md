---
title: "Week 11 Progress Report"
date: 2023-08-13
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week11/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Tested the USB CAN adapters with [hlcand](https://github.com/alexmohr/usb-can) on Raspberry Pi OS and PC.
	- Minor UI changes changed the default font to ttf-Dejavu.
- What I plan to do next 

---

## # Testing USB CAN Adapters

#### Resources used,
1. [hlcand](https://github.com/alexmohr/usb-can)
2. [USB to CAN Analyzer Adapter](https://www.seeedstudio.com/USB-CAN-Analyzer-p-2888.html)
3. can-utils
4. Raspberry Pi 4 (Raspberry Pi OS)
5. PC (Ubuntu 22.04)

This week I tried using hlcand to set up the can0 interface between the Pi and my laptop but only got as far as setting up the interface. There seemed to be no activity on the bus thereafter. 

Desired set up: **Laptop (Transceiver) <-> can_A <=> can_A <-> Pi (RPiOS) (Receiver)**

### Steps To Recreate:   

**1. Start hlcand: (on both the laptop and the Pi)**

```bash
$ sudo ./hlcand -F -s 500000 /dev/ttyUSB0

[6] starting on TTY device /dev/ttyUSB0

[5] attached TTY /dev/ttyUSB0 to netdevice
```

**2. Then to verify can0:** 

```bash
$ ip link show can0

4: can0: <NOARP> mtu 16 qdisc noop state DOWN mode DEFAULT group default qlen 10  
    link/can  
```

**3. Set up can0:**

``` bash
$ sudo ip link set can0 up type can bitrate 500000

$ ip link show can0  
4: can0: <NOARP,UP,LOWER_UP> mtu 16 qdisc pfifo_fast state UP mode DEFAULT group default qlen 10  
    link/can   
```

**4. Send messages over can0:**

```bash
$ cangen can0 -v -L 8

  can0  4D3#8B.C6.E1.24.A7.B4.16.59  
  can0  6C9#42.09.DD.51.F3.14.1B.60

  ...
```

At this point, if I use "candump can0" or "can_viewer.py -c can0 -i socketcan" on the same machine, I can see traffic but nothing if I do it on the other end... i.e. the indicators are also non-responsive on the adapters.

I tried changing the CAN speed and the baud rate but that didn't help. 

At this point, I started rolling back several commits to find a usable state for the application which I found to be at commit #[770776d](https://github.com/alexmohr/usb-can/commit/770776d7c4b53de43992b5ff097cab231f6fa21e) [repo](https://github.com/alexmohr/usb-can/tree/770776d7c4b53de43992b5ff097cab231f6fa21e).

### # Rolling Back

Following the same setup as before, i.e. **Laptop (Transceiver) <-> can_A <=> can_A <-> Pi (RPiOS) (Receiver)**,  I found it to be partially functional at this point. 
### Steps to Recreate

**1. Start `canusb`: (on both the laptop and the Pi)**

```bash
$ sudo ./canusb -p -s 500000 -d /dev/ttyUSB0 -t -F
[7] Running in foreground process
>>> aa 55 12 03 01 00 00 00 00 00 00 00 00 00 01 00 00 00 00 17 
[7] Serial adapter to vcan handler running...
<<< 
```

**2. Then to verify slcan0:** 

```bash
$ ip link show slcan0
4: slcan0: <NOARP,UP,LOWER_UP> mtu 72 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/can 
```

**3. Send messages over slcan0:**

```
$ cangen slcan0 -v
  slcan0  3A9#9E.29.37.55.18.35
  slcan0  7AB#08.60
  slcan0  305#CC.4C.1C.36.6C.1A.FD.68
  slcan0  094#62.7C.7D.2B.4B.8F
  slcan0  331#2E.FD.07.28.E4.7C.B2.2F
  ...
```

Console o/p (PC)
```bash
$ sudo ./canusb -p -s 500000 -d /dev/ttyUSB0 -t -F
[7] Running in foreground process
>>> aa 55 12 03 01 00 00 00 00 00 00 00 00 00 01 00 00 00 00 17 
[7] Serial adapter to vcan handler running...
<<< dlc = 6, data = 9e2937551835
>>> aa c6 a9 03 9e 29 37 55 18 35 55 
dlc = 2, data = 0860
>>> aa c2 ab 07 08 60 55 
dlc = 8, data = cc4c1c366c1afd68
>>> aa c8 05 03 cc 4c 1c 36 6c 1a fd 68 55 
dlc = 6, data = 627c7d2b4b8f
>>> aa c6 94 00 62 7c 7d 2b 4b 8f 55 
dlc = 8, data = 2efd0728e47cb22f
>>> aa c8 31 03 2e fd 07 28 e4 7c b2 2f 55 
```

Console o/p (RPi)
```bash
$ sudo ./canusb -p -s 500000 -d /dev/ttyUSB0 -t -F
[7] Running in foreground process
>>> aa 55 12 03 01 00 00 00 00 00 00 00 00 00 01 00 00 00 00 17 
[7] Serial adapter to vcan handler running...
<<< aa c6 a9 03 9e 29 37 55 18 35 55 
3740.212130 Frame ID: 3a9, Data: 9e2937551835
[7] Not sending message which has been received.
<<< aa c2 ab 07 08 60 55 
3740.407688 Frame ID: 7ab, Data: 0860
[7] Not sending message which has been received.
<<< aa c8 05 03 cc 4c 1c 36 6c 1a fd 68 55 
3740.614939 Frame ID: 305, Data: cc4c1c366c1afd68
cansend - send CAN-frames via CAN_RAW sockets.

Usage: cansend <device> <can_frame>.

<can_frame>:
 <can_id>#{data}          for 'classic' CAN 2.0 data frames
 <can_id>#R{len}          for 'classic' CAN 2.0 data frames
 <can_id>##<flags>{data}  for CAN FD frames

<can_id>:
 3 (SFF) or 8 (EFF) hex chars
{data}:
 0..8 (0..64 CAN FD) ASCII hex-values (optionally separated by '.')
{len}:
 an optional 0..8 value as RTR frames can contain a valid dlc field
<flags>:
 a single ASCII Hex value (0 .. F) which defines canfd_frame.flags

Examples:
  5A1#11.2233.44556677.88 / 123#DEADBEEF / 5AA# / 123##1 / 213##311223344 /
  1F334455#1122334455667788 / 123#R / 00000123#R3

[3] failed to send data via can
<<< aa c6 94 00 62 7c 7d 2b 4b 8f 55 
3740.812400 Frame ID: 94, Data: 
```

While some frames are not successfully sent across the BUS, possibly due to the data length, at least there seems to be some activity on the bus, verified by the indicators for can_h and can_l LED's on the adapters. 

-> _Note_: After connecting the adapter, It should be visible as
```
$ lsusb
Bus 003 Device 004: ID 1a86:7523 QinHeng Electronics CH340 serial converter
...
```

If there are any issues using `/dev/ttyUSB0`, try removing the `brltty` package.
## # UI changes

This week I also made minor adjustments to the font by switching from the `ttf-Open-Sans-font` to the `ttf-Dejavu` font.

I also increased the sizes of buttons and sliders on the `IC Page` to make them more touch-friendly and for the icons and text to be more legible.

---
## # What Next?

- Implement the module responsible for sending CAN frames for steering wheel controls.
- Revisit the Kuksa_Instance module to support Kuksa-client 0.4.0.

---

[Week 12 Report →](/articles/week-12) 

[← Week 10 Report](/articles/week-10)