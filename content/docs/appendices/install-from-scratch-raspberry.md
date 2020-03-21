---
weight: 10100
title: "Install from Scratch on a Raspberry-Pi"
---

# Install from Scratch on a Raspberry-Pi

This guide describe how we generate our ISO. Its main use is for us to keep it up-to-date and always accessible for our team. You can freely use it and adapt it to your needs but we don't intend to explain every details here.

## Prepare the Raspberry-Pi

* Burn an SD-Card (16Go or more) using [Raspbian Buster with desktop - February 2020](https://www.raspberrypi.org/downloads/raspbian/).
* Make sure to allow ssh (```touch ssh``` on the _boot_ partition) or directly work from the Pi board using a mouse, keyboard and screen.
* Power your Raspberry-Pi and connect it to internet via Ethernet (WiFi is not configure at this point).
* Once booted, connect to it using ssh and the pi login (```ssh pi@raspberrypi.local``` password _raspberry_)

## Raspi-config

Once booted and logged in the Raspberry-Pi, we need to run raspi-config to setup few things. So, on the Pi bash run ```sudo raspi-config``` (We will give the number/letter corresponding to the entry item for each command below)

* Change user password to _reachy_: _1_
* Change hostname to _reachy_: _2-N1_
* Turn on WiFi (you can set the country and your own WiFi if you want but this is not necessary) _2-N2-Cancel-Cancel-_
* Turn on the camera _5-P1-Yes_
* Finish and reboot

## Install Reachy needed software

* Reconnect ```ssh pi@reachy.local``` (password _reachy_)
* Create our own work folder ```mkdir dev``` and ```cd dev```
* Clone Reachy software: ```git clone https://github.com/pollen-robotics/reachy```
* Checkout latest release ```cd reachy```and ```git checkout tags/v1.0.0.a2```
* Install atlas for scipy: ```sudo apt install -y libatlas-base-dev```
* Install deps for opencv: ```sudo apt install -y libhdf5-dev libhdf5-serial-dev libhdf5-103 libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5 libatlas-base-dev libjasper-dev```
* Install Reachy dependencies using the system python3: ```pip3 install -r ~/dev/reachy/software/requirements.txt```
* Install Reachy software using the system python3: ```pip3 install -e ~/dev/reachy/software```
* Get the Access Point and dashboard libraries: ```cd ~/dev```and ```git clone https://github.com/pollen-robotics/RAP```
* Install them: ```cd ~/dev/RAP/``` and ```sudo bash install.sh``` (Say Yes both time when asked during install)

## Install extra stuff

* Install IPython, Jupyter, Matplotlib: ```pip3 install ipython jupyter matplotlib```

## Check if everything is fine

* Reboot
* Connect to reachy access point (SSID: _Reachy-AP_)
* Connect to the dashboard: http://reachy.local


## Generate ISO

* You will need another Raspberry-Pi with lots of space (64Go) to do that
* Connect the reachy SD-card to the other Raspberry PI (using an USB adaptor)
* Create an img using dd (check the disk number using ```sudo fdisk -l```): ```sudo dd bs=4M if=/dev/sda of=reachy-$(date +%F).img conv=fsync```
* download shrink: ```wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh```
* Run ```sudo bash pishrink.sh -p -z reachy-$(date +%F).img```
* Share the create compressed img!
