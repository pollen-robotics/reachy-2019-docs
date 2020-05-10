---
weight: 710
title: "Create your own virtual scene for Reachy"
---

## Create your own virtual scene for Reachy

We've created a simulated version of our Reachy robot where we simulate its behaviour and motors physics. It comes as a Unity package with everything already setup.

This lets you create your own 3D Scene within Unity so you can **build your own virtual experimental setup**. You can **add 3D objects with their physics** (a table, objects that can be grasped and moved, etc). You can setup lighting conditions and retrieve what the robot will perceive from its camera. You can also detect collisions, so you **can grasp objects**, or **train the robot to perform some specific moves**. You can even add humans avatar to prototype Human-Robot Interactions.

{{< img alt="reachy waiting" src="simu/reachy_standing.PNG" width="600px" >}}


## What you need

Unity, editor version 2020.1.0b7 or higher. ([Get it here](https://unity3d.com/fr/beta/2020.1b))
We rely on the new _Articulation Body_ component made for robotics. So it will not work on previous version.

Also, make sure to setup a few parameters in your Scene to enable better robotics physics simulation **TODO: ask Sarah for a link**.

## Import reachy package
You can get the package asset of the reachy simulation on our GitHub deposit: [https://github.com/pollen-robotics/reachy-unity-package](https://github.com/pollen-robotics/reachy-unity-package).

The Unity package can be found directly in the release:  [https://github.com/pollen-robotics/reachy-unity-package/releases/latest](https://github.com/pollen-robotics/reachy-unity-package/releases/latest). 

To use it, simply open your project and go to 'Assets' --> Import package --> custom package and select our package.

{{< img alt="How to import reachy" src="simu/import_steps.gif" width="600px" >}}

## What's inside?

Our package is mainly composed of 2 prefabs composed of the meshes and materials for Reachy, plus a few scripts to control it.

The Reachy prefab is our robot and the support prefab is simply the support that keeps the robot upright.
In the scripts folder, the ReachyController contains the function you will use to control Reachy. A JointController script is associated to each part of Reachy. This script lets you drive the motor as you would with the real robot. Gripper also have a _force sensor_ script that retrieves the current force applied on the gripper.

We also provide IO (only a WebSocket for the moment, but other can be easily implemented) scripts which allow you to communicate with the external world. This can be used to control the virtual robot using the same Python's API that we use for the real robot (see section [Simulation](../)). This IO let you send command (i-e move Reachy) from an other application or get the current reachy state (i-e reachy's camera image, motors position, gripper force).

## Play with reachy

Reachy is composed of static parts, such as the torso, the left and the right shoulder cap, and of mobile parts such as the head (only the antennas can move for the moment) and the arms.

You can directly make those parts move by setting the variables _targetPosition_ in the motors property of the _ReachyController_ component attached to the Reachy Prefab. **TODO: add gif** Each part of an arm is linked to a parent so moving one part will make the children move with it.

{{< hint info >}}
As, we use the Unity Physics engine, make sure you are in _Play Mode_ before trying to move the robot.
{{< /hint >}}


# Reachy commands

The _ReachyCommand_ script provides high level function to make reachy move and get his current state:
* HandleCommand(SerializableCommands command): Giving a list of motors names and their target position, it will update the robot movements.
* GetCurrentState(): Get the current state of reachy (composed of the cameras view, and the current  position of every motors).

Those functions are for instance called by the IO when you are remotely controlling the robot.


# Handle collisions

Each motor use the [Articulation Body](https://docs.unity3d.com/2020.1/Documentation/ScriptReference/ArticulationBody.html) and [colider](https://docs.unity3d.com/ScriptReference/Collider.html) components, they allow Reachy to interact with other physical objects.
According to the Unity Docmentation, "a Rigidbody object will be pulled downward by gravity and will react to collisions with incoming objects if the right Collider component is also present". 
If you want to add other objects to your Scene and have Reachy interact with them, you need to make sure to attach those two components to them. For instance, you can make reachy flip a table by adding a rigidiBody and a collider to a table GameObject and ask reachy to quickly move his arm bottom-up (when close enough to the table).

{{< img alt="reachy flipping" src="simu/flippingTable.gif" width="600px" >}}

# Articulation Body

Each motor has the component [Articulation Body](https://docs.unity3d.com/2020.1/Documentation/ScriptReference/ArticulationBody.html) which represents the characteristics of how a specific part of the arm is supposed to move. All motors in Reachy are revolute joints, and they are controlled using the [Articulation Drive](https://docs.unity3d.com/2020.1/Documentation/ScriptReference/ArticulationDrive.html) mechanism.

We've preset a few parameters to try to match closely the real behavior of Reachy's motor
* The Lower limit and Upper limit define the same minimum and maximum angle of rotation as the real robot.
* The stiffness and damping are PD gains of a PID controller. 
* The force limit is the maximum force the joint can apply.

Those parameters were defined by recording moves on the real robot, measure the real trajectory, the backlash and try to reproduce them on the simulation. Yet, depending on your experimental conditions you may need to tune them to get better results.