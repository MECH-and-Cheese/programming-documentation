# Advanced 3D Tracking (AprilTags)

Limelight 3 and newer software allow for full field localization. Instead of just aiming at a target, the robot knows exactly where it is on the field (X, Y, Z).



---

## What is BotPose?

The Limelight returns a generic array called `BotPose`. It contains 6 numbers:

* **X, Y, Z:** Position in meters from the field origin (Blue Alliance Wall corner).
* **Roll, Pitch, Yaw:** Rotation of the robot.

### Blue vs. Red Alliance Origin

!!! warning "Coordinate System Chaos"
    The field coordinate system changes depending on your alliance!
    
    * **Blue Alliance:** (0,0) is the right corner of the Blue Driver Station.
    * **Red Alliance:** (0,0) is the right corner of the *Blue* Driver Station (it stays fixed!).
    
    Always use the `BotPose_wpiblue` output to ensure your math stays consistent regardless of which side you are playing on.

---

## MegaTag Implementation

"MegaTag" is Limelight's algorithm for combining data from multiple tags to reduce jitter.

### Updating the Odometry (Swerve)

If you are using Swerve Drive, you can inject vision data into your odometry to correct for wheel slip.

```java
// Inside your DriveSubsystem.java

public void updateOdometry() {
    // 1. Update standard wheel odometry
    m_poseEstimator.update(getGyroRotation(), getModulePositions());

    // 2. Add Vision Measurement if available
    if (LimelightHelpers.getTV("limelight")) {
        // Get the pose from the specific "Blue" array
        Pose2d visionPose = LimelightHelpers.getBotPose2d_wpiBlue("limelight");
        
        // Get the timestamp (latency compensation)
        double timestamp = Timer.getFPGATimestamp() - (LimelightHelpers.getLatency_Pipeline("limelight") / 1000.0);

        // Reject bad data (ambiguity check)
        // Only trust vision if the tag is close and clear
        m_poseEstimator.addVisionMeasurement(visionPose, timestamp);
    }
}
```

!!! tip 
    "Trust Standard Deviations" Vision is noisy. You can tell the PoseEstimator to trust Vision less than the Encoders by increasing the standard deviation values in the PoseEstimator constructor.