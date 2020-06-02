---
title: "Check the status of Reachy"
weight: 35
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Check the status of Reachy

## Check the mechanics

Once connected, the next step is to check that everything is ok. The first thing to check is that the robot is ok. You should check that the arm are in a correct position, otherwise they may turn to go back to their base position and unplug a wire.

The base position of the arm looks like this:

TODO

You should also make sure the head is in a base position, meaning looking in front of it.

## Check all connections

The next step is to actually check all connections. This means that we can find all sensors and motors on the robot. The dashboard will help you do that. 

If you go to the dashboard on [http://reachy.local](http://reachy.local), it will automatically tries to connect to each part (the right arm, the head and the left arm) and check if it finds all required modules. After waiting a few seconds, you should see appear a green box for each part present in your robot.

For instance, here we have a Reachy with a head and a right arm fully working:

{{< img alt="Dashboard status ok" src="dashboard-status-ok.png" width="1080px" >}}

If you see an orange or red box, this means it has detected an issue. For instance, below you can see a Reachy with a head and both arms. But there is an issue on the left arm.  

{{< img alt="Dashboard status ok" src="dashboard-status-problem.png" width="1080px" >}}

You can see it says _"module dxl_20 missing"_. This means that it did not manage to find the dynamixel motor 20, so you should check the wire connecting this motor to see if one is unplug (please refer to the technical specifications sections to find out which motor corresponds to which id).

If you encounter such problem, please check the FAQ and if it did not help, please report it to our [forum](https://forum.pollen-robotics.com) so we can help you.