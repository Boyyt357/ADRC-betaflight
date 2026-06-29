
<img width="1376" height="768" alt="na" src="https://github.com/user-attachments/assets/eb513be2-56d0-4fa1-88e8-2a53b7f61d74" />


# Betaflight ADRC Controller (Active Disturbance Rejection Control)

This repository implements **Active Disturbance Rejection Control (ADRC)** on Betaflight, completely replacing the traditional PID loop. ADRC acts as a "PID Killer"—providing incredible stability, robust wind resistance, and smooth handling even with uncalibrated parameters, changing propeller sizes, or extreme, unbalanced dynamic payloads.

---

## For more Info

[![ADRC Betaflight](https://img.youtube.com/vi/BLTQN-Gw7LE/0.jpg)](https://www.youtube.com/watch?v=BLTQN-Gw7LE)



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

It is **highly recommended** to disable PID at minimum throttle in case the initial ADRC parameters are incorrect for your drone — otherwise it may behave unpredictably on arm while you adjust parameters. In the Betaflight command line interface (CLI) run:
```
set pid_at_min_throttle = off
```

### Example Parameters
| Drone type | Control Bandwidth (P) | Observer Bandwidth (I) | System Gain (D) |
| :--- | :---: | :---: | :---: |
| 10" drone (author's video) | 10 | 50 | 20 |
| 5" drone (jmsweng) | 10 | 110 | 100 |

---

## Compiling ADRC-Betaflight
Compiles using the same procedure as standard Betaflight, detailed [here](https://betaflight.com/docs/category/building).

It is also possible to build it on an ARM system (like a Raspberry Pi) — there's probably no good reason to do this. Tested on a Raspberry Pi 3B running Raspbian Trixie 13.5:

1) Update the system and install the toolchain
```
sudo apt update
sudo apt upgrade
sudo apt install gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential
```
2) Clone ADRC-Betaflight
```
git clone https://github.com/Boyyt357/ADRC-betaflight
```
3) Change into the ADRC-betaflight directory
```
cd ADRC-betaflight
```
4) Comment out the `$(error No toolchain URL defined ...)` line in `mk/tools.mk` (line 43)
5) Check the installed compiler version (confirm it installed properly)
```
arm-none-eabi-gcc -dumpversion
```
6) Append a local configuration override to `mk/local.mk`
```
echo "GCC_REQUIRED_VERSION = $(arm-none-eabi-gcc -dumpversion)" >> mk/local.mk
```
7) Build the firmware:
```
make clean
make configs
```
8) Build for your target board (a DAKEFPVF405, for example):
```
make DAKEFPVF405
```

---

## Hardware Issues

Betaflight does not manufacture or distribute their own hardware. While we are collaborating with and supported by a number of manufacturers, we do not do any kind of hardware support.

If you encounter any hardware issues with your flight controller or another component, please contact the manufacturer or supplier of your hardware, or check [Discord](https://discord.gg/n4E6ak4u3c) to see if others with the same problem have found a solution.

## Betaflight Releases

You can find our release [here](https://github.com/betaflight/betaflight/releases) on Github and we also have more detailed [release notes](https://www.betaflight.com/docs/category/release-notes) at [betaflight.com](https://www.betaflight.com).

## Open Source / Contributors

Betaflight is software that is **open source** and is available free of charge without warranty to all users.

For a complete list of contributors (past and present) see [Github](https://github.com/betaflight/betaflight/graphs/contributors).
