---
title: "Week 12 Progress Report"
date: 2023-08-19
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week12/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Created CAN interface using cannelloni
	- Implemented the FeedCAN module.
	- Added support for sending Steering wheel CAN messages.
	- Created a test script for sending CAN frames using the USB CAN adapter. 
- What I plan to do next 

---
## # How to use cannelloni

To test the `AGL Demo Control Panel` for sending CAN messages to the target machine, we can create a virtual CAN interface using cannelloni. [Cannelloni](https://github.com/mguentner/cannelloni) is a SocketCAN over Ethernet tunnel that uses UDP to transfer CAN frames between two machines. Here are the steps used to create a vcan interface using cannelloni between the host machine (laptop) and target machine (raspberry pi) running the AGL IVI Image:

_Note_: Since I am using the `IVI` image for testing this, there is no need to install `cannelloni` and configure the `can0 interface` on the target machine.

1. On both machines, create the virtual CAN interface with the command:
```bash
 sudo ip link add dev can0 type vcan
```
2. On both machines, bring the virtual CAN interface online with the command: 
```bash
sudo ip link set up can0
```
3. On both machines, install cannelloni from its [GitHub repository](https://github.com/mguentner/cannelloni)
4. On one machine (e.g. the laptop), run cannelloni as a server with the command: 
```bash
cannelloni -I can0 -R <raspberry pi IP address> -L \
<laptop IP address> -S 20000 -D 20000
```
5. On the other machine (e.g. the raspberry pi), run cannelloni as a client with the command: 
```bash
cannelloni -I can0 -R <laptop IP address> -L \
<raspberry pi IP address> -S 20000 -D 20000
```
6. You should now be able to send and receive CAN frames between the two machines using the vcan interface and cannelloni.
### To test connection

On the sender machine (Laptop): 
```
cansend can0 021#FFFFFFFF80000000
```

On receiver machine (RPi4): 
```
can_viewer.py -c can0 -i socketcan
# OR
candump can0
```


## # FeedCAN module

The "FeedCAN" module sends a CAN signal using the `can` module by defining a function `send_can_signal` that takes a CAN frame as input, separates the frame into arbitration ID and data parts, and sends the CAN signal using the `can.interface.Bus` class. 

```python
import can

def send_can_signal(frame):
    msg = separate_can_frame(frame)
    bus = can.interface.Bus(channel='can0', bustype='socketcan')
    try:
        bus.send(msg)
        print("CAN signal sent successfully:")
        print("CAN ID:", hex(msg.arbitration_id))
        print("Data:", msg.data)
    except can.CanError:
        print("Failed to send CAN signal")
    finally:
        bus.shutdown()

def separate_can_frame(frame):
    # split the frame into arbitration ID and data parts
    arb_id, data = frame.split("#")
    arb_id = int(arb_id, 16)
    data = bytes.fromhex(data)
    message = can.Message(arbitration_id=arb_id, data=data)
    return message
    
def main():
    frame = "021#FFFFFFFF10000000"
    send_can_signal(frame)

if __name__ == "__main__":
    main()
```


## # Steering wheel CAN messages

After making the `FeedCAN`, the next step involved defining the relevant CAN frames for the Steering controls and mapping them accordingly. To simplify the convention, I decided to map the VSS path and CAN frame message to the Object name (QPushButton).

```python
self.switches = {

"VolumeUp": ["Vehicle.Cabin.SteeringWheel.Switches.VolumeUp","021#FFFFFFFF40000000"],
"VolumeDown": ["Vehicle.Cabin.SteeringWheel.Switches.VolumeDown","021#FFFFFFFF10000000"],
"VolumeMute": ["Vehicle.Cabin.SteeringWheel.Switches.VolumeMute","021#FFFFFFFF01000000"],
...
}
```

Then depending on whether the user wishes to send the Kuksa signal or CAN messages, we can easily switch between the two.

```python
def controls_clicked(self, button):
	button_clicked = button.objectName()
	# send CAN message
	feed_can.send_can_signal(self.Steering.switches[button_clicked][1])
	# send kuksa signal
	self.feed_kuksa.send_values\
	(self.Steering.switches[button_clicked][0], str(true))
```

## # Test script for USB CAN adapters

I also created a test script for the [USB CAN adapters](https://www.seeedstudio.com/USB-CAN-Analyzer-p-2888.html) which allows us to send messages over the interface, however, the downside is that the adapter does not show up as a CAN interface on the machine. Possible solutions, if we decide to go ahead with the adapters for demo purposes, we could create a small Python script that routes all the incoming messages to the can0 interface.

### Test script

Sender (Host machine):
```python
from can import Message
from can.interfaces.seeedstudio import SeeedBus
import threading
import time

class CanSend(threading.Thread):
    def __init__(self, channel):
        super(CanSend, self).__init__()
        self.bus = SeeedBus(bustype='seeedstudio', channel=channel, bitrate=1000000)
        self.stop_requested = threading.Event()

    def run(self):
        while not self.stop_requested.is_set():
            button_actions = [
                { "button": "previous", "data": 0xFFFFFFFF80000000 },
                { "button": "volup", "data": 0xFFFFFFFF40000000 },
                # We can add more buttons here
            ]
            for button_action in button_actions:
                self.send_button_action(button_action)

    def stop(self):
        self.stop_requested.set()

    def send_button_action(self, button_action):
        arbitration_id = 0x021
        data = button_action["data"]
        message = Message(arbitration_id=arbitration_id, data=[data & 0xff, (data >> 8) & 0xff, (data >> 16) & 0xff, (data >> 24) & 0xff], is_extended_id=False)
        self.bus.send(message)

sender = CanSend('/dev/ttyUSB0')
sender.start()
```

Receiver (RPi4):
```python
import can

def receive_can_messages():
    # Create a new 'SeeedBus' instance with 'usb0' channel
    bus = can.interface.Bus(channel='/dev/ttyUSB0', bustype='seeedstudio')

    # Loop to continuously receive messages
    while True:
        # Receive a message
        message = bus.recv()

        # If a message is received, print its details
        if message is not None:
            print(f'Received message: {message}')

if __name__ == '__main__':
    receive_can_messages()
```

---
## # What Next?

- Initial work on using kuksa-client version 0.4.0
- Subscribe to VSS signals for registering changes made on target.
- If necessary, I could implement a service to route the incoming messages on the can0 interface. (after discussion with mentors)

---

[Week 13 Report →](/articles/week-13)

[← Week 11 Report](/articles/week-11)