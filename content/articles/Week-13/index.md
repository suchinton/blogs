---
title: "Week 13 Progress Report"
date: 2023-08-26
tags: ["GSoC 2023", "AGL", "The Linux Foundation", "GSoC '23 WPR"]
---

### # Topics To Be Covered In This Post
- What I did this week
	- Subscribe to VSS signals for registering changes made on target.
	- Refactored the toggle inputs as animated switches.
	- Start working with Kuksa-client 0.4.0 
- What I plan to do next 

---

## # Subscribing to VSS signals

This week I added support for subscribing to `VSS Signals` for the `IC` and `HVAC` Pages such that any changes made to their values through a different interface will now also be reflected on the Control Panel without delay.

## # Refactoring Toggles

I also replaced the checkbox inputs for `Insecure Mode` and `CAN to Kuksa` toggles with animated toggle switches, Making the inputs more touch friendly and easy to understand.

<video src="/images/WPR/Week13/Demo.mp4" controls="controls" style="max-width: auto; border-radius: 10px">
</video>

## # Kuksa-client 0.4.0

This week also saw slight modifications made to the `Kuksa_Insatance` module to support the newly released `kuksa-client` version 0.4.0.

However, I am facing setbacks while enabling SSL communication (Insecure Mode: False) between the host and the server when running on separate machines connected via LAN.

**Command line mode**:
```bash
kuksa-client --ip 10.42.0.95 --port \
8090 --protocol ws \
--cacertificate ./kuksa_certificates/CA.pem
```

**Control Panel Config**: I also tried modifying the configuration to specify the default config as follows,
```python
KUKSA_CONFIG = {
    "ip": 'localhost',
    "port": "8090",
    'protocol': 'ws',
    'insecure': False,
    'cacertificate': os.path.join(os.path.expanduser("~"),
                                    f".local/lib/{python_version}/site-packages/kuksa_certificates/CA.pem"),
    'certificate': os.path.join(os.path.expanduser("~"),
                                    f".local/lib/{python_version}/site-packages/kuksa_certificates/Client.pem"),
    'keyfile': os.path.join(os.path.expanduser("~"),
                                    f".local/lib/{python_version}/site-packages/kuksa_certificates/Client.key"),
}
```

While both of these allow for `Secure` and `Insecure` connections on the same machine, I am yet to be able to replicate this on the intended setup. 

---
## # What Next?

- Add tool tips & and support for notifications. 
- Revisit the script for the IC page.
- Refactor the code wherever necessary & and clean up unused assets.  
- Submit new patch set on Gerrit for review.