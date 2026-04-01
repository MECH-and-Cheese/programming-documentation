## Drivetrain
A poorly tuned drivetrain ruins every other subsystem. 

### Basic Tuning (Elevated)
1. **Find kV:** * Start with `kV = 1.0` (wheels in the air).
    * Increase by `0.5` until the velocity stops increasing.
2. **Find P:**
    * Increase until the wheels **oscillate** when stopping.
    * Decrease in small increments until the oscillation disappears.

### Characterizing kS (On Carpet)
1. Place the robot on the floor.
2. Using **REV Hardware Client**, increase Duty Cycle until the robot barely starts moving.
3. **The Formula:** $kS = \text{Duty Cycle} \times 12$