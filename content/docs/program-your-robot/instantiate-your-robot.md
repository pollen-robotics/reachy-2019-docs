---
title: "Instantiate Your Robot"
weight: 210
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Instantiate your robot

The first step is to "instantiate" your robot. What we mean here, is that we will look for the different parts of your robot (connected via USBs on the Raspberry-Pi). We will identify them and check if all modules are connected.

We will also launch the synchronisation between the Raspberry-Pi and the different parts of your robot. The sensors value read from the robot will automatically be updated in your Python object. Similarly you will send command to your Robot effector hardware by simply affecting Python variables.

## Which parts are present on my Reachy?

Reachy is built around the concept of modular parts. A Reachy can be composed of:
* a trunk (with all electronics and power supply)
* one arm (left or right) or both with different kind of end-effectors
* a head

{{< figure src="https://www.pollen-robotics.com/img/modules.gif" width="50%" >}}

So, to instantiate your robot we have to specify which parts you want to use. If you are using a "full" Reachy, ie with both arm equipped with force gripper, and a head; you can run the following Python code on your Raspberry-Pi:

```python
from reachy import Reachy, parts

reachy = Reachy(
    left_arm=parts.LeftArm(
        io='/dev/ttyUSB*',
        hand='force_gripper',
    ),
    right_arm=parts.RightArm(
        io='/dev/ttyUSB*',
        hand='force_gripper',
    ),
    head=parts.Head(
        io='/dev/ttyUSB*',
    ),
)
```

And if you have only the right arm and the head:

```python
from reachy import Reachy, parts

reachy = Reachy(
    right_arm=parts.RightArm(
        io='/dev/ttyUSB*',
        hand='force_gripper',
    ),
    head=parts.Head(
        io='/dev/ttyUSB*',
    ),
)
```

If you don't see any error, good news, you are now connected to your Robot and all the parts have been found! :tada: :tada: :tada:

{{< hint danger >}}
If it didn't work, check the [FAQ](../getting-started/faq) or the [forum](https://forum.pollen-robotics.com)! Your problem is most likely already describe there.
{{< /hint >}}

### Going deeper: the arm part

Let's dive a bit into the details of the code above.

```python
from reachy import Reachy, parts
```

Our API is available through the [reachy](https://github.com/pollen-robotics/reachy) Python module. This is the main entry point for controlling your robot.

```python
    right_arm=parts.RightArm(
        io='/dev/ttyUSB*',
        hand='force_gripper',
    ),
```

Here, we specify that we want to add a _Right Arm_ part and it should be found on a USB serial port of type "/dev/ttyUSB*". This is the standard name for the serial port on a Linux system. On other OS the name may differ (e.g. COM* on Windows).

Then, we specify which types of hand are attached to the arm. In our cases we set it to "force_gripper".  

### Going deeper: the head part

```python
    head=parts.Head(
        io='/dev/ttyUSB*',
    ),
```

Similarly to the arm, we define the USB port on which we should find the part "/dev/ttyUSB\*".

### Reachy: putting everything together

The _Reachy_ object is mainly a container to regroup the different parts. It also provides you higher level methods, for instance to program a complex motion happening on multiple parts at the same time.

```python
reachy = Reachy(
    left_arm=...,
    right_arm=...,
    head=...,
)
```

## Reachy's modularity: based on [Luos technology](https://www.luos-robotics.com/)

To permit the parts system in Reachy, we are relying on the [Luos technology](https://www.luos-robotics.com/). This modular system is based around the concept of using a tiny electronic Luos board for each sensors or effectors in your robot. They can be daisy chained and are automatically detected and recognized.

Those modules are exposed to the Raspberry-Pi using a [Luos gate](https://luos-robotics.github.io/index.html) that communicates via USB-serial interface.

<!-- TODO: photo -->

Each part is composed of several Luos modules:

* Arm
    * a dynamixel module to communicate with the motors
    * a force sensor module for the gripper
    * a gate to communicate with the Raspberry-Pi
* Head
    * three DC motor controllers for Orbita
    * a dynamixel module for the antennas
    * a gate to communicate with the Raspberry-Pi
