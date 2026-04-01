# Autonomous Tuning

Autonomous routines require a high degree of precision. Consistency between software constants and physical reality is the most frequent point of failure.

### Control Methods
* **PathPlanner:** The standard for high-level path following and trajectory generation.
* **Manual Paths:** Trajectories created manually via code for specific, rigid movements.

!!! tip "Odometry Assistance"
    Never rely solely on encoders for long paths. It is highly recommended to use a **Limelight** to provide global pose estimates to assist your odometry and prevent drift.

### Configuration Matching
You must ensure that your **PathPlanner constants** and your **Robot Code constants** match exactly.

!!! danger "Synchronization Error"
    If the max speed in PathPlanner is set to $4.0 \text{ m/s}$, but your robot code autonomous configuration is capped at $3.0 \text{ m/s}$, the robot will experience persistent lag error that PathPlanner cannot correct.

### Testing Workflow
1. **Drivetrain Health:** Verify drivetrain tuning before testing autonomous.
2. **Iterative Testing:** Do not trust a single "lucky" run. Perform 5–10 consecutive tests to check for variance.
3. **Command Logic:** If the robot isn't reaching its goal, determine if it's a mechanical grip issue or a command scheduling issue within PathPlanner.