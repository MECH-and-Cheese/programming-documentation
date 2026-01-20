# Turret Tracking

Turrets are harder than drivetrains because they have **Hard Stops**. If you drive a turret past its limit, you will break wires or strip gears. We must use code to prevent this.

---

## Safety Strategy

We combine a standard PID loop with **Soft Limits**:

1.  **Calculate Speed:** Use PID based on `tx`.
2.  **Check Limits:** * If moving **Left** AND at **Left Limit** -> **STOP**.
    * If moving **Right** AND at **Right Limit** -> **STOP**.
3.  **Apply Power:** Only move if safe.

---

## Java Implementation

This command assumes you have a `TurretSubsystem` that can report its current angle in degrees.

```java
package frc.robot.commands;

import edu.wpi.first.math.controller.PIDController;
import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.TurretSubsystem;
import frc.robot.subsystems.VisionSubsystem;

public class TurretAlign extends Command {
    private final TurretSubsystem m_turret;
    private final VisionSubsystem m_vision;
    
    // Tuning
    private final double kP = 0.02; 
    private final double kStatic = 0.04; // Feedforward to break stiction
    
    // Safety Limits (Degrees)
    private final double LEFT_LIMIT = -180.0;
    private final double RIGHT_LIMIT = 180.0;

    private final PIDController m_controller = new PIDController(kP, 0, 0);

    public TurretAlign(TurretSubsystem turret, VisionSubsystem vision) {
        m_turret = turret;
        m_vision = vision;
        addRequirements(m_turret);
    }

    @Override
    public void execute() {
        double speed = 0;

        if (m_vision.hasTarget()) {
            // 1. Calculate PID
            speed = m_controller.calculate(m_vision.getTX(), 0);

            // 2. Add Feedforward (Stiction)
            // If speed is tiny, the turret won't move. Add a "kick".
            if (Math.abs(speed) > 0.01) {
                speed += Math.copySign(kStatic, speed);
            }
        }

        // 3. Safety Checks (Soft Limits)
        double currentAngle = m_turret.getAngle();

        // If trying to turn Left (< 0) BUT we are past the Left Limit
        if (speed < 0 && currentAngle < LEFT_LIMIT) {
            speed = 0; 
        }
        // If trying to turn Right (> 0) BUT we are past the Right Limit
        else if (speed > 0 && currentAngle > RIGHT_LIMIT) {
            speed = 0; 
        }

        m_turret.setSpeed(speed);
    }
}
```