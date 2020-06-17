---
weight: 700
title: "Reachy simulation"
---

# Reachy simulation

Based on the [Unity engine](https://unity.com), we provide our own visualisation and simulation tool for reachy. 

* It lets you try to move the simulated robot without any risk of breaking anything. It’s a good way to learn how to control Reachy without fear.
* You can try yourself controlling a simulated Reachy before purchasing one, to better understand what it can and cannot do.
* It can be used extensively and with as many robots as you want for instance to train your machine learning algorithm.

This tool is still rather primitive at the moment, but we are dedicated to improve it and add new features in the coming weeks and month.

{{< img alt="Simulation presentation" src="simu/hello-code.gif" width="600px" >}}

## Quickstart

### What you need

So, what do you need to do the same on your own computer?

First, you don't need a real Reachy. You can access the simulator without having the real robot directly here: http://playground.pollen-robotics.com

What you only need is to install our Python’s package. It can be found on [GitHub](https://github.com/pollen-robotics/reachy) or directly on [PyPi](https://pypi.org/project/reachy/). It requires Python 3 and a few classical dependencies (numpy, scipy, etc). This software is the same one that runs on the real robot. We've simply included a specific IO layer that changes the communication with the hardware (motors and sensors) to use a WebSocket communication that interacts with the 3D visualisation.

You also need to use a Web browser that is compatible with WebGL, which is the case with all more or less recent browsers. It will also most likely not work on mobile devices, as Unity support is only partial at this time (See https://docs.unity3d.com/Manual/webgl-browsercompatibility.html for details).

### Getting started

Both hardware and simulated robot share the same API, to let you switch from simulation to a real robot back and forth easily.

They are a few difference that you should take into consideration.

First, when instantiating your robot, you need to specify that you want to use the simulated version. To do that, you simply set all IOs to _'ws'_ (instead of specifying the USB port on a real Reachy):

```python
from reachy import parts, Reachy

r = Reachy(
    right_arm=parts.RightArm(io='ws', hand='force_gripper'),
    left_arm=parts.LeftArm(io='ws', hand='force_gripper'),
)
```

This creates a web socket server in the background that connects to the Unity simulation. When opening  [http://playground.pollen-robotics.com](http://playground.pollen-robotics.com), click on the 'Connect' button and you should see the connection status going green, meaning both are synced.

Then, you can run command using Python’s API and you should see the visualisation move!
For instance:

```python
r.right_arm.elbow_pitch.goal_position = -80
```

### Navigate in the 3D scene

Once in the simulation, you can control the camera and rotate around reachy (with the left click) and move or zoom (with the mouse wheel).
Four buttons on the left allow you to show or hide reachy's components if you want to focus on specific parts.

## Difference with controlling a real robot

They are still some difference between the real robot and its simulation equivalent. Some will be erased in the future, some are inherent to this kind of problem.

### What's currently missing (and should be added soon)

At the moment, we do not provide support in the simulation for:

* the head part (you can see it but not control it)
* the present position can not be read

Those limits should be removed in the next few days.

Extra sensors, such as the force sensor, are not implemented in the simulation. You can still access them from code to guarantee compatibility but they will return random or nan values. We are currently investigating how they can be implemented within Unity.

### What's more complex (but we are working on it!)

The current version of the simulation does not rely on physics at all. Motors are teleported to their goal position and no gravity force is applied on the robot.

We are planning to better integrate with Unity and take advantage of their new Physics API. This should allow us to have collision detection and interaction with the rest of the environment.
