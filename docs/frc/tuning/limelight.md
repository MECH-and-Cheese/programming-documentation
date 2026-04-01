## Limelight

### Tracking Modes
* **3D Tracking (AprilTags):** Essential for field-relative odometry.
* **2D Tracking:** Best for high-speed tasks like shooter/turret alignment.

### Thermal Management
The Limelight processes all data on-board and **overheats very easily**.
* **Software:** Limit LED usage when not in a match.
* **Hardware:** Install a **heatsink** to maintain performance.

!!! warning "Reliability"
    Never rely only on Limelight. Always have **backup commands** for the driver to use if the camera fails due to heat or power issues.