---
title: "Week 8 Progress Report"
date: 2023-07-19
images:
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week8/GSOC Report IMG.png
---



### # Topics To Be Covered In This Post
- What I did this week
	- Uploaded the repo to Gerrit as a patch set, [link](https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29060/1).  
	- Deprecated Navigation Page after discussion with mentors.
	- Refactored QThread instance for feeding values to the kuksa server.  
	- Added option in settings to hide pages.
	- Reviewed documentation for Python-can to enable Steering wheel controls.
- What I plan to do next 

---

## # Pushing To Gerrit

I was able to push all of my progress to Gerrit this week despite a few challenges, where every time I would try to clone the `agl-demo-control-panel` repo, I would get the following message:

```bash
Cloning into 'agl-demo-control-panel'... suchinton2001@gerrit.automotivelinux.org: Permission denied (publickey). fatal: Could not read from remote repository.
```

Initially, I thought this was due to the permissions not being set properly for the public and private keys files, but as it turns out OpenSSH 8.8 disabled RSA signatures using the SHA-1 hash algorithm. so I ended up creating a new key set using `ed25519`.

```bash
ssh-keygen -t ed25519   
```

and added the public key to Gerrit and all was well. Alternatively, you can also configure the client to accept RSA by adding this ssh config option in `~/.ssh/config`:

```bash
PubkeyAcceptedKeyTypes +ssh-rsa
```

## # Deprecated Navigation Page

After discussing my progress with my mentors, we decided to deprecate the Navigation Page as it could result in more complicated and longer build times for AGL. 

If necessary, in the future we may consider different ways of implementing the Navigation Inputs without the use of QtWebEngine, but for now, this page will be disabled by default.  

## # Refactored QThread Class `FeedKuksa`

This class is now a separate module that enables feeding values to the `kuksa-client` by executing on a separate thread and keeping the GUI responsive. This will be further enhanced in the next week to provide an override function for different applications.  

```python
import time
from PyQt5.QtCore import QThread
from . import Kuksa_Instance as kuksa_instance

class FeedKuksa(QThread):
    def __init__(self, parent=None):
        QThread.__init__(self,parent)
        self.stop_flag = False
        self.set_instance()

    def run(self):
        print("Starting thread")
        self.set_instance()
        while not self.stop_flag:
            self.send_values()

    def stop(self):
        self.stop_flag = True
        print("Stopping thread")

    def set_instance(self):
        self.kuksa = kuksa_instance.KuksaClientSingleton.get_instance()
        self.client = self.kuksa.get_client()

    def send_values(self, Path=None, Value=None, Attribute=None):
        if self.client is not None:
            if self.client.checkConnection() is True:

                if Attribute is not None:
                    self.client.setValue(Path, Value, Attribute)
                else:
                    self.client.setValue(Path, Value)
            else:
                print("Could not connect to Kuksa")
                self.set_instance()
        else:
            print("Kuksa client is None, try reconnecting")
            time.sleep(2)
            self.set_instance()
```

## # Visibility Settings

This week I also added a visibility setting for the various pages of the `AGL_Demo_Control_Panel` which allows specific pages and their corresponding navigation buttons to be visible as per the user preference.

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week8/IMG.png"height="auto" width="100%" style="border-radius: 10px;">
</div>


## # CAN Bus Signals for Steering Wheel Inputs

This week I also reviewed the documentation and some projects that utilize the `python-can` library to enable communications using the Controller Area Network (CAN) protocol. I still have a lot to understand about the same since I've never worked with CAN before, however, I hope to have a working implementation by next week with the guidance of my mentors and assistance from the community.

---

## # What Next?

- Refining the navigation bar, preferably adopting a horizontal layout.
- Submission of the initial implementation of the Steering wheel inputs using CAN bus and adding a toggle to switch to kuksa-client.

---

[Week 9 Report →](/articles/week-9)

[← Week 7 Report](/articles/week-7)