---
title: "Control The Arm"
weight: 220
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Control the arm

Once you have instantiated the _Reachy_ object, you are actually connected to your robot. This means that **the hardware (sensors and effectors) are synced with their software equivalent**. This synchronization loop runs at about 100Hz. In particular, this lets you retrieve the arm(s) state and send commands to make them move. 

**Before actually running some commands, it's important to make sure you are ready**:

* Make sure the robot is in a "safe" state (as shown in the images **TODO** for instance). If a wire is blocked or a motor is completly reversed, sending target position or even turning the motor on may result in a violent motion that may damage the robot.
* The motor are powerful so they can make the whole arm move fluidly. Do not hesitate to first try the move you want to make, with a low speed (we will show how to do that below). When you are sure everything is okay, you can increase the speed again.

{{< hint danger >}}
More information is available on the [Safety first section](../posts/safety). It provides simple guidelines to avoid damaging your robot.
{{< /hint >}}

{{< hint warning >}}
In the examples below, we will show examples on a right arm. Using the left arm instead should be straightforward. 
Always make sure that the different commands suggested below make sense in your context:
* angles from a right arm to a left arm may differ
* if your robot is attached in front of a desk or can freely move
* etc.
{{< /hint >}}

Everything is setup? It's time to make Reachy move!

<!-- **TODO: GIF** -->

## Moving individual motors

Reachy's upper arm is composed of 4 motors:

* _shoulder_pitch_
* _shoulder_roll_
* _arm_yaw_
* _elbow_pitch_

And depending on the hand used, usually 3 or 4 others motors. For the force gripper, you have:

* _forearm_yaw_
* _wrist_pitch_
* _wrist_roll_
* _gripper_

Each of these motors can be controlled individually. 

{{< hint info >}}
Higher-level ways to control the whole arm are shown in the next sections.
{{< /hint >}}

They are servo motors that you **control in position**. You can also control their **compliancy** or their **torque limit**. Theses motors are also behaving as sensor, so their **present position** can also be read at any time.

To access a specific motor (here _elbow_pitch_) in Python code:
```python
reachy.right_arm.elbow_pitch
>>> <DxlMotor "right_arm.elbow_pitch" pos="-83.209" mode="stiff">
```

And to get its present position:
```python
reachy.right_arm.elbow_pitch.present_position
>>> -83.121
```

{{< hint info >}}
The codes above assume that you already instantiated your Reachy, as shown in the section [Instantiate Your Robot](../instantiate-your-robot/) and assigned it to the _reachy_ variable.
{{< /hint >}}

In a more general manner, you can access any motor by using ```reachy.part_name.motor_name```. 

{{< hint info >}}
Please note that the part name can also be nested (e.g. _right_arm.hand_). All available parts are:

* _right_arm_
* _right_arm.hand_
* _left_arm_
* _left_arm.hand_
* _head_

If one part is not present in your robot, it's value will simply be _None_.
{{< /hint >}}



For each motor, we have defined angle limits. They correspond to what moves the arm can actually make. For instance, the elbow pitch is limited from 0° to 125°. The forearm yaw can move from -150° to 150°. 

The zero position of each motor is defined so as when all motors are at 0°, the arm is straight down along the trunk. For more information on the motor orientation and configuration, please refer to the section **TODO**.

<!-- **PHOTO** -->

{{< hint warning >}}
While each motors has angle limits corresponding to what it can do, all combinations of these limits are not reachable! When moving a motor, you always have to take into consideration the configuration of the other motors.
{{< /hint>}}

### Compliant or stiff

The servo motors used in Reachy's arm have two operating modes:

* **compliant**: the motors is soft and can be freely turned by hand. It cannot be controlled, setting a new target position will have no effect. Yet you can still read the motor position.
* **stiff**: the motors is hard and cannot be moved by hand. It can be controlled by setting new target position.

When you turn on your robot, all motors are compliant. You can freely move Reachy's arm and place it in its base position. 

To make Reachy keep its position and let you control its motors, you need to turn them stiff. To do that, you can use the _compliant_ property.

For instance, to make the elbow stiff, run the following code:

```python
reachy.right_arm.elbow_pitch.compliant = False
```

Now, the elbow should be hard, you cannot move it by hand anymore.

To turn it back compliant, simply run:

```python
reachy.right_arm.elbow_pitch.compliant = True
```

### Setting a new target position for a motor

Assuming the motor is now stiff, you can now make it move. Once again, make sure the target position you will set corresponds to a reachable position in your configuration.

To make our motor move, we will use the [goto method](https://pollen-robotics.github.io/reachy/autoapi/reachy/parts/motor/index.html#reachy.parts.motor.DynamixelMotor.goto). We will define a target position and a move duration. Let's try having our elbow at 90° in 2s:

```python
reachy.right_arm.elbow_pitch.goto(
    goal_position=90,  # in degrees
    duration=2,  # in seconds
    wait=True,
)
```
When running this code, you should see the motor move. 

{{< hint warning >}}
The _wait=True_ option blocks until the move is actually done. You can set it to False and the function will return immediatly.

**Be careful not to have two trajectories running on the same motor in parallel!** This could result in an unpredicted behavior (Trajectory are not thread safe).

{{< /hint >}}


Let's try to move another motor, the _arm_yaw_:

```python
reachy.right_arm.arm_yaw.goto(
    goal_position=20,
    duration=2,
    wait=True,
)
```

Run this code... And if you followed this documentation closely, your motor should not have moved... Can you guess why? Yes, indeed the motor is still compliant. You have to turn it stiff and then run the code above again.

{{< hint warning >}}
As a safety measure, when turning a motor stiff, its goal position is reset. This avoids to directly jump to a new position when you turn the motor stiff. Indeed, the target position is stored inside the motor and could be kept even if you restart your Python script.
{{< /hint >}}

### goto vs goal_position

To control & motor in the examples above we used the _goto_ method. A lower API to control the motor is to directly use the [_goal_position_](https://pollen-robotics.github.io/reachy/autoapi/reachy/parts/motor/index.html#reachy.parts.motor.DynamixelMotor.goal_position) property.

Yet, you should be careful when doing so, because the motor **will try to reach this new goal position as fast as it can**. And it is really fast (up until ~600-700 degrees per sec)! A workaround is to also use the _moving_speed_ property to set the maximum speed that the motor can reach.

```python
reachy.right_arm.elbow_pitch.moving_speed = 50  # in degrees per sec 
reachy.right_arm_elbow_pitch.goal_position = 110  # in degrees
```

Yet, in our experience, when using this approach for controlling a motor, it may be hard to follow smoothly complex trajectories and have precise timing. You only set the maximum speed but **have no control over the acceleration**.

The approach used in the _goto_ method differs in a sense that it only using position control (the maximum speed is allowed)

**A full position trajectory profile is actually generated going from the current position to the goal position**. This trajectory is then interpolated at a predefined frequency (100Hz) to compute all intermediary target position that should be followed before reaching the final goal position. 
Depending on the interpolation mode chosen, you can have a better control over speed and acceleration. More details are given below.

For instance, the code below will make the _elbow_pitch_ motor follow this curve (assuming the motor was still in 110°):

```
reachy.right_arm.elbow_pitch.goto(
    goal_position=80,  # in degrees
    duration=1,  # in seconds
    wait=True,
    interpolation_mode='minjerk',
)
```

{{< img alt="Minimum Jerk Trajectory example" src="minjerk-example.png" width="600px" >}}


{{< hint info >}}
Remember to reset the maximum speed of the _elbow_pitch_ before runing any _goto_ on it. To reset the maximum speed of a motor (using no speed limit) simply set it to 0:

```python
reachy.right_arm.elbow_pitch.moving_speed = 0 
```

{{< /hint >}}

## Moving multiple motors at once

Most of the time when controlling a robot, you want to move multiple motors at once. While you can do it by running multiple goto and using the option _wait=False_, there is a simpler way.

You can actually use a [_goto_](https://pollen-robotics.github.io/reachy/autoapi/reachy/index.html#reachy.Reachy.goto) the robot level and specify a list of (motor, target_position). This will create a trajectory for each motor and run them all in parallel.

To move both antennas at the same time, you can run:
```python
reachy.goto(
    goal_positions={
        'head.left_antenna': 90,
        'head.right_antenna': -90,
    },
    duration=2,
    wait=True,
)
```

{{< hint info >}}
Note the use of the complete motor name (eg. 'head.left_antenna') as a key string in the goal_positions dict.
{{< /hint >}}

Motors from different parts can be mixed in the same _goto_.

```python
reachy.goto(
    goal_positions={
        'head.left_antenna': 0,
        'head.right_antenna': 0,
        'right_arm.elbow_pitch': 90,
    },
    duration=2,
    wait=True,
)
```

You can concatenate multiple goto simply by using the _wait=True_ option and running both codes sequentially:

```python
reachy.goto(
    goal_positions={
        'head.left_antenna': 90,
        'head.right_antenna': -90,
    },
    duration=2,
    wait=True,
)
reachy.goto(
    goal_positions={
        'head.left_antenna': 0,
        'head.right_antenna': 0,
    },
    duration=1,
    wait=True,
)
```

## Different trajectory interpolation

All goto accept a _interpolation_mode_ argument. This lets you define the way you want the trajectory to interpolate from A to B.

It cames with two basic way:

* linear interpolation
* [minimum jerk](https://ieeexplore.ieee.org/document/12075)

Running the same goto (from 0° to 90°) and changing the interpolation mode from _linear_ to _minjerk_ will result in the two different trajectories:

{{< img src="minjerk-vs-linear.png" alt="Comparison of Linear and Minjerk interpolation" width=600px >}}

Note that both trajectories start and finish at the same points. Yet, the followed positions and therefore speed and acceleration differs quite a bit. The Minimum Jerk will slowly accelerate at the begining and slowly decelerate at the end.

{{< hint warning >}}
What's show in the figure above is a theoretical trajectory. The motor has a low level controller that will try to follow this curve as closely as possible. Yet, depending on the speed/acceleration that you try to reach and the motor configuration, if it has to move a big loads, the real and theoretical curves may differ.
{{< /hint >}}



## Grasping

## Record trajectories

## FK & IK