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

Reachy comes with a [Raspberry-Pi 4](https://www.raspberrypi.org) dedicated to its control. When you receive, your Reachy, the board is pre-installed with all required softwares. **So, you don't have to install anything on your computer.**

This section is meant to give you a bit more information on how things are actually working. First, you can find the Raspberry-Pi inside the trunk of the robot, as shown on this picture:

**TODO**

Then, if you want to update or re-install the system, we recommend you to rewrite the whole image. You can
use [etcher](https://www.balena.io/etcher/) to burn the ISO. You will need a SD-card with at least a 16Go capacity. You can find the latest ISO [here](TODO).

{{< hint danger >}}
**Backup**

Make sure to save and copy your work as it will be lost during the re-writing!
{{< /hint >}}

The image is based on the [Raspbian Buster OS](https://www.raspberrypi.org/downloads/raspbian/) (desktop version). We then installed our own software, mainly a few Python packages. 

{{< hint warning >}}
**Install on your own machine**

We provide a pre-install Raspberry-Pi to ensure that all Reachy are shipped ready to be used and with the same configuration. Yet, if you want to use your own computer to control Reachy, or if you want to customize the Raspbian image it is possible. It is a more complex approach and requires knowledge on development environment. The whole process will soon be described in its own section.
<!-- The details can be found on the [_Install from scratch_](TODO) documentation. -->
{{< /hint >}}

## Connect to your Reachy

What is left to configure is how you want to access your Reachy. There are a few options. You will present the most common below.

### Work directly on the robot

You can directly plug a keyboard, mouse and display on the back of robot and you are good to go. 

{{< hint info >}}
**Login access**

The robot comes with the default login: _pi_ and a default password: _reachy_.
{{< /hint >}}

**TODO: image**

You will have access to the Raspbian GUI where you will be able to configure everything you need (WiFi, ssh access, login, etc). Please, refer to the [official Raspberry-Pi documentation](https://www.raspberrypi.org/documentation/) for more information.


### Access it via the network

You can also work from your own computer and access the robot remotely. To do so, you will first need to connect the robot to your network. 

If you already know how to connect a Raspberry-Pi, you can follow your usual procedure. The only modification from a "vanilla" Raspbian is the hostname: _reachy_ and the default password: _reachy_.

{{< hint warning >}}
**Login access**

The robot comes with the default login: _pi_ and a default password: _reachy_. **Make sure to change the password if you enable remote access! Or even better, only connect using ssh key.** The hostname is  _reachy_.
{{< /hint >}}

To connect to your robot, you will need to either:

* know its IP address on your network (see with your FAI or IT to get the information)
* use the mDNS protocol and access the robot via its hostname

More information is accessible on the [Raspberry-Pi official documentation](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).

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

#### Using Ethernet

You can connect your Reachy using the ethernet socket on the back of the robot. Most configuration should work without any additional setup.

**TODO: image**

#### Configuring the WiFi

Configuring the WiFi on a Raspberry-Pi for the first time can be a bit tricky. Here, again we strongly recommend to follow the [official guide](https://www.raspberrypi.org/documentation/configuration/wireless/). 

The simplest way, by far, is to first directly connect to the board via a keyboard and display (see [this section](#work-directly-on-the-robot)) and use the GUI to configure the WiFi.

You can also first connect to the board via Ethernet and configure the WiFi via ssh in command line (see [this documentation](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)).

Finally, if none of the options are available for you, you will need to open the robot and remove the SD-card to connect it to your computer and write the [wpa_supplicant.conf file](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md).

