# General Tuning (PID & SVA)

Proper tuning follows a specific order: **SVA (Feedforward) tuning must be completed before PID tuning.**

### Feedforward (SVA)
Feedforward tells the mechanism how much voltage is required to reach a target speed in an ideal environment. 

| Constant | Name | Definition |
| :--- | :--- | :--- |
| **kS** | Static | Voltage to overcome "stiction" or initial friction. |
| **kV** | Velocity | Voltage to maintain a constant commanded velocity. |
| **kA** | Acceleration | Voltage to reach a target velocity quickly. |

!!! note
    Tune your mechanism while it is free-spinning. Do not tune with game pieces or external loads unless characterizing for those specific conditions.

### PID Control
PID corrects for real-world error caused by unexpected friction, battery sag, or game piece weight.

* **P (Proportional):** The primary correction. Increase until the system **oscillates** (overshoots and undershoots the target), then back it off until the movement is smooth.
* **I (Integral):** Used to correct steady-state error (not recommended for beginners).
* **D (Derivative):** Used to dampen the approach to the target (not recommended for beginners).

!!! warning "Avoid Oscillation"
    Under no circumstances should you leave a mechanism with visible oscillation. This leads to motor wear, excessive power draw, and command inaccuracies.