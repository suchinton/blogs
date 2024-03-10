---
title: Week 14 Progress Report
date: 2023-09-02
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---
### # Topics To Be Covered In This Post
- What I did this week
	- Enabled Secure Mode 
	- Switched to Carbon icons.
	- Animated Navigation of Control Panel[]()
- What I plan to do next 

---

## # Enable Secure Mode

Till now, our testing of the `AGL Demo Control Panel` was being conducted in Insecure mode, i.e. SSL encryption was disabled when connecting to the server (had to run both with the `--insecure` flag).

This week, I made progress in enabling secure communication between the `Kuksa-val-server` and the `Kuksa-client` by using the AGL-signed `CA.pem` certificate and specifying the `--tls-server-name` to `Server`.  

Command line:
```
kuksa-client --ip <kuksa ip-addr> --cacertificate ./assets/CA.pem --tls-server-name Server
```

Testing the new implementation,
```python
import os
import kuksa_client as kuksa
import time

# AGL CA file
ca_file = "./assets/CA.pem"

config = {
    "ip": '10.42.0.95',
    "port": "8090",
    'protocol': 'ws',
    'insecure': False,
    'cacertificate': ca_file,
    'tls_server_name': "Server",
}

token = os.path.join(os.path.expanduser("~"), 
                          f".local/lib/python3.10/site-packages/kuksa_certificates/jwt/super-admin.json.token")

class Test_class():
    def __init__(self):
        
        try:
            self.client = kuksa.KuksaClientThread(config)
            self.client.start()
            time.sleep(1)
            self.client.authorize(token)
        except Exception as e:
            print(e)
            print("Could not connect to Kuksa")

    def do_test(self):
        self.client.setValue("Vehicle.Speed", "100", 'value')

if __name__ == "__main__":
    test = Test_class()
    test.do_test()
    print("Done")
```

The kuksa-client configuration has now been updated to use the same logic, hence both the server and the client can connect in secure mode.

## # Switched to Carbon Icons.

I also updated the icons being used, from feather icons to carbon icons, and removed the various redundant assets from the project.

Updated Look
<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="/images/WPR/Week14/IC_Demo.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="/images/WPR/Week14/HVAC_Demo.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="/images/WPR/Week14/SC_Demo.png"height="auto" width="100%" style="border-radius: 10px;">
</div>
## # Animated Navigation of Control Panel

This week, I made some changes in the `UI_Handler` module such that in the revised version of the `FaderWidget`, the `QPropertyAnimation` object animates the opacity of the `QGraphicsOpacityEffect` applied to the new widget. The animation starts when the `start` method of the `QPropertyAnimation` object is called.

The `QEasingCurve.OutCubic` easing curve is used to make the transition smoother. When the animation is finished, we close the old widget and show the new one.

Additionally, I also added an animation to move in and out of the Dashboard such that the Navigation bar collapses when in the Dashboard as the user can use the navigation tiles instead. 

<video src="/images/WPR/Week14/Demo.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

---
## # What Next?

- Upload to Gerrit and issue patches based on review.
- Improve Error Handling.
- Start documentation and improve README.