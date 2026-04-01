## General Tuning (SVA & PID)

### Feedforward (SVA)
Tune the mechanism while it is free-spinning. Do not tune with game pieces inside.

| Constant | Name | Function |
| :--- | :--- | :--- |
| **kS** | Static | Voltage to overcome friction. |
| **kV** | Velocity | Voltage to maintain target speed. |

### PID Control
PID corrects for real-world errors like game piece weight.

!!! info "Proportional (P) Tuning"
    Start small and increase until you see **oscillation** (the motor "hunts" for the target). Lower the value to the first point where oscillation stops.