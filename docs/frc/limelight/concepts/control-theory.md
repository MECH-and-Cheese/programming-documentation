# Control Theory & PID

In robotics, we rarely just tell a motor to "go to 50% power." We want to tell it to "go to 30 degrees" or "aim at the tag." 

To do this, we use **Closed-Loop Control**{: data-tooltip="A control system that uses feedback (sensors) to correct itself continuously." }. The most common form of this in FRC is the **PID Controller**.

---

## The Feedback Loop



At its heart, a control loop is just doing simple subtraction over and over again, hundreds of times a second.

1.  **Setpoint (SP)**{: data-tooltip="The Goal: Where you want the system to be (e.g., 0 degrees error)." }: The target value we want to reach. For vision alignment, our Setpoint is usually **0** (center of the screen).
2.  **Process Variable (PV)**{: data-tooltip="The Reality: What the sensor is currently reading (e.g., current tx value)." }: The current reading from the sensor. For vision, this is our **`tx`** angle.
3.  **Error**{: data-tooltip="The Difference: Setpoint minus Process Variable." }: calculated as `Error = Setpoint - Process Variable`.

The robot constantly asks: *"How far away am I?"* and adjusts motor power based on the answer.

---

## The PID Equation

The motor output is calculated using three terms that work together. The formula looks like this:

$$
Output = (K_p \cdot e) + (K_i \cdot \int e) + (K_d \cdot \frac{\Delta e}{\Delta t})
$$

### 1. P - Proportional (The "Present")
* **Concept:** "The further I am, the harder I push."
* **Math:** Multiply the error by a constant ($K_p$).
* **Behavior:** If $K_p$ is too low, the robot is sluggish. If $K_p$ is too high, the robot will violently **oscillate**{: data-tooltip="Shaking back and forth around the target because the correction force is too strong." }.

### 2. I - Integral (The "Past")
* **Concept:** "I haven't reached the target yet, so I'll try harder over time."
* **Math:** Accumulates the sum of error over time.
* **Behavior:** In FRC Vision, **we rarely use I**. It often causes **Integral Windup**{: data-tooltip="When the I-term builds up too much power while the robot is stuck, causing it to overshoot massively once it gets free." }, making the robot overshoot the target wildly.

### 3. D - Derivative (The "Future")
* **Concept:** "I'm closing in too fast, I need to slow down."
* **Math:** Looks at the *rate of change* of the error.
* **Behavior:** This acts as a **damper**{: data-tooltip="A force that resists motion, smoothing out the response and preventing overshoot." } or brake. It helps stop the robot gently without overshooting.

---

## The "F" Term (Feedforward)

Sometimes, PID isn't enough. If your turret is heavy, a small P-value might not be enough to overcome gravity or friction.

**Feedforward (FF)**{: data-tooltip="A prediction term. We apply this power blindly because we know the physics of the system require it." } is a base value we add to the output *before* the PID loop does its work.

* **Static Feedforward ($kS$):** The minimum power required to break **stiction**{: data-tooltip="Static Friction. The force preventing the mechanism from starting to move." } and get the motor moving.
* **Gravity Feedforward ($kG$):** Used for arms/elevators to hold them up against gravity.

---

## Visualizing the Response



When tuning, you are looking for specific behaviors:

* **Overdamped:** The robot moves too slowly and takes forever to reach the target. (Increase $K_p$)
* **Underdamped:** The robot shoots past the target, turns back, shoots past again, and wobbles. (Decrease $K_p$ or Increase $K_d$)
* **Critically Damped:** The robot moves to the target as fast as possible without passing it. **This is the goal.**

!!! warning "Saturation"
    Motors can only go to 100% power (1.0). If your PID calculation results in a value of 5.0, the motor will simply output 1.0. This is called **Saturation**{: data-tooltip="When the calculated output exceeds the physical limit of the motor." }.
    
    This breaks the math of the PID loop because the robot isn't reacting as hard as the math thinks it should. Keep your PID constants small enough to stay within the -1.0 to 1.0 range for typical errors.