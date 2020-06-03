---
title: "Arm"
weight: 160
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Reachy's arm specifications

## Weight repartition

* Overall Arm: 1670g
* Shoulder: 240g
* Upper arm: 610g
* Forearm: 590g
* Gripper: 230g

**Maximum payload: 500g** 

Of course, this may really vary depending on the holding and duration configuration. 

## Degrees of freedom

**Reachy's arm offers 7 degrees of movement + 1 for the gripper**

{{< img alt="Motors of a Reachy arm" src="reachy-arm-front-motors.png" width="500px" >}}


{{< columns >}}

**Right Arm**

| Motor name | Angle limits | Motor ID |
|:----------:|:------------:|:--------:|
|shoulder_pitch| -180, 90   | 10       |
|shoulder_roll | -10, 180   | 11       |
|arm_yaw       | -90, 90    | 12       |
|elbow_pitch   | -125,  0   | 13       |
|forearm_yaw   | -150, 150  | 14       |
|wrist_pitch   | -50, 50    | 15       |
|wrist_roll    | -45, 45    | 16       |
|gripper       | -69, 20    | 17       |

<--->

**Left Arm**

| Motor name | Angle limits | Motor ID |
|:----------:|:------------:|:--------:|
|shoulder_pitch| -180, 90   | 20       |
|shoulder_roll | -10, 180   | 21       |
|arm_yaw       | -90, 90    | 22       |
|elbow_pitch   | 0,  125    | 23       |
|forearm_yaw   | -150, 150  | 24       |
|wrist_pitch   | -50, 50    | 25       |
|wrist_roll    | -45, 45    | 26       |
|gripper       | -69, 20    | 27       |
{{< /columns >}}



## Standard version

Animated by the following motors: 

* 1 Dynamixel MX-106T
* 3 Dynamixel MX-64AT
* 1 Dynamixel MX-28AT
* 2 Dynamixel AX-18A

## Performance version

Animated by the following motors: 
* 3 Dynamixel MX-106T
* 1 Dynamixel MX-64AT
* 2 Dynamixel MX-28AT
* 1 Dynamixel AX-18A


 
