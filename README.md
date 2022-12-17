# vm-utm-pfsense

Since the SAIT - Secure Networking Fundamentals (ITSC-200-A) course requires [pfSense](https://www.pfsense.org) as a virtual router, a Kali machine as a user machine in a local LAN network, and an Ubuntu machine as a server machine in another LAN (OPT1) network.

Below are the steps to set up pfSense on a Mac machine with M1/M2 chips (ARM architecture).  
For this purpose, we will use the Mac VM app: [UTM](https://mac.getutm.app) (release [v4.0.9](https://github.com/utmapp/UTM/releases/tag/v4.0.9))

1. Go to the terminal and generate UUID for each internal network you want to have, for example, one for LAN, and one for OPT1.

    command: `uuidgen`

2. Add the below commands to the bottom of QEMU settings in your VM (we will use vmnet-host for our network architecture).

    `-device` `<whatever lan card you like>,`
    `mac=<generate random MAC address of you lan card>,`
    `netdev=<give it a name for the network>`
    `-netdev vmnet-host, `
    `id=<the same name you gave it in netdev>,`
    `net-uuid=<the UUID you generated for this network>`

For example:
* UUID generated for LAN: `C8E4774B-BA27-408D-B62F-86267973AFE0`
* UUID generated for OPT1: `42F88754-F5F2-41A7-A886-2288CAABB6CB`


For pfSense VM:
In the network setting, use `1 network card` (default card) in GUI and set it as `Shared Network` (for WAN, sharing internet from Host).
Add the below commands to the bottom of `QEMU` settings.
```
-device e1000,mac=15-EE-C4-92-60-81,netdev=lan1 -netdev vmnet-host,id=lan1,net-uuid=C8E4774B-BA27-408D-B62F-86267973AFE0
-device e1000,mac=BD-90-CE-A9-7A-B1,netdev=lan2 -netdev vmnet-host,id=lan2,net-uuid=42F88754-F5F2-41A7-A886-2288CAABB6CB
```

For Kali VM:
In the network setting, use `0 network card` in GUI (Remove the default card).
Add the below commands to the bottom of `QEMU` settings.
```
-device e1000,mac=63-A4-6A-79-AA-35,netdev=lan1 -netdev vmnet-host,id=lan1,net-uuid=C8E4774B-BA27-408D-B62F-86267973AFE0
```

For Ubuntu VM:
In the network setting, use `0 network card` in GUI (Remove the default card).
Add the below commands to the bottom of `QEMU` settings.
```
-device e1000,mac=A9-B2-18-A8-E9-54,netdev=lan2 -netdev vmnet-host,id=lan2,net-uuid=42F88754-F5F2-41A7-A886-2288CAABB6CB
```
This is how we can make UTM to use internal networks for our VM(s).
