---
title: "Pick and Place"
weight: 250
---

# Pick and Place

## Prepare Reachy workspace

With all motors compliant try if you can realise the task you would like the robot to do.
We will realise most of the recordings using physical demonstration.

### Choose a good starting point

### Define a resting position

Rest between motions.
Real rest position (even turn the motor compliant if possible).

{{< hint danger >}}
The rest position should not force! Static forcing is how the motors heat the fastest.
{{< /hint>}}

## Record a trajectory

```python
from reachy.trajectory import TrajectoryRecorder

traj_recorder = TrajectoryRecorder(reachy.right_arm.motors)
```

{{< hint info >}}
Compensating gravity
{{< /hint>}}


### Record the _pick_ trajectory

```python
traj_recorder.start()
```

```python
traj_recorder.stop()
```

```python
import numpy as np

pick_traj = traj_recorder.trajectories
np.savez('pick-traj.npz', **pick_traj)
```

### Record the _place_ trajectory

```python
traj_recorder.start()
```

```python
traj_recorder.stop()
```

```python
import numpy as np

place_traj = traj_recorder.trajectories
np.savez('place-traj.npz', **place_traj)
```

### Record the _back_ trajectory

```python
traj_recorder.start()
```

```python
traj_recorder.stop()
```

```python
import numpy as np

back_traj = traj_recorder.trajectories
np.savez('back-traj.npz', **back_traj)
```

## Putting everything together

```python
from reachy.trajectory import TrajectoryPlayer

goto_base_position(duration=2)

TrajectoryPlayer(reachy, pick_traj).play(wait=True)

reachy.right_arm.hand.close()

TrajectoryPlayer(reachy, place_traj).play(wait=True)

reachy.right_arm.hand.open()

TrajectoryPlayer(reachy, back_traj).play(wait=True)

goto_resting_position(duration=2)
```

### Open & close the gripper

### Look what you are doing

### Make it loop


