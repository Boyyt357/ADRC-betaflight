
<img width="1376" height="768" alt="na" src="https://github.com/user-attachments/assets/eb513be2-56d0-4fa1-88e8-2a53b7f61d74" />


# Betaflight ADRC Controller (Active Disturbance Rejection Control)

This repository implements **Active Disturbance Rejection Control (ADRC)** on Betaflight, completely replacing the traditional PID loop. ADRC acts as a "PID Killer"—providing incredible stability, robust wind resistance, and smooth handling even with uncalibrated parameters, changing propeller sizes, or extreme, unbalanced dynamic payloads.

---

## For more Info

<img width="2752" height="1536" alt="1T" src="https://github.com/user-attachments/assets/0e7b9a59-aff6-4e84-ac83-92dbd45ac3cc" />
https://youtu.be/BLTQN-Gw7LE



---

## Key Features
- **No Heavy Tuning Required:** Flies exceptionally well even out of the box with rough, uncalibrated values.
- **Unbalanced Payload Handling:** Actively estimates and cancels external forces dynamically, allowing stable flight even with swinging weights attached to a single motor arm.
- **Propeller Versatility:** Dynamically handles transitions between different prop sizes on the fly without changing parameters.

---

## How it Works: Repurposing the PID Fields
Instead of standard Proportional, Integral, and Derivative gains, this implementation repurposes the Betaflight PID configuration fields to control the ADRC system:

| Field | ADRC Parameter | Description |
| :---: | :--- | :--- |
| **P** | **Control Bandwidth** | Dictates the response speed to errors. Higher values yield faster correction; lower values correct errors more slowly. |
| **I** | **Observer Bandwidth** | Controls the speed of the Extended State Observer (ESO). It dictates how fast the controller estimates and cancels external forces (e.g., wind, prop wash). *Note: Setting this too high can amplify gyro noise and heat up motors.* |
| **D** | **System Gain** | Informs the controller how powerful the motors are based on acceleration and KV rating. Decreasing this increases overall gain (for fast-accelerating motors); increasing it decreases overall gain (for smoother control). |

---
## Support
https://www.patreon.com/c/EngineeringRobotic

## Hardware Issues

Betaflight does not manufacture or distribute their own hardware. While we are collaborating with and supported by a number of manufacturers, we do not do any kind of hardware support.

If you encounter any hardware issues with your flight controller or another component, please contact the manufacturer or supplier of your hardware, or check [Discord](https://discord.gg/n4E6ak4u3c) to see if others with the same problem have found a solution.

## Betaflight Releases

You can find our release [here](https://github.com/betaflight/betaflight/releases) on Github and we also have more detailed [release notes](https://www.betaflight.com/docs/category/release-notes) at [betaflight.com](https://www.betaflight.com).

## Open Source / Contributors

Betaflight is software that is **open source** and is available free of charge without warranty to all users.

For a complete list of contributors (past and present) see [Github](https://github.com/betaflight/betaflight/graphs/contributors).
