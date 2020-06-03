---
weight: 200
title: "Program your robot"
---

# Program your robot

This section will guide you in how to control your robot. It will describe the most basic features needed to make your robot move and interact with its environment.

The robot API is written in [Python](https://www.python.org) and versions above 3.6 are supported. It is pre-installed on the Raspberry-Pi of the robot. So **to program your Reachy you don't need to install anything on your own machine**. Just connect to the Raspberry Pi as described in the [Getting Started](../getting-started/connect-to-your-robot/).

{{< hint info >}}
Working remotely on the robot is possible via ssh or using a [Jupyter server](https://jupyter.org) (Jupyter is pre-installed on the Raspberry Pi).
{{< /hint >}}


The SDK has been designed to be accessible and easy-to-use. Yet, as it is fully open-source (available [here](https://github.com/pollen-robotics/reachy)), you can dig inside and adapt it to your specifics needs ([contributions are welcome!](https://github.com/pollen-robotics/reachy/blob/master/CONTRIBUTING.md)). 

In this section, we will cover:
* how to instantiate your robot and define the parts you are using,
* make the arm moves (motor by motor or using kinematics), recording and replay motions,
* make the head look somewhere specific,
* and run pre-defined behaviors.

The full Python's API is also accessible [here](https://pollen-robotics.github.io/reachy/).

{{< hint danger >}}
Before actually running any code, we strongly recommend you to take a look at the [Safety first section](../posts/safety). It provides simple guidelines to avoid damaging your robot.
{{< /hint >}}

More advanced topics (like using advanced vision for the [TicTacToe demo](../tictactoe)) will be discussed in their own chapter.

