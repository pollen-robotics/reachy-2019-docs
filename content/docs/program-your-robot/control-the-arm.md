---
title: "Control The Arm"
weight: 220
latex: true
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

You can also define positions, save them and then directly _go to_ those positions.

```python
upper_pos = {
    'head.left_antenna': 0,
    'head.right_antenna': 0,
}
lower_pos = {
    'head.left_antenna': 90,
    'head.right_antenna': -90,
}

for _ in range(3):
    reachy.goto(goal_positions=lower_pos, duration=2, wait=True)
    reachy.goto(goal_positions=upper_pos, duration=1, wait=True)
```

{{< hint info >}}
An easy way to define a position is to actually record it directly on the robot. To do that, you can play with the compliant mode so you can freely move the robot and make it adopt the position you want.
Then, you can record its position using a code similar to:
```python
arm_position = {
    motor.name: motor.present_position for motor in reachy.right_arm.motors
}

reachy_whole_position = {
    motor.name: motor.present_position for motor in reachy.motors
}
```
Make sure only the motors you want to track are actually included in the position you have recorded.
{{< /hint >}}

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

While the motors of the Force Gripper hand can be controlled like the rest of the motors of the arm, there is an additional functionality that lets you easily close and open the gripper.

Opening the gripper is as simple as:

```python
reachy.right_arm.hand.open()
```

This actually simply runs a _goto_ on the gripper motor with pre-defined target position (-30°) and duration (1s).

More interestingly, you can also use the close method:

```python
reachy.right_arm.hand.close()
```

This also runs a _goto_ on the gripper, but it uses the force sensor inside the gripper to detect when it did grab something. It will try to automatically adjust the grip, to maintain enough force to hold the object while not forcing too much and overheat the motor.

All parameters used in the [close](https://pollen-robotics.github.io/reachy/autoapi/reachy/parts/hand/index.html#reachy.parts.hand.ForceGripper.close) method can be adjusted depending on your needs (please refer to the the [APIs](https://pollen-robotics.github.io/reachy/autoapi/reachy/parts/hand/index.html#reachy.parts.hand.ForceGripper.close) for more details.)

{{< hint danger >}}
The gripper on Reachy's end effector is not meant to hold objects for a long time. The motor used in the gripper will quickly overload if doing so. Holding objects as you can see on the TicTacToe demo for instance is a good approximation of what the robot can do for long period of time (hold ~5s rest ~10s).

If you need to hold objects for longer period of time, you probably need to have a specific gripper for this task. While this is part of Pollen Robotics plan, we do not yet have a ready-to-use solution for this kind of use. Let us know if you would be interested in such features or want to help design one.
{{< /hint >}}

The Force Gripper also gives you access to the current _grip_force_. The load sensor is located inside Reachy's gripper. It measures the force applied on the gripper (exactly it measures the deformation of the gripper due to the grasping).

```python
print(reachy.right_arm.hand.grip_force)
>>>TODO
```

{{< hint info >}}
The returned value is nor accurate nor express in a standard unit system. Only relative values should be taken into account.

For instance:
```python
if abs(reachy.right_arm.hand.grip_force) < 50:
    print('not holding')
else:
    print('holding')
```
{{< /hint >}}

<!-- **TODO: photo of load sensor and gripper** -->

## Record & Replay Trajectories

So far, we have showed you how to make your robot move using _goto_ and pre-defined position. This works well for simple motion. But sometimes you would like to have more complex motions. That's when another technique comes into place: **recording by demonstration**.

### Record by demonstration

With this approach, you will demonstrate whole trajectories on Reachy by moving it by hand (using the compliant mode) and record its position at high-frequency (about 100Hz). Depending on what you want, you can record a single motor, or multiple at the same time. We provide a [TrajectoryRecorder](https://pollen-robotics.github.io/reachy/autoapi/reachy/trajectory/recorder/index.html#reachy.trajectory.recorder.TrajectoryRecorder) object that makes this process really simple.

For instance, assuming you want to record a movement on the Right Arm and that it is already compliant:
```python
recorder = TrajectoryRecorder(reachy.right_arm.motors)
```

And when you are ready to record:
```python
recorder.start()
```
And when the movement is over:
```python
recorder.stop()
```

The recorded trajectories can then be accessed via:

```python
print(recorder.trajectories)
>>> TODO
```

You can save it as a numpy array using:
```python
import numpy as np

np.savez('my-recorded-trajectory.npz', **recorder.trajectories)
```

The recorder can be restart as many times as you want. A new trajectory will be recorded on store as ```recorder.trajectories```.

You can record any list of motors. For instance if you want to record the left antenna and the gripper you can use:
```python
recorded_motors = [reachy.head.left_antenna, reachy.right_arm.hand.gripper]

recorder = TrajectoryRecorder(recorded_motors)

recorder.start()
time.sleep(10)
recorder.stop()
```

### Replay a trajectory

Let's say you have recorded a nice motion and save it on the disk using the code above. You can first reload it at any time using:

```python
my_loaded_trajectory = np.load('my-recorded-trajectory.npz')
```

Then, you want to play it on the robot. To do that, we have created an object called [TrajectoryPlayer](https://pollen-robotics.github.io/reachy/autoapi/reachy/trajectory/player/index.html#reachy.trajectory.player.TrajectoryPlayer). It can be used as follow:

```python
trajectory_player = TrajectoryPlayer(reachy, my_loaded_trajectory)
```

First, you need to specify on which robot you want to play the trajectory. If you have multiple Reachy, you can record a movement on one and play it on the other.
Then, you need to specify which trajectory you want to play.

Assuming the motors you want to use to play the trajectory are stiff and that the robot is in position where he can play the trajectory:

```python
trajectory_player.play(wait=True, fade_in_duration=1.0)
```

The code above will play the trajectory, wait for it to finish. 

{{< hint danger >}}
The _fade_in_duration_ ensure there is no jump at the begining of the motion. Indeed, if you play a trajectory from a different starting point that what you record, the robot will try to reach this starting position as fast as it can. This parameter basically says, "first goto the recorded starting position in 1s, then play the trajectory".

Always think about starting position before recording and replaying trajectories!
{{< /hint >}}

You can also play multiple trajectories as a sequence. Assuming you have recorded three motion (traj_1, traj_2 and traj_3):

```python
TrajectoryPlayer(reachy, traj_1).play(wait=True, fade_in_duration=1)
TrajectoryPlayer(reachy, traj_2).play(wait=True, fade_in_duration=1)
TrajectoryPlayer(reachy, traj_3).play(wait=True, fade_in_duration=1)
```

or in a more concise way:

```python
for traj in [traj_1, traj_2, traj_3]:
    TrajectoryPlayer(reachy, traj).play(wait=True, fade_in_duration=1)
```

### Work on a trajectory - Smoothing

Recorded trajectories are actually simple objects. It's a dict in the form: 

```{motor_name: numpy_array_of_position}```

Position arrays have the same length for all motors. You can thus convert the whole trajectory as a 2D array of shape _MxS_ where _M_ is the number of motor and _S_ is the number of sample in the trajectory. The number of sample corresponds roughly to the duration in seconds x 100 (the default sampling frequency).

By recording a trajectory by demonstration, you may sometimes also record involuntary movements or jerkiness. When replayed, you may observe such artefacts. A good practice is to actually apply some smoothing/filtering on the trajectory before saving it.

As they are store as [numpy arrays](https://numpy.org), you can use all typical libraries for this task (numpy/scipy/etc). Depending on the type of movements you recorded or what you want to do (eg. really smooth and slow moves, accurate trajectory, etc), the smoothing you want to apply may vary and there is not a single approach that will work for all types of moves.

We still provide a _cubic smooth_ functionalities as it is something we often use. The [cubic smoothing](https://pollen-robotics.github.io/reachy/autoapi/reachy/trajectory/interpolation/index.html?highlight=cubic#reachy.trajectory.interpolation.cubic_smooth) will garantee to have continuous acceleration which is really important for human perception. You simply need to choose the number of keypoints that will be used to represent your smoothed trajectory.

```python
from reachy.trajectory.interpolation import cubic_smooth

smoothed_trajectories = cubic_smooth(recorder.trajectories, nb_kp=10)
```

{{< hint info >}}
The cubic smooth function will keep the same number of samples by default. But you can modify this behavior using the _out_points_ parameters.
{{< /hint >}}

<!-- **TODO: plot** -->

## Forward & Inverse Kinematics

In all examples above, we have used joint coordinates. This means that we have controlled each joint separately. This can be hard and is often far from what we actually do as humans. When we want to grasp an object in front of us, we think of where we should put our hand, not how to flex each individual muscle to reach this position. This approach relies on the cartesian coordinates: the 3D position and orientation in space.

Forward and Inverse Kinematics is a way to go from one coordinates system to the other:

* forward kinematics: joint coordinates --> cartesian coordinates
* inverse kinematics: cartesian coordinates --> joint coordinates

### Kinematic model

We have defined the whole kinematic model of the arm. On a right arm equipped with a force gripper this actually look like this:

* shoulder_pitch - translation: [0, -0.19, 0] rotation: [0, 1, 0]
* shoulder_roll - translation: [0, 0, 0] rotation: [1, 0, 0]
* arm_yaw - translation: [0, 0, 0] rotation: [0, 0, 1]
* elbow_pitch - translation: [0, 0, -0.307] rotation: [0, 1, 0]
* forearm_yaw - translation: [0, 0, 0] rotation: [0, 0, 1]
* wrist_pitch - translation: [0, 0, -0.224] rotation: [0, 1, 0]
* wrist_roll - translation: [0, 0, -0.032] rotation: [1, 0, 0]
* gripper - translation: [0, -0.185, -0.06] rotation: [0, 0, 0]

<!-- **TODO: schema** -->

This describe the translation and rotation needed to go from the previous motor to the next one. We actually use a simplified Denavit hHrtenberg notation.

<!-- TODO: model origin -->

### Forward kinematics

Using these parameters we can actually compute the 3D position and orientation of any joint in the arm considering their joint angles. This can indeed be formalized as a trigonometry problem.

We provide a function to directly compute the forward kinematics of a reachy arm. Assuming you are working on a Right Arm with a Force Gripper (8 joints), you can find the pose when all motors are at their zero position:


```python
print(reachy.right_arm.forward_kinematics(joints_position=[0, 0, 0, 0, 0, 0, 0, 0]))
>>> TODO
```

{{< hint info >}}
The 4x4 matrix returned by the forward kinematics method is what is often called a pose. It actually encodes both the 3D translation and the 3D rotation into one single representation. 
$$
\begin{bmatrix}
R_{11} & R_{12} & R_{13} & T_x\\\\\\
R_{21} & R_{22} & R_{23} & T_y\\\\\\
R_{31} & R_{32} & R_{33} & T_z\\\\\\
0 & 0 & 0 & 1
\end{bmatrix}
$$

{{< /hint >}}

You can also get the current pose by doing:
```python
current_position = [m.present_position for m in reachy.right_arm.motors]
print(reachy.right_arm.forward_kinematics(joints_position=current_position))
>>> TODO
```

{{< hint warning >}}
If you are using an external 3D tracker, you may observe difference between the measure end effector position and the one given by the forward kinematics.

Keep in mind that the forward kinematics rely on a theoretical model of the Reachy arm. You may have difference due to motor jerk or assembly approximation.
{{< /hint >}}

### Inverse kinematics

Knowing where you arm is located in the 3D space can be useful but most of the time what you want is to move the arm in cartesian coordinates. You want to have the possibility to say: "move your hand to [x, y, z] with a 90° rotation around Z axis".

This is what inverse kinematics does. We have provided a method to help you. You need to give it a 4x4 target pose (as defined in the forward kinematics) and an initial joint state. 

```python
TODO: cool example
```

Yet, it's important to understand that while the forward kinematics has a unique and well defined solution, the inverse kinematics is a much harder and ill defined problem. 

First, a Right Arm with a Gripper has 8 Degrees Of Freedom (8 motors) meaning that you may have multiple solutions to reach the same 3D point in space.

Second, at the moment, the inverse kinematics is computed using an approximation method that may take time to converge. You also need to give it a starting point that may have a tremendous influence on the final result.


{{< hint warning >}}
Multiple approaches exist for tackling the inverse kinematics problem. We have provided the most straightforward one, using black box optimization, that work in a general context. Yet, we intend to provide other ones more dedicated to specific context: 

* having more natural movement
* minimizing change when performing whole cartesian trajectory
* computation speed
* ...
{{< /hint >}}