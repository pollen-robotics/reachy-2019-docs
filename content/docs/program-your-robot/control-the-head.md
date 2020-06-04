---
title: "Control The Head"
weight: 230
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Control the head

The head is an important part of Reachy. It's where its camera are located and it participates a lot to the robot expressivity.

{{< img src="reachy-head.gif" width="50%" >}}

In the following sections, you will see how:

* you can orient the head by controlling its neck,
* to move the antennas,
* and use its cameras.

In the examples below, we will assume that you have already instantiated the head part of a Reachy as show in the section [Instantiate Your Robot](../instantiate-your-robot/).

## Homing

Before being able to control the head, there is a required calibration step. The position coders used in the Orbita neck only provides relative positionning. The calibration procedure will automatically find the zero position of the head for you.

This calibration, also called homing, is rather simple. All three disks composing orbita will turn in the same direction, making the head turn to the left until they reach their limit. We can used this left physical limits as a known absolute position (-160° for all three disks). We then go back to the zero position, meaning when the head is looking straight forward.

To do the homing:

* First, make sure the head can freely turn.
* Then, run the following python line of code:

```python
reachy.head.homing()
```

You should see the head realises the motion described above and end up in its base position. The whole procedure takes about 5 seconds.

{{< hint info >}}
The head homing must be performed every time you start your robot after powering it off.
{{< /hint >}}

## Looking around

Now that the head has been calibrated, we can start controlling it. We will show you two main ways to control the neck and orient the head:

* Moving each disks individually
* Specifing look points in space.

The neck has 3 degrees of freedom (one per disk). When combined those 3 rotations will allow you to set a 3D orientation of the head.

{{< img src="orbita-schema.png" width="50%" >}}


### Controlling the disks individually

Each disk is actuated by a motor that can be controlled in almost the same way than motors from the arm. In particular, you will be able to:

* turn them stiff/compliant,
* set a new target position,
* define a maximum speed,
* read their current position (in degrees),
* get their current temperature (in °C).

{{< hint info >}}
It's important to understand how you can control the disks of Reachy's head as it's always how they are control behind the scene. Yet, in the next section we will show you simpler way to control the head orientation and look at specific 3D points in space. 
{{< /hint >}}

#### Stiff/Compliant

Like for the motors of the arm, the head can be used in two different modes:

* compliant: where you can freely move the head and still read its current position
* stiff: where the motors are hard and can be controlled in position.

You can change the mode for a disk by running:

To make it compliant:
```python
reachy.head.neck.disk_bottom.compliant = True
```

To make it stiff:
```python
reachy.head.neck.disk_bottom.compliant = False
```

You can also change the mode for the three disks using:

```python
for disk in reachy.head.neck.disks:
    disk.compliant = True
```

Or you can even use the shortcut:

```python
reachy.head.compliant = False
```

#### Control a disk

Like the motors of the arm, you can control the disk of the neck by setting new target position. You can also set maximum speed that will be used to reach those targets.

You can set a new target position of one disk using:

To set the disk_top to 20°:
```python
reachy.head.neck.disk_top.target_rot_position = 20
```

If the head was stiff, you should have seen of the arm move.

To move all three disks at the same time you can run:

```python
for disk in reachy.head.neck.disks:
    disk.target_rot_position = -20
```

We also provide a goto function that lets you control the move duration as well. For instance to go back to 0 position on all three disks in 1 second:

```python
reachy.head.neck.goto(thetas=(0, 0, 0), duration=1, wait=True)
```

The goto method supports the same extra arguments than for the arm version:

* wait (True/False): whether or not to wait for the end of the move
* interpolation_mode: to specify how the trajectory is computed (see [Trajectory interpolation](../control-the-arm/#different-trajectory-interpolation) for details).

#### Read positions or temperature

The current position of each disk can be retrieved via:

```python
for disk in reachy.head.neck.disks:
    print(disk.rot_position)
```

Similarly to access the temperature:

```python
for disk in reachy.head.neck.disks:
    print(disk.temperature)
```

The position are updated at about 100Hz while the temperature is updated at ~1Hz. The temperature is also used internally to trigger the fan inside the Orbita neck.

### Look at specific points in space

## Move antennas

## Access the camera