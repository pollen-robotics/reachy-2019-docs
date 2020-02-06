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

Once you have instantiated the _Reachy_ object, you are actually connected to your robot. You are thus able to retrieve the arm(s) state and send commands to make them move. 

**Before actually running some commands, it's important to make sure you are ready**:

* Make sure the robot is in a "safe" state (as shown in the images **TODO** for instance). It a wire is blocked or a motor is completly reversed, sending target position or even turning the motor on may result in a violent motion that may damage the robot.
* The motor are powerful so they can make the whole arm move fluidly. Do not hesitate to first try the move you want to make, with a low speed (we will show how to do that below). When you are sure everything is okay, you can increase the speed again.

{{< hint danger >}}
More information is available on the [Safety first section](../posts/safety). It provides simple hints to avoid damaging your robot.
{{< /hint >}}

{{< hint warning >}}
In the examples below, we will show examples on a right arm. Using the left arm instead should be straightforward. 
Always make sure that the different commands suggested below make sense in your context:
* angles from a right arm to a left arm may differ
* if your robot is attached in front of a desk or can freely move
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

They are servo motors that you **control in position**. You can also control their **compliancy**, their **torque limit** among others. The motors are also sensors, so the real **present position** can also be retrieved.

To access a specific motor (here _elbow_pitch_) in Python code:
```python
reachy.right_arm.elbow_pitch
>>> **TODO**
```

And to get its present position:
```python
reachy.right_arm.elbow_pitch.present_position
>>> 88.54
```

{{< hint info >}}
The codes above assume that you already instantiated your Reachy, as shown in the section [Instantiate Your Robot](../instantiate-your-robot/) and assigned it to the _reachy_ variable.
{{< /hint >}}


For each motor, we have defined angle limits that corresponds to what moves the arm can actually make. For instance, the elbow pitch is limited from 0° to 125°. The forearm yaw can move from -150° to 150°. The zero position of each motor is defined so when all motors are at 0°, the arm is straight down along the trunk. For more information on the motor orientation and configuration, please refer to the section **TODO**.

<!-- **PHOTO** -->

{{< hint warning >}}
While each motors has angle limits correspind to what it can do, all combinations of these limits are not reachable! When moving a motor, you always have to take into consideration the configuration of the other motors.
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

To make our motor move, we will use the [goto method](**TODO LINK API**). We will define a target position and a move duration. Let's try having our elbow at 90° in 2s:

```python
reachy.right_arm.elbow_pitch.goto(
    goal_position=90,  # in degrees
    duration=2,  # in seconds
)
```

When running this code, you should see the motor move. Let's try to move another motor, the _arm_yaw_:

```python
reachy.right_arm.arm_yaw.goto(
    goal_position=0,
    duration=2,
)
```

Run this code... And if you followed this documentation, your motor should not have moved... Can you guess why? Yes, indeed the motor is still compliant. You have to turn it stiff and then run the code above.

### _goto_ vs _goal_position_

To control the motor in the examples above we used the _goto_ method. A lower API to control the motor is to directly use the _goal_position_ property.

Yet, you should be careful when doing so, because the motor will try to reach this new goal position as fast as it can. And it is really fast! You can also use the _moving_speed_ property to set the maximum speed that the motor will reach.

```python
reachy.right_arm.elbow_pitch.moving_speed = 50  # in degrees per sec 
reachy.right_arm_elbow_pitch.goal_position = 110  # in degrees
```

Yet, when using this approach for controlling a motor, it may be hard to follow smoothly complex trajectories. The approach used in the _goto_ method differs. A full position trajectory profile is actually generated going from the current position to the goal position. This trajectory is then interpolated at a predefined frequency (100Hz) to compute all intermediary target position that should be followed before reaching the final goal position.

## Go to specific positions

## Follow trajectories

## Grasping

## Record trajectories

## FK & IK