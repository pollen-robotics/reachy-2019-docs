---
weight: 510
title: "Setup The Demo"
---

# Setup the demo

On the ISO provided with the robot, the code to run the TicTacToe playground is already pre-installed. From your Raspberry-Pi board, you can find in the folder ```~/dev/reachy-tictactoe```.

Inside, you will find:
* the code implementing the gameplay mechanisms,
* the specific assets: move trajectories, pre-trained vision network
* a service file to simplify launching

## Launch the demo at boot

The easiest way to setup the demo is to launch it automatically at startup. Once setup, all you have to do is turn on the robot and after about 30s (basically the Raspberry-Pi boot time), it will directly start playing.

{{< hint danger >}}
Make sure you put the robot in a "safe" position before turning it on. Indeed, as soon as it starts it will try to move to its rest position.

**TODO: image**
{{< /hint >}}

We provide a pre-configured service file. So, to make it run automatically at startup, you need to:
1. connect to your Raspberry-Pi
2. run the following command: 
    - ```sudo systemctl enable tictactoe_launcher.service```
3. restart your Raspberry-Pi
4. wait for about 30s and you should see the behavior starting

{{< hint info >}}
You only need to do this operation one time. Then, the demo will always start when the robot is turned on. 

* If you want to stop the demo, run:
    * ```sudo systemctl stop tictactoe_launcher.service```
* If you want to disable the auto-launch behavior, run:
    *  ```sudo systemctl disable tictactoe_launcher.service```
{{< /hint >}}
