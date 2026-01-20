# Limelight Vision System

# Limelight Vision System

Welcome to the team's documentation for the Limelight Vision System. This module covers how our robot perceives the world, tracks AprilTags, and automates alignment for shooting and movement.

!!! abstract "Goal of this Module"
    By the end of this section, you will be able to:
    
    1. Understand the difference between **2D alignment** and **3D positioning**.
    2. Write code to make a chassis or turret lock onto a target.
    3. Troubleshoot common issues like "the infinite spin" or "**stiction**{: data-tooltip="Static Friction -> Stiction. The resistance or 'stickiness' that must be overcome to start moving two surfaces that are stationary against each other." }."

---

## 📹 Video Introduction

!!! tip "Watch: System Overview (30s)"
    <iframe width="100%" height="400" src="https://www.youtube.com/embed/YOUR_VIDEO_ID" title="Limelight Overview" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
    
    *In this video, we demonstrate the robot identifying an AprilTag on the Speaker and automatically aligning the turret.*

---

## 🛠️ Hardware Prerequisites

Before writing any code, ensure the hardware is ready. We currently use the **Limelight 4**.

1.  **Mounting:** The camera must be mounted firmly. If it wobbles, our code will oscillate.
2.  **Ethernet:** Static IP should be set to `10.TE.AM.11` with any zeros being excluded (e.g., `10.51.3.11`).
3.  **Pipeline Settings:**
    * **Pipeline 0:** Standard AprilTag 2D (Used for Chassis).
    * **Pipeline 1:** Neural Network (Used for Game Piece Detection).
    * **Pipeline 2:** Standard AprilTag 3D (Used for Chassis).
    * **Pipeline 3:** High-Exposure AprilTag (Used for Turret).

!!! warning "Crucial Step: Calibration"
    If you are using **3D features** (BotPose), you **must** perform the ChAruco calibration and upload the correct field map `.fmap` file via the [Limelight Interface](http://limelight.local:5801). 
    
    Without this, `tx` works, but distance calculations will be wrong.

---

## 📚 Documentation Tree

Select a topic below to get started:

<div class="grid cards" markdown>

-   **[Core Concepts](concepts/coordinates.md)**
    
    Learn the math behind `tx`, `ty`, and PID control loops. Start here if you are new to control theory.

-   **[Chassis Alignment](implementations/chassis-align-2d.md)**
    
    How to make the drivetrain auto-align to a target and stop at the perfect shooting distance.

-   **[Turret Tracking](implementations/turret-tracking.md)**
    
    Specialized code for the `TurretAlignCommand`, managing specific challenges like stiction and safety stops.

-   **[Troubleshooting](reference/troubleshooting.md)**
    
    Robot spinning in circles? Turret humming but not moving? Check the solutions here.

</div>