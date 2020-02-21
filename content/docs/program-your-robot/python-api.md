---
title: "Python's API"
weight: 290
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# Python's API

The whole Python's API is available here: [https://pollen-robotics.github.io/reachy/](https://pollen-robotics.github.io/reachy/).

The inline documentation can also be accessed directly from Python using instrospection. For instance:

```python
In [1]: from reachy import Reachy                                                                                                                             
In [2]: print(Reachy.__doc__)                                                                                                                                 
Class representing the connection with the hardware robot.

Connect and synchronize with the hardware robot.

Args:
    left_arm (reachy.parts.LeftArm): left arm part if present or None if absent
    right_arm (reachy.parts.RightArm): right arm part if present or None if absent
    head (reachy.parts.Head): hrad part if present or None if absent

It can be used to monitor real time robot state and to send commands.
Mainly a container to hold the different parts of Reachy together.
    
In [3]: help(Reachy)      
```
