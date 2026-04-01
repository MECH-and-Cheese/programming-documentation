# Drivetrain Tuning

The drivetrain is the foundation of the robot. A poorly tuned drivetrain will negatively affect odometry, autonomous accuracy, and Limelight calculations.

### Mechanical Pre-Check
Before tuning, ensure the following:
* Gears are properly greased.
* Swerve modules/gearboxes are free of debris ("gunk").
* No mechanical binding is present.

### Basic Tuning (Elevated)
1. **Preparation:** Place the robot on blocks so wheels spin freely. Set all Feedforward (SVA) and PID values to $0$.
2. **Find kV:** * Start with $kV = 1.0$.
    * Drive at max speed and monitor motor velocity via Shuffleboard.
    * Increase $kV$ in increments of $0.5$ (or $0.1$ for fine-tuning) until the velocity stops increasing.
    * Lower the value to the first point where that max speed was achieved.
3. **Find P:**
    * Start with a small value (e.g., $1.0$).
    * Increase until the wheels **oscillate** when returning to a zero setpoint.
    * Decrease by very small increments until oscillation stops.

### Characterizing kS (On Carpet)
To find the voltage needed to break static friction:
1. Place the robot on the floor.
2. Using the **REV Hardware Client**, spin all drive motors in Duty Cycle mode.
3. Slowly increase the value until the robot achieves slow, consistent movement.
4. **Calculate:** Multiply that Duty Cycle value by $12$ to find your $kS$ voltage.

$$kS = \text{Duty Cycle} \times 12$$