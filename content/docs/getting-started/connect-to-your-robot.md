---
title: "Connect to Your Robot"
weight: 30
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Connect to your robot

## Everything is already installed :tada:

Reachy comes with a [Raspberry-Pi 4](https://www.raspberrypi.org) dedicated to its control. When you receive your Reachy, the board is pre-installed with all required softwares. **So, you don't have to install anything on your computer.**

This section is meant to:
* let you know how to connect to the Raspberry-Pi,
* give you a bit more information on how things are actually working. 

First, you can find the Raspberry-Pi inside the trunk of the robot, as shown on this picture below. If you need to find or re-flash the SD-card, that's where you will find it.

{{< img alt="Raspberry Pi location inside Reachy" src="raspi-in-reachy.jpg" width="600px" >}}


Then, if you want to update or re-install the system, we recommend you to rewrite the whole image. You can
use [etcher](https://www.balena.io/etcher/) to burn the ISO. You will need a SD-card with at least a 16Go capacity. You can find the latest ISO [here](https://github.com/pollen-robotics/reachy/releases/latest).

{{< hint danger >}}
**Backup**

Make sure to save and copy your work as it will be lost during the re-writing!
{{< /hint >}}

The image is based on the [Raspbian Buster OS](https://www.raspberrypi.org/downloads/raspbian/) (desktop version). We then installed our own software, mainly a few Python packages. 

{{< hint warning >}}
**Install on your own machine**

We provide a pre-install Raspberry-Pi to ensure that all Reachy are shipped ready to be used and with the same configuration. Yet, if you want to use your own computer to control Reachy, or if you want to customize the Raspbian image it is possible. It is a more complex approach and requires knowledge on development environment. Some components are dedicated to the Raspberry Pi (such as the camera and you will have to either use a Raspberry Pi or change the cameras).
{{< /hint >}}

What is left to configure is how you want to access your Reachy. There are a few options. We will present the most common below.

## Work directly on the robot

You can directly plug a keyboard, mouse (USB) and display (HDMI) on the back of robot and you should be good to go. You can also plug an ethernet cable.

{{< img alt="Reachy ports on the back" src="reachy-back-port.jpg" width="600px" >}}

{{< hint info >}}
**Login access**

The robot comes with the default login: _pi_ and a default password: _reachy_.
{{< /hint >}}

You will have access to the Raspbian GUI where you will be able to configure everything you need (WiFi, ssh access, login, etc). Please, refer to the [official Raspberry-Pi documentation](https://www.raspberrypi.org/documentation/) for more information.

{{< img alt="Raspberry-Pi homescreen on Reachy" src="raspi-homescreen.jpg" height="600px" >}}

## Connect Reachy to the network

You can also work from your own computer and access the robot remotely. To do so, you will first need to connect the robot to your network. 

There are multiple ways of doing that. If you already know how to connect a Raspberry-Pi, you can follow any  procedure that you are familiar with. The only modification from a standard Raspbian is the hostname: _reachy_ and the default password: _reachy_.

Otherwise, we will dive into details below.

### Using Ethernet

You can connect your Reachy using the ethernet socket on the back of the robot. Most configuration should work without any additional setup.


### Configuring the WiFi

Configuring the WiFi on a Raspberry-Pi for the first time can be a bit tricky. That's why we developed a simple web dashboard to help you do that. Yet, this dashboard is hosted on the robot, so you need to connect to it. 

This can be done:
* by connecting once via ethernet or by connecting a keyboard/screen directly to the robot (see above).
* or via Reachy hotspot: if the robot does not manage to connect to a known WiFi network it will create its own access point that you can join. A minute or two after booting your robot, you should see a new WiFi network named _Reachy-AP_. You can join it using the passphrase: _Reachy-AP_.

## Accessing the dashboard

Once connected to your robot, you can access the dashboard using your web-browser and connecting to [http://reachy.local](http://reachy.local).

{{< hint info >}}
**Using hostname:**
Note that to work, we use the mDNS protocol and access the robot via its hostname. This should works without any configuration under Mac OS and GNU/Linux or recent version of Windows 10. If it does not work we recommend you to install [Bonjour Print Services for Windows](https://support.apple.com/kb/DL999).
{{< /hint >}}

Alternatively, you can directly access it using the robot IP address on your network (see with your FAI or IT to get the information). The Find App for smartphone may also help you find it. It is available for [Android](https://play.google.com/store/apps/details?id=com.overlook.android.fing) and [iOS](https://apps.apple.com/gb/app/fing-network-scanner/id430921107). More information is accessible on the [Raspberry-Pi official documentation](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).

When you've found reachy's IP, you can directly access the dashboard on [http://192.168.0.42/](http://192.168.0.42) where you replace _192.168.0.42_ by the found IP.

### Setup a new WiFi using the dashboard

On the left column of the dashboard, you should see a _WiFi Settings_ option. When you click on it, you should see something similar to the screenshot below.

{{< img alt="Dashboard WiFi settings" src="dashboard-wifi-hotspot.png" width="600px" >}}

You can see that it's currently on the Hotspot configuration. You can fill the _Add new WiFi_ form on the right, then click on the _UPDATE CONFIG_ button:

{{< img alt="Dashboard new WiFi" src="dashboard-setup-wifi.png" width="600px" >}}

Reboot your robot and it should connect to your freshly setup WiFi. If you come back to the _Wifi Settings_ page, you should now see:

{{< img alt="Dashboard WiFi connected" src="dashboard-wifi-connected.png" width="600px" >}}

## Access the robot via ssh

You can also access Reachy via ssh.

{{< columns >}}
**Using the IP**
```bash
ssh pi@192.168.0.42
```
Replace 192.168.0.42 with the IP you found.
<--->
**Using ZeroConf**
```bash
ssh pi@reachy.local
```
{{< /columns >}}

_Note: for information on how to use ssh on your own machine, please refer to [the Raspberry-Pi documentation](https://www.raspberrypi.org/documentation/remote-access/ssh/)._

{{< hint warning >}}
**Login access**

The robot comes with the default login: _pi_ and a default password: _reachy_. **Make sure to change the password if you enable remote access! Or even better, only connect using ssh key.**
{{< /hint >}}


