# Coordinate Systems & Math

Before we can write code to align the robot, we must understand **what** the camera is actually telling us. The Limelight does not natively see "meters" or "inches"; it sees **angles**.

---

## 1. The 2D Coordinate System (`tx` & `ty`)


When the Limelight is in "Standard" or "Retro-reflective" mode, it treats the world like a flat image. The center of the camera lens is `(0,0)`.

* **`tx` (Target X):** The horizontal angle from the center of the camera to the target.
    * **Range:** -29.8° to +29.8° (roughly).
    * **Use Case:** This is your **Rotation Error**. If `tx` is positive, the target is to the right. We spin the robot right to make `tx` zero.
* **`ty` (Target Y):** The vertical angle from the center of the camera to the target.
    * **Range:** -24.85° to +24.85°.
    * **Use Case:** This is used to calculate **Distance**. As you get closer to the target, the target moves "up" in the image (higher `ty`).

!!! note "The Crosshair"
    If your **`tx`** is 0 and **`ty`** is 0, the target is perfectly dead-center in the camera lens.

---

## 2. Calculating Distance (Trigonometry)

Since `ty` is an angle, we can use basic trigonometry to turn it into a distance measurement (inches or meters). This is essential for determining how hard to shoot a note or where to stop the robot.

The formula we use is:

$$
d = \frac{h_{target} - h_{camera}}{\tan(a_{mount} + a_{target})}
$$

Where:
* $d$ = Distance to the target (floor distance).
* $h_{target}$ = Height of the center of the AprilTag (from field manual).
* $h_{camera}$ = Height of your Limelight lens from the floor.
* $a_{mount}$ = The **mounting angle** of your Limelight (how far back it is tilted).
* $a_{target}$ = The `ty` value from the camera.

!!! warning "Math Trap: Radians vs Degrees"
    Java's `Math.tan()` function expects **radians**, but the Limelight gives us **degrees**.
    
    You must convert them: `Math.toRadians(limelightMountAngle + ty)`

---

## 3. The 3D Coordinate System (BotPose)


Newer Limelight updates allow for **3D Tracking**. Instead of just giving us angles, the camera looks at the distortion of the AprilTag to calculate exactly where the robot is on the field.

This is returned as a **BotPose** array: `[x, y, z, roll, pitch, yaw]`.

### The Axes (Blue Alliance Origin)
In FRC, the "Origin" `(0,0,0)` is usually the corner of the Blue Alliance wall.

* **X (Forward/Back):** Movement down the long length of the field.
* **Y (Left/Right):** Movement across the short width of the field.
* **Z (Up/Down):** Height off the carpet (usually 0 for a robot on the ground).

### The Orientation (Rotation)
* **Yaw:** Rotation around the vertical axis (Turning left/right). This corresponds to your **Gyro** heading.
* **Pitch:** Tipping forward/backward (like a seesaw).
* **Roll:** Tipping side-to-side.

### Practical Application & Terminology
* **FOV (Field of View):** The Limelight 3 has a roughly 60° horizontal **FOV**{: data-tooltip="Field of View: The total observable area the camera can see. If a target leaves the FOV, the robot is blind." }. If the target leaves this cone, you cannot track it.
* **Pipelines:** We switch **pipelines**{: data-tooltip="A saved profile of settings (exposure, brightness, color thresholds) on the camera." } to look for different things (e.g., Pipeline 0 for AprilTags, Pipeline 1 for Notes).
* **Reliability:** 3D tracking is powerful, but it has more **latency**{: data-tooltip="The delay between an event happening and the system noticing it." } than 2D tracking.
* **Filtering:** We usually check the **ambiguity**{: data-tooltip="Uncertainty in data. High ambiguity means the camera isn't sure which way the tag is facing." } metric. If ambiguity is > 0.2, the reading is likely "jumping" or incorrect, so we ignore it to prevent the robot from teleporting on the map.