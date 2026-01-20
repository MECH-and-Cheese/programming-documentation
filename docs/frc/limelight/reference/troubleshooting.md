# Troubleshooting & FAQ

When the robot misbehaves, use this guide to diagnose the issue. Do not change code until you have verified the hardware and pipeline settings!

---

## 🛑 Common Symptoms

| Symptom | Probable Cause | The Fix |
| :--- | :--- | :--- |
| **No Target Found** (TV is 0) | Exposure too low or wrong Pipeline. | Check the Limelight Interface (`10.TE.AM.11:5801`). Switch to the correct pipeline. |
| **"The Infinite Spin"** | Inverted Motors or Sensor Phase. | If the robot spins *away* from the target, invert the motor output in code (`-speed`). |
| **Jitter / Shaking** | **kP** is too high. | Decrease `kP` by 0.01 until it stops shaking. |
| **Stalls near Target** | **Stiction** (Friction). | The power is too low to move the gears. Increase **Feedforward (kS)**{: data-tooltip="Static Feedforward: The minimum voltage needed to move the mechanism." }. |
| **Teleporting Robot** | High Ambiguity (3D). | The tag is too far or angled. Filter out any targets with `ta < 0.5` (Area) or `ambiguity > 0.2`. |

---

## 🌐 Network Issues

If you cannot connect to the Limelight, follow these steps in order:

1.  **Check IP:** Ensure your laptop is set to `DHCP` (Automatic) or a compatible Static IP (e.g., `10.51.3.5`).
2.  **Ping Test:** Open PowerShell and type `ping 10.51.3.11`.
    * *Reply:* Connection is good.
    * *Timeout:* Check the Ethernet cable and Radio breaker.
3.  **The "MDNS" Trap:** Do not use `limelight.local` in your Java code. It is slow and unreliable on the field. Always use the static IP or the NetworkTables key.

---

## 📸 Tuning the Pipeline

If the Limelight sees "garbage" (ceiling lights, shiny metal) as a target, you need to tune the thresholding.

1.  **Exposure:** Turn this **DOWN**. The image should be almost pitch black, with only the retro-reflective tape glowing green.
2.  **Black Level Offset:** Increase this to remove gray static noise.
3.  **Erosion steps:** Use this to separate two close targets that are merging into one blob.

!!! danger "Do Not Use WiFi"
    Never attempt to tune the Limelight over WiFi at a competition. You will lag out the network and likely get a warning from the FTA. Always tether via USB or Ethernet switch.

---

## ❓ Still Stuck?

If the hardware is fine but the code won't work, revert to the "isolate" strategy:
1.  Comment out the PID Controller.
2.  Print `tx` to the SmartDashboard.
3.  Manually push the robot. Does `tx` change as expected? (Positive when target is right?)