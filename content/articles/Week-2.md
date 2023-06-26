---
title: "Week 2 Progress Report"
date: 2023-06-09
images: 
- https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week2/GSOC Report IMG.png
---

### # Topics To Be Covered In This Post
- What I did this week
	- Implement Settings Window
	- Initial support for Kuksa Signals
	- Initializing the GitHub Repository
- What I plan to do next 

---

## # Settings Window
This week I implemented the settings window to start/ configure an instance of kuksa-client to be used across the whole application. Features of this module are as follows:
- Configure Kuksa-client
	- Custom IP address
	- JWT token path
	- Toggle for Insecure mode 
- Start/ Reconnect Kuksa-client
- Display Connection Status

### How Does it Work?

We first created a `KuksaSingleton` module using the [Singleton Design Pattern](https://www.geeksforgeeks.org/singleton-method-python-design-patterns/). The benefits of using the Singleton design pattern are:

- **Initialization**: An object created by the Singleton method is initialized only when it is requested for the first time.
- **Access to the object**: We get global access to the instance of the object.
- **Count of instances**: In singleton, method classes can’t have more than one instance

This allows us to maintain a single instance of the kuksa-client object, and have access to it globally.

```python

import kuksa_client as kuksa

class KuksaClientSingleton:
    __instance = None

    @staticmethod
    def get_instance():
        if KuksaClientSingleton.__instance is None:
            KuksaClientSingleton()
        return KuksaClientSingleton.__instance

    def __init__(self):
        if KuksaClientSingleton.__instance is not None:
            raise Exception("This class is a singleton!")
        else:
            self.default_Config = {
                "ip": '10.10.10.203',
                "port": "8090",
                'protocol': 'ws',
                'insecure': True,
            }

            self.token = "/home/suchinton/.local/lib/\
            python3.10/site-\
            packages/kuksa_certificates/jwt/\
            super-admin.json.token"

            try:
                self.client = kuksa.KuksaClientThread\
                (self.default_Config)
                self.client.authorize(self.token)
                self.client.start()
            except Exception as e:
                print(e)

            KuksaClientSingleton.__instance = self

    def get_client(self):
        return self.client, self.default_Config, self.token

    def reconnect_client(self, new_Config, new_Token):
        self.client.stop()
        print(self.client.checkConnection())
        self.client = kuksa.KuksaClientThread(new_Config)
        self.client.authorize(new_Token)
        self.client.start()

    def __del__(self):
        self.client.stop()
```

This implementation allows us to write custom functions such as `reconnect_client` which helps restart the client with new configurations.

_Note_: This module will be modified to read and write to a config file which will store the default values and the previous entries. 

### Accessing the Kuksa-Client Object

As visible in the above code segment, the `client` object, its `default_Config` and the default `token` path can be retrieved using the `get_client` method in other modules of this application.

All we need to do to get access to the client instance is add the necessary module as such:

```python
.
.
.
import sys

current_dir = os.path.dirname(os.path.abspath(__file__))

# ========================================

sys.path.append(os.path.dirname(current_dir))

import extras.Kuksa_Instance as kuksa_instance

.
.
.
```

and to create the `client` object, the following syntax is used:

```python
self.kuksa = kuksa_instance.KuksaClientSingleton.get_instance()
self.client, self.config, self.token = self.kuksa.get_client()
```

### Settings Window

The impedimented settings menu was made keeping certain requirements in mind, such as:
1. The client's first instance must be started only via the settings
2. The user should be able to restart the client
3. The settings menu must allow the user to modify the default config
4. It must reflect the status of the connection with Kuksa

_Note: The StyleSheet is yet to be updated for the Settings Window_

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week2/settings_demo.gif" height="auto" width="100%" style="border-radius: 10px;">
</div>

## # Kuksa Signals for IC Application

I also added initial support for the [IC application](https://github.com/aakash-s45/ic) by specifying its [VSS](https://www.w3.org/auto/wg/wiki/Vehicle_Signal_Specification_(VSS)/Vehicle_Data_Spec) paths subscribed by it, this allows us to control the visible widget values and states using dials, buttons and input fields. 

The implementation in the Demo application had some flaws that I was able to overcome this week by  optimizing the slider for a smoother experience. This was done using the following Qt modules:
1. **QTimer** object to update the values at a fixed interval instead of updating it every time the slider value changes. This reduces the number of requests sent to the server and makes the application more responsive.
2. **QThread** object to run the timer in a separate thread so that it doesn’t block the main thread and cause the application to freeze.

As a consequence, the app is more responsive, and also the values fed to kuksa-val-server using this method are continuously updated at defined intervals. Which can help us implement  dedicated scripts for the AGL demo apps in the future.

The signal paths specified for the application comply with the recently released [VSS 3.1.1. being used by KUKSA.val 0.3.1](https://github.com/eclipse/kuksa.val/releases). 

<div style="display: flex; flex-direction: column; align-items: center;">
  <img src="https://raw.githubusercontent.com/suchinton/blogs/main/images/WPR/Week2/IC_controls.gif" height="auto" width="100%" style="border-radius: 10px;">
</div>

_Note_: During this week's GSoC meeting, my mentors mentioned that since AGL does not, for the time being, use any navigation APIs, We have decided to make the Navigation Widget (mentioned in last week's report)  an optional module.

## # GitHub Repository

This  week I also initialized the GitHub repository for this project, hosting it under the [Apache V2](https://www.apache.org/licenses/LICENSE-2.0) License after a discussion regarding the same with my mentors.

The repository can be accessed using the provided link.

→   https://github.com/suchinton/AGL_Demo_Control_Panel

## # What Next?

- Next week I expect to have complete support for IC
- Add initial support for HVAC
- Finalize the UI/UX with mentors
- Build AGL master branch on remote server

---
## # References:

- https://www.w3.org/auto/wg/wiki/Vehicle_Signal_Specification_(VSS)/Vehicle_Data_Spec
- https://www.geeksforgeeks.org/singleton-method-python-design-patterns/
- https://github.com/eclipse/kuksa.val/blob/d9dc9f808268be91abd13f8751337cece470c96c/kuksa-client/docs/examples/threaded.md
- https://github.com/eclipse/kuksa.val/releases
- https://dias-kuksa-doc.readthedocs.io/en/latest/contents/invehicle.html
- https://www.apache.org/licenses/LICENSE-2.0

---
[Week 3 Report →](/articles/week-3)

[← Week 1 Report](/articles/week-1)