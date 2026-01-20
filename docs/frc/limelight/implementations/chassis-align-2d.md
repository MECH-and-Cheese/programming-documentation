# 2D Chassis Alignment

The most common use for the Limelight is pointing the drivetrain at a target. This uses a simple **Proportional Controller (P-Loop)**.

---

## The Math

We want the robot to rotate until the target is centered (`tx = 0`).

* **If `tx` is positive (+):** Target is to the Right -> **Turn Right**.
* **If `tx` is negative (-):** Target is to the Left -> **Turn Left**.

The speed is determined by the formula:

$$
\text{Rotation} = \text{error} \times kP
$$

---

## Java Implementation

Copy this command into your robot project. It drives the robot based on vision error while the button is held.

```java
package frc.robot.commands;

import edu.wpi.first.math.controller.PIDController;
import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.DriveSubsystem;
import frc.robot.subsystems.VisionSubsystem;

public class LimelightAlign extends Command {
    private final DriveSubsystem m_drive;
    private final VisionSubsystem m_vision;

    // Tuning Constants
    // kP: Start small (0.01) and increase until it oscillates
    private final double kP = 0.035; 
    private final double kI = 0.0;
    private final double kD = 0.0;

    private final PIDController m_controller = new PIDController(kP, kI, kD);

    public LimelightAlign(DriveSubsystem drive, VisionSubsystem vision) {
        m_drive = drive;
        m_vision = vision;
        addRequirements(m_drive);
    }

    @Override
    public void initialize() {
        m_controller.reset();
        m_controller.setSetpoint(0.0); // Target is center of screen
        m_controller.setTolerance(1.0); // Allow 1 degree of error
    }

    @Override
    public void execute() {
        if (m_vision.hasTarget()) {
            double rotationOutput = m_controller.calculate(m_vision.getTX());

            // Clamp speed (Max 50%) to prevent tipping
            // Math.max/min ensures we don't go above 0.5 or below -0.5
            rotationOutput = Math.max(-0.5, Math.min(0.5, rotationOutput));

            // Drive method: xSpeed, ySpeed, Rotation, FieldRelative
            m_drive.drive(0, 0, rotationOutput, false);
        } else {
            m_drive.stop();
        }
    }

    @Override
    public boolean isFinished() {
        return false; // Run until button is released
    }
}
```