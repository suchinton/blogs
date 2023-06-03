---
title: GSoC Community Bonding Period
date: 2023-05-26
images: 
- https://raw.githubusercontent.com/suchinton/blogs/main/images/CBP/CBP.png
---

### # Topics To Be Covered In This Post
- What is the Community bonding period?
- What I did the past month
- Challenges faced
- Community interactions
- What I plan to do next 

---

## What is GSoC & What Have I Been Up To During The CBP?

Google Summer of Code (GSoC) is a global program that offers students an opportunity to contribute to open-source projects. The program is sponsored by Google and has been running since 2005. The program aims to bring together students and open-source organizations to work on real-world projects. The program has three phases: the community bonding period, the coding period, and the final evaluation period.

The community bonding period is the three weeks between GSoC student acceptance and the start of the coding date (May 4th - May 28th). During this period I engaged with my GSoC mentors, [Jan-Simon Möller](mailto:jsmoeller@linuxfoundation.org) ,[Walt Miner](mailto:wminer@linuxfoundation.org), [Scott Murray](mailto:smurray@konsulko.com) and [Marius Vlad](mailto:mvlad@collabora.com) and the rest of the AGL community. I used this period to get familiar with the various tools used in AGL and set up my work environment for the coding period. This also allowed me to better understand the requirements of my project.

I also participated in the  [AGL Weekly Dev Meet](https://wiki.automotivelinux.org/dev-call-info) events to better understand the work done at AGL and its various technical groups.  

## Setting Up The Build Environment

On our first weekly GSoC meetings, [Jan-Simon Möller](mailto:jsmoeller@linuxfoundation.org) was kind enough to set us up with the remote servers we may need to build the AGL images as building on local hardware is a time-consuming process. 

During my first attempt at building the [AGL Flutter Instrument Cluster demo image](https://docs.automotivelinux.org/en/master/#01_Getting_Started/03_Build_and_Boot_guide_Profile/02_Flutter_Instrument_Cluster_%28qemu-x86%29/), I encountered an issue where there wasn't enough storage available to build new images on the server. However, this should soon be resolved after a cleanup job. So, till then I'll continue building on my local machine.  

## Building AGL Locally

On May 11th, I faced an AGL Build failure on Pop!\_OS 22.04 LTS to which I found the solution at [yocto Docs](https://docs.yoctoproject.org/3.2.3/ref-manual/ref-system-requirements.html#ubuntu-and-debian).

The error was due to the `oss4-dev` package installed on my system, which resulted in QEMU build failures due to the package installing its own custom `/usr/include/linux/soundcard.h` on the Debian systems.

This was resolved by resolving the packages.

```bash
sudo apt-get build-dep qemu
sudo apt-get remove oss4-dev
```

## Working With Kuksa-val-server, Kuksa-client and AGL Images

So in order the test my proposed implementation, I initially tried running the Docker build of the Kuksa-val server on my machine to manipulate values on the [IC](https://github.com/aakash-s45/ic) flutter application using my Demo application [AGL-Kuksa.val-Visualiser](https://github.com/suchinton/AGL-Kuksa.val-Visualiser/tree/main/Demo_1), while it was successfully setting the required values for `Vehicle.Speed`, the IC application would require a hot reload every time to update the values. 

To overcome this, I and a doubt-clearing session with [Scott Murray](mailto:smurray@konsulko.com), which led me to use the [AGL Flutter Instrument Cluster demo image](https://docs.automotivelinux.org/en/master/#01_Getting_Started/03_Build_and_Boot_guide_Profile/02_Flutter_Instrument_Cluster_%28qemu-x86%29/), by using port forwarding and establishing a connection between the host and QEMU machine. This proved to be a failure since my host machine was able to ping the QEMU instance but not the other way around.

Then during the weekly GSoC meeting, [Marius Vlad](mailto:mvlad@collabora.com) cleared my doubts regarding this issue. He recommended I set up the communication interface using a bridge network and redirected me to the TAP network [guide](https://gist.github.com/extremecoders-re/e8fd8a67a515fee0c873dcafc81d811c) for testing out my implementation.

I modified the available script for my convenience and added comments to better understand the various steps.

### Setting Up A Network Bridge

*Note: script only works on the wireless interface*

To create the bridge network `br0` we run the `setup_tap_wireless_int.sh` script with elevated privileges. The script helps connect wireless devices to a network by creating a bridge between the wireless interface `wlp4s0` and the network interface. It also sets up a DHCP and DNS server for the bridge interface and creates rules to allow traffic to flow between the wireless and network interfaces. 

```bash
#!/bin/bash

# Find the wireless interface
WIRELESS=$(iwconfig 2>/dev/null | awk '/IEEE 802.11/ {print $1; exit}')

BRIDGE=br0
NETWORK=10.10.10.0
NETMASK=255.255.255.0
GATEWAY=10.10.10.1
DHCPRANGE=10.10.10.100,10.10.10.254

# Create the bridge interface
ip link add $BRIDGE type bridge
ip link set dev $BRIDGE up

# Assign an IP address to the bridge interface
ip addr add dev $BRIDGE $GATEWAY/$NETMASK

# Enable IP forwarding
sysctl -w net.ipv4.ip_forward=1 > /dev/null 2>&1

# Flush existing iptables rules and set default policies to ACCEPT
iptables --flush
iptables -t nat -F
iptables -X
iptables -Z
iptables -P OUTPUT ACCEPT
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT

# Allow DHCP and DNS traffic on the bridge interface
iptables -A INPUT -i $BRIDGE -p tcp -m tcp --dport 67 -j ACCEPT
iptables -A INPUT -i $BRIDGE -p udp -m udp --dport 67 -j ACCEPT
iptables -A INPUT -i $BRIDGE -p tcp -m tcp --dport 53 -j ACCEPT
iptables -A INPUT -i $BRIDGE -p udp -m udp --dport 53 -j ACCEPT

# Allow forwarding of packets between the bridge and the network
iptables -A FORWARD -i $BRIDGE -o $BRIDGE -j ACCEPT
iptables -A FORWARD -s $NETWORK/$NETMASK -i $BRIDGE -j ACCEPT
iptables -A FORWARD -d $NETWORK/$NETMASK -o $BRIDGE -m state --state RELATED,ESTABLISHED -j ACCEPT

# Accept packets from the bridge interface with source and destination within the network
# to prevent masquerading of bridged frames/packets
iptables -t nat -A POSTROUTING -s $NETWORK/$NETMASK -d $NETWORK/$NETMASK -j ACCEPT

# Perform network address translation (NAT) for packets from the network
iptables -t nat -A POSTROUTING -s $NETWORK/$NETMASK -j MASQUERADE

# Configure dnsmasq as the DHCP and DNS server for the bridge interface
dns_cmd=(
    dnsmasq
    --strict-order
    --except-interface=lo
    --interface=$BRIDGE
    --listen-address=$GATEWAY
    --bind-interfaces
    --dhcp-range=$DHCPRANGE
    --conf-file=""
    --pid-file=/var/run/qemu-dnsmasq-$BRIDGE.pid
    --dhcp-leasefile=/var/run/qemu-dnsmasq-$BRIDGE.leases
    --dhcp-no-override
)

# Execute the dnsmasq command
echo ${dns_cmd[@]} | bash

# Allow traffic from the bridge interface to the wireless interface
iptables -A FORWARD -i $BRIDGE -o $WIRELESS -j ACCEPT

# Perform masquerading for outgoing packets on the wireless interface
iptables -t nat -A POSTROUTING -o $WIRELESS -j MASQUERADE

# Allow known traffic from the wireless interface to return to the bridge interface
iptables -A FORWARD -i $WIRELESS -o $BRIDGE -m state --state RELATED,ESTABLISHED -j ACCEPT
```

### Starting The QEMU VM 

For utilizing the bridge `br0` created using the script, the VM needs to be started with elevated privileges, and the specified arguments.

```bash
sudo qemu-system-x86_64 -device virtio-net-pci,\
netdev=net0,mac=52:54:00:12:35:02 -netdev bridge \
,br=br0,id=net0 -drive file=agl-cluster-demo-platform-flutter-qemux86-64.ext4, \
if=virtio,format=raw -usb -usbdevice tablet \
-device virtio-rng-pci -snapshot -vga virtio -vnc :0 \
-soundhw hda -machine q35 -cpu kvm64 \
-cpu qemu64,+ssse3,+sse4.1,+sse4.2,+popcnt -enable-kvm \
-m 2048 -serial mon:vc -serial mon:stdio -serial null -kernel bzImage \
-append 'root=/dev/vda rw console=tty0 mem=2048M ip=dhcp oprofile.timer=1 console=ttyS0,115200n8 verbose fstab=no'
```

We enter `root` as the username and get access to the AGL command line. The IP address of the VM is verified using the `ip address show` command.

```bash
root@qemux86-64:~# ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,DYNAMIC,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:12:35:02 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.203/24 metric 1024 brd 10.10.10.255 scope global dynamic eth0
       valid_lft 3490sec preferred_lft 3490sec
    inet 10.10.10.204/24 brd 10.10.10.255 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe12:3502/64 scope link 
       valid_lft forever preferred_lft forever
3: sit0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/sit 0.0.0.0 brd 0.0.0.0
4: can0: <NOARP,UP,LOWER_UP> mtu 72 qdisc noqueue state UNKNOWN group default qlen 1000
    link/can 
```

### Starting Kuksa-val-server In QEMU Instance

By default Kuksa, and all the applications using it listen to the localhost ip `127.0.0.1` of the QEMU instance. Therefore,  to enable communication with the host OS, we need to run the server on the ip `10.10.10.204` in `--insecure` mode. 

```bash
kuksa-val-server --address 10.10.10.204 --log-level VERBOSE --insecure
```

### Connecting To Kuksa-val-server 

On the Host machine, we use the [Kuksa-client](https://github.com/eclipse/kuksa.val/tree/master/kuksa-client) in `cli` mode to test the connection with the given arguments. Kuksa-client Python SDK is a library that allows us to interact with either kuksa-val-server or kuksa_databroker. 

```bash
kuksa-client --ip 10.10.10.204 --insecure
```

Host Machine: 
```bash
Welcome to Kuksa Client version 0.3.1

                  `-:+o/shhhs+:`                  
                ./oo/+o/``.-:ohhs-                
              `/o+-  /o/  `..  :yho`              
              +o/    /o/  oho    ohy`             
             :o+     /o/`+hh.     sh+             
             +o:     /oo+o+`      /hy             
             +o:     /o+/oo-      +hs             
             .oo`    oho `oo-    .hh:             
              :oo.   oho  -+:   -hh/              
               .+o+-`oho     `:shy-               
                 ./o/ohy//+oyhho-                 
                    `-/+oo+/:.             

Default tokens directory: /home/suchinton/.local/lib/python3.10/site-packages/kuksa_certificates/jwt

connect to ws://10.10.10.204:8090
Websocket connected.
Test Client> authorize /home/suchinton/.local/lib/python3.10/site-packages/kuksa_certificates/jwt/super-admin.json.token 
{
  "TTL": 1767225599,
  "action": "authorize",
  "requestId": "26a3c251-7418-43df-bcc2-d4956726462e",
  "ts": "2023-05-27T18:22:54.1685211774Z"
}

Test Client> setValue Vehicle.Speed 12
{
  "action": "set",
  "requestId": "675a0cf6-fd12-47f3-8dd4-36ddf4b951bd",
  "ts": "2023-05-27T18:23:31.1685211811Z"
}
```

QEMU instance of AGL:
```bash
root@qemux86-64:~# kuksa-val-server --address 10.10.10.204 --log-level VERBOSE --insecure
kuksa.val server
Commit  from 
Read configs from /etc/kuksa-val/config.ini
Update vss path to /usr/share/kuksa-val/vss_release_3.1.1.json
Update cert-path to /etc/kuksa-val
Log START
VERBOSE: Try reading JWT pub key from /etc/kuksa-val/jwt.key.pub
VERBOSE: SubscribeThread: Started Subscription Thread!
VERBOSE: VssDatabase::VssDatabase : VSS tree initialized using JSON file = /usr/share/kuksa-val/vss_release_3.1.1.json
VERBOSE: Receive action: authorize
VERBOSE: VssCommandProcessor::processQuery: authorize query with token = eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJrdWtzYS52YWwiLCJpc3MiOiJFY2xpcHNlIEtVS1NBIERldiIsImFkbWluIjp0cnVlLCJtb2RpZnlUcmVlIjp0cnVlLCJpYXQiOjE1MTYyMzkwMjIsImV4cCI6MTc2NzIyNTU5OSwia3Vrc2EtdnNzIjp7IioiOiJydyJ9fQ.p2cnFGH16QoQ14l6ljPVKggFXZKmD-vrw8G6Vs6DvAokjsUG8FHh-F53cMsE-GDjyZH_1_CrlDCnbGlqjsFbgAylqA7IAJWp9_N6dL5p8DHZTwlZ4IV8L1CtCALs7XVqvcQKHCCzB63Y8PgVDCAqpQSRb79JPVD4pZwkBKpOknfEY5y9wfbswZiRKdgz7o61_oFnd-yywpse-23HD6v0htThVF1SuGL1PuvGJ8p334nt9bpkZO3gaTh1xVD_uJMwHzbuBCF33_f-I5QMZO6bVooXqGfe1zvl3nDrPEjq1aPulvtP8RgREYEqE6b2hB8jouTiC_WpE3qrdMw9sfWGFbm04qC-2Zjoa1yYSXoxmYd0SnliSYHAad9aXoEmFENezQV-of7sc-NX1-2nAXRAEhaqh0IRuJwB4_sG7SvQmnanwkz-sBYxKqkoFpOsZ6hblgPDOPYY2NAsZlYkjvAL2mpiInrsmY_GzGsfwPeAx31iozImX75rao8rm-XucAmCIkRlpBz6MYKCjQgyRz3UtZCJ2DYF4lKqTjphEAgclbYZ7KiCuTn9HualwtEmVzHHFneHMKl7KnRQk-9wjgiyQ5nlsVpCCblg6JKr9of4utuPO3cBvbjhB4_ueQ40cpWVOICcOLS7_w0i3pCq1ZKDEMrYDJfz87r2sU9kw1zeFQk with request id 26a3c251-7418-43df-bcc2-d4956726462e
VERBOSE: Receive action: getMetaData
VERBOSE: VssDatabase::getMetaData: VSS specific path =$['Vehicle']
VERBOSE: Receive action: set
VERBOSE: Set request with id 675a0cf6-fd12-47f3-8dd4-36ddf4b951bd for path: Vehicle.Speed with attribute: value
VERBOSE: SubscriptionHandler::publishForVSSPath: set value 12.0 for path Vehicle.Speed
```

*Yay!*

So, we're able to connect to the server and send the necessary signals from the host. But this is only a working solution as none of the AGL demo apps listen on this specific ip `10.10.10.204` for kuksa updates. 

## What Next?

During the Weekly GSoC and Dev meetings of AGL, [Scott Murray](mailto:smurray@konsulko.com) pointed out that soon AGL will be moving to [Kuksa-databroker](https://github.com/eclipse/kuksa.val/tree/master/kuksa_databroker) (written in RUST) from the currently used [Kuksa-val-sever](https://github.com/eclipse/kuksa.val/tree/master/kuksa-val-server). This should not affect my implementation as Kuksa-client works with both ( using Web-sockets & GRPC). This will give us the window to update our implementation and come to a working solution by the time the coding period starts. 

I will also be updating my working repository on GitHub soon, and start work on coding the GUI elements of the Application.

All in all, the Community bonding period proved to be extremely educational and very interactive, not just with the AGL community but also with my fellow GSoC contributors. I can't wait to get codding and look forward to overcoming the challenges that come my way.

## Resources I Found Useful

- https://docs.automotivelinux.org/en/master/
- https://docs.yoctoproject.org/3.2.3/index.html
- https://github.com/aakash-s45/ic
- https://gist.github.com/extremecoders-re/e8fd8a67a515fee0c873dcafc81d811c
- https://github.com/eclipse/kuksa.val
- https://github.com/eclipse/kuksa.val/tree/master/kuksa-val-server
- https://github.com/eclipse/kuksa.val/tree/master/kuksa-client

 [Week 1 Report →](/articles/Week_1.md)