## Autonomous
Autonomous routines require a combination of pathing logic and hardware precision.

### Control Methods
* **PathPlanner:** The industry standard for high-level path following.
* **Manual Path Creation:** Building trajectories manually via code.

!!! tip "Odometry Assistance"
    It is highly recommended to use a **Limelight** to assist odometry to prevent drift over time.

### The Golden Rule of Tuning
Consistency is key. You must ensure that your **PathPlanner constants** and your **Robot Code constants** match exactly.

!!! danger "Common Error"
    If PathPlanner is set to a max speed of 4 m/s, but your robot code autonomous config is capped at 3 m/s, PathPlanner will never be able to correct the resulting error.