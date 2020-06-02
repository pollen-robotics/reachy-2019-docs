---
title: "FAQ"
weight: 50
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# FAQ

## Can I run the software on my own computer?

Yes, most of it. Our main required dependency is USB to serial communication using the [serial library](https://pyserial.readthedocs.io/en/latest/index.html). 
Yet, you will not have access to the camera or our vision primitive that relies on specific hardware (Raspberry-Pi camera and Google Coral TPU).

## Where are the installed packages on the Raspberry-Pi?

All python packages can be found in _$HOME/dev_.

## Can I have multiple instances connected to the robot at the same time?

No! Only one Python software may access the hardware at the same time. So make sure to close all previous softwares/notebooks before trying to connect.