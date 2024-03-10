---
title: Week 18 Progress Report
date: 2023-09-27T14:39:31+05:30
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---

### # Topics To Be Covered In This Post
- What I did this week
	- Pushing code to Gerrit.
	- Simplified settings page.
	- Add compatibility for Databroker.
	- Refactor acceleration and script for IC page.
- What I plan to do next 

---

## # Pushing to Gerrit

This week saw major documentation work for the `AGL Demo Control Panel` and partial commits were made for review on Gerrit. 

Changes pushed so far:

- 29237: Update UI files | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29237 (Merged)
- 29243: agl-demo-control-panel: Upload Draft 1 documentation | https://gerrit.automotivelinux.org/gerrit/c/AGL/documentation/+/29243 (Merged)
- 29255: agl-demo-control-panel: Update and add new assets | https://gerrit.automotivelinux.org/gerrit/c/src/agl-demo-control-panel/+/29255 (In review, merge conflict due to renaming of CA.pem file)
## Settings Page

This week I worked to simplify the settings page and add new icons to better shop connection to the server, auto-refresh the status after the reconnect button is pressed, and better showcase the available options for the Steering wheel signals.

<div style="display: flex; flex-direction: column; align-items: center;">
	<img src="/images/WPR/Week18/IMG1.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

<div style="display: flex; flex-direction: column; align-items: center;">
	<img src="/images/WPR/Week18/IMG2.png"height="auto" width="100%" style="border-radius: 10px;">
</div>
<div style="display: flex; flex-direction: column; align-items: center;">
	<img src="/images/WPR/Week18/IMG3.png"height="auto" width="100%" style="border-radius: 10px;">
</div>

## Databroker

This week I also added initial support for databroker. This is still a work in progress and even though the client is successfully authorized, the connection drops.
### Enable Secure Connection on Target

```bash
databroker --address 0.0.0.0 --tls-cert /etc/kuksa-val/Server.pem --tls-private-key /etc/kuksa-val/Server.key --jwt-public-key /usr/lib/python3.10/site-packages/kuksa_certificates/jwt/jwt.key.pub --vss /usr/share/vss/vss_rel_3.1.1-agl.json
```

### Connecting to Databroker Secure Mode

```bash
kuksa-client --cacertificate /home/suchinton/Repos/AGL_Demo_Control_Panel/assets/CA.pem --tls-server-name Server --protocol grpc --ip 10.42.0.95 --port 55555
```

```bash
kuksa-client --cacertificate /etc/kuksa-val/CA.pem --tls-server-name Server --protocol grpc --ip 10.42.0.95 --port 55555

Test Client> authorize eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJsb2NhbCBkZXYiLCJpc3MiOiJjcmVhdGVUb2tlbi5weSIsImF1ZCI6WyJrdWtzYS52YWwiXSwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE3NjcyMjU1OTksInNjb3BlIjoiYWN0dWF0ZSBwcm92aWRlIn0.x-bUZwDCC663wGYrWCYjQZwQWhN1CMuKgxuIN5dUF_izwMutiqF6Xc-tnXgZa93BbT3I74WOMk4awKHBUSTWekGs3-qF6gajorbat6n5180TOqvNu4CXuIPZN5zpngf4id3smMkKOT699tPnSEbmlkj4vk-mIjeOAU-FcYA-VbkKBTsjvfFgKa2OdB5h9uZARBg5Rx7uBN3JsH1I6j9zoLid184Ewa6bhU2qniFt5iPsGJniNsKsRrrndN1KzthO13My44s56yvwSHIOrgDGbXdja_eLuOVOq9pHCjCtorPScgEuUUE4aldIuML-_j397taNP9Y3VZYVvofEK7AuiePTbzwxrZ1RAjK74h1-4ued3A2gUTjr5BsRlc9b7eLZzxLJkrqdfGAzBh_rtrB7p32TbvpjeFP30NW6bB9JS43XACUUm_S_RcyI7BLuUdnFyQDQr6l6sRz9XayYXceilHdCxbAVN0HVnBeui5Bb0mUZYIRZeY8k6zcssmokANTD8ZviDMpKlOU3t5AlXJ0nLkgyMhV9IUTwPUv6F8BTPc-CquJCUNbTyo4ywTSoODWbm3PmQ3Y46gWF06xqnB4wehLscBdVk3iAihQp3tckGhMnx5PI_Oy7utIncr4pRCMos63TnBkfrl7d43cHQTuK0kO76EWtv4ODEHgLvEAv4HA
```

### Configuration in Control Panel

```python
import os
import platform

python_version = f"python{'.'.join(platform.python_version_tuple()[:2])}"

CA = os.path.abspath(os.path.join(os.path.dirname(__file__), "../assets/cert/CA.pem"))

KUKSA_CONFIG = {
    "ip": '10.42.0.95',
    "port": "8090",
    'protocol': 'ws',
    'insecure': False,
    'cacertificate': CA,
    'tls_server_name': "Server",
}

WS_TOKEN = os.path.join(os.path.expanduser("~"), f".local/lib/{python_version}/site-packages/kuksa_certificates/jwt/super-admin.json.token")
GRPC_TOKEN = os.path.abspath(os.path.join(os.path.dirname(__file__), "../assets/token/grpc/actuate-provide-all.token"))
```

_Note_: the config only holds the default configuration, changes made in preference of `protocol`, `SSL (insecure) mode`, and `jwt` token are handled by the `settings` module.

## IC acceliration and Script (WIP)

This week I also started refactoring the IC script and the `acceleration` function triggered by the controls in the `IC` page to better show the acceleration and deceleration of a car, also reworking the triggers for the *Cruise controls* for the `Steering Controls` page.

---
## # What Next?

- Continue testing
- Continue work on supporting databroker
- Continue pushing code to Gerrit and issue patched as required.