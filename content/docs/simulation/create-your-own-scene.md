---
weight: 710
title: "Create your own virtual scene for Reachy"
---

## Create your own virtual scene for Reachy
We've created a simulation of our reachy robot where we simulate its behaviour and motors physics.

* It lets you try to move the simulated robot without any risk of breaking anything. It’s a good way to learn how to control Reachy without fear.
* You can try yourself controlling a simulated Reachy before purchasing one, to better understand what it can and cannot do.
* It can easily be instanciate as you many time as you want for instance to train your machine learning algorithm.
* You can create your own animations and prepare unic behaviours.
* It can interact with physical objects in 3D scenes.

{{< img alt="reachy waiting" src="simu/reachy_standing.PNG" width="600px" >}}

## What you need
Unity, editor version 2020.1.0b7 or higher. ([Get it here](https://unity3d.com/fr/beta/2020.1b))

## Import reachy package
You can get the package asset of the reachy simulation on our GitHub deposit : "https://github.com/pollen-robotics/reachy-unity-package".
In the '???' folder you will find our unity package. To use it, simply open your project and go to 'Assets' --> Import package --> custom package and select our package.

{{< img alt="How to import reachy" src="simu/import_steps.gif" width="600px" >}}

## What's inside ?
Our package is composed of 2 prefabs and 3 scripts. (plus the meshes and materials used in the prefabs, and a webSocket plugin).
The reachy prefab is our robot and the support prefab is simply the support that keeps the robot upright.
In the scripts folder, the ReachyController contains the function you will use to play with reachy.
A JointController script is associated to each part of reachy.
The WsIO script allows you to communicate by webSocket so you can send command (i-e move reachy) from an other application or get the current reachy state (i-e reachy's view or motors position).

## Play with reachy
Reachy is composed of static parts, such as the torso, the left and the right shoulder cap, and of mobile parts such as the head (only the antennas can move for the moment) and the arms.

The variables in charge of the movements of the arms are the 'targetPosition'. Each parts (also called motors) of an arm may rotate around one axis and the targetPosition defines the angle its supposed to go to.
The reachy GameObject has access to each of theses motors and specially to their target position.

# Reachy commands
The ReachyCommand script allows you to make reachy move and get his current state at any time with the following functions :
* HandleCommand(SerializableCommands command) : Giving a a list of motors names and their target position, it will update the robot movements.
* GetCurrentState() : Get the current state of reachy (composed of the cameras view, and the current  position of every motors).

Each part of an arm is linked to a parent so moving one part will make the children move with it.

# Handle collisions
Each motor use the [articulation body](https://docs.unity3d.com/ScriptReference/Rigidbody.html) and [box colider](https://docs.unity3d.com/ScriptReference/Collider.html) components, they allow reachy to interact with other physical objects that contain a rigidBody and a collider. According to the Unity Docmentation, "a Rigidbody object will be pulled downward by gravity and will react to collisions with incoming objects if the right Collider component is also present". For instance, you can make reachy flip a table by adding a rigidiBody and a collider to a table GameObject and ask reachy to quickly move his arm bottom-up (when close enough to the table).

{{< img alt="reachy flipping" src="simu/flippingTable.gif" width="600px" >}}

# Articulation Body
Each motor has the component [articulation body](https://docs.unity3d.com/2020.1/Documentation/ScriptReference/ArticulationBody.html) which represents the characteristics of how a specific part of the arm is supposed to move. Let's focus on some of the 'X drive' part that we use: 
* The Lower limit and Upper limit simply define a minimum and maximum angle of rotation.
* The stiffness globally defines the speed.
* The damping is similar to a break.
Both combined define the force used to move the part.
* The target represent the target position for a part and cannot be set directly.
The Force is computed with the following equation : 
    F = stiffness * (currentPosition - target) - damping * (currentVelocity - targetVelocity)