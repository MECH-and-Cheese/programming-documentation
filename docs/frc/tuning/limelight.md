# Limelight Tuning & Management

The Limelight is a powerful vision processor, but it requires careful thermal and software management to remain reliable during a match.

### Tracking Modes
* **3D Tracking (AprilTags):** Used for field-relative odometry and precise alignment.
* **2D Tracking:** Best for high-speed reflective tape tracking or basic turret alignment.
* **AI/Neural Detector:** Specialized for detecting game pieces on the floor.

### Thermal Management
Limelights process data on-board and generate significant heat, leading to thermal throttling or crashes.
* **Software:** Limit LED usage and processing power in code when the robot is not in a match or active state.
* **Hardware:** It is highly recommended to install a **heatsink** to help dissipate heat.

!!! warning "Fail-Safe Programming"
    Never rely solely on the Limelight. Always program **backup commands** for the driver or fallback sensor logic in case the Limelight fails due to heat, power draw, or wiring issues.