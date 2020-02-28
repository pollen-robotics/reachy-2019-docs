---
title: "Head"
weight: 180
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Reachy's head specifications

Reachy’s head features two cameras: one to observe its environment and another camera to focus on the task of manipulating.
The head is animated by Orbita, a unique technology developed by Pollen Robotics' R&D team. This ball joint actuator allows unpreceded dynamic and multi-directional movement.
With animated antennas, Reachy can convey many emotions to his audience.

Coral development board G950-01456-01

See the head in action in this video: https://youtu.be/X9dgsLX_u9I

## Cameras

2 Raspberry Pi cameras associated with 2 optical lenses (one macro and one wide angle). See details on Raspberry Pi Camera module here: https://www.raspberrypi.org/documentation/hardware/camera/

## Orbita neck joint

Ball joint actuator that is composed of a parallel mechanism motorized by 3 DC Maxon motors. The control of each motor is done with a Pololu magnetic encoder and an LUOS DC motor module.


## Antennas

Antennas are animated by a Dynamixel motor and are removable. A system of 3 magnets (2 south and 1 north) allow to attach the antennas to the rotation axis.

