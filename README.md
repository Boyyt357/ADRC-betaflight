<img width="1376" height="768" alt="Gemini_Generated_Image_exhvovexhvovexhv" src="https://github.com/user-attachments/assets/3014451f-cb07-47a4-b374-432f6aef1514" />

# Betaflight ADRC Controller (Active Disturbance Rejection Control)

This repository implements **Active Disturbance Rejection Control (ADRC)** on Betaflight, completely replacing the traditional PID loop. ADRC acts as a "PID Killer"—providing incredible stability, robust wind resistance, and smooth handling even with uncalibrated parameters, changing propeller sizes, or extreme, unbalanced dynamic payloads.

---

## For more Info

[![ARDC Betaflight](https://img.youtube.com/vi/BLTQN-Gw7LE/0.jpg)](https://www.youtube.com/watch?v=BLTQN-Gw7LE)

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

It is **highly recommended** to set PID in betaflight to off at minimum throttle in case the initial ARDC parameters are incorrect for your drone. Otherwise it may behave unpredictably on arm while adjusting parameters. To do this go to the command line interface (CLI) in betaflight and run the following command
```
set pid_at_mid_throttle = off
```
### Example Parameters
| Drone type | Control Bandwidth | Observer Bandwidth | System Gain |
| :---: | :--- | :--- | :--- |
| 10" Drone | 10 | 50 | 20 |
| 5" Drone | 10 | 110 | 100 |

Control Bandwidth (P)  = 10, Observer Bandwidth (I) = 110, System Gain (I) = 100.

---

## Compiling ARDC-Betaflight
Compiles using the same procedure as standard Betaflight detailed [here](https://betaflight.com/docs/category/building)

It is also possible to build it on an ARM system (like a Raspberry Pi) with the following instructions (there's probably no good reason to do this). Tested on a Raspberry Pi 3B running Raspbian Trixie 13.5
1) Update system and install toolchain
```
sudo apt update
sudo apt upgrade
sudo apt install gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential
```
2) Clone ADRC-Betaflight
```
git clone https://github.com/Boyyt357/ADRC-betaflight 
```
3) Change to ARDC-betaflight directory
```
cd ARDC-betaflight
```
4) Modify betaflight tools.mk file
```
nano mk/tools.mk
```
5) Comment out line 43:   
```
# $(error No toolchain URL defined for $(HOST_OS)-$(HOST_ARCH). Stop.)
```
6) Save and exit (ctrl+x, enter)
7) Check installed compiler version to be used for compilation;  double check to see that it's installed properly
```
arm-none-eabi-gcc -dumpversion
```
8) Append local configuraion override to mk/local.mk
```
echo "GCC_REQUIRED_VERSION = $(arm-none-eabi-gcc -dumpversion)" >> mk/local.mk
```	
9) Compile firmware:
```
make clean
make configs
```
10) Make firmware for whatever target board. For a DAKEFPVF405 for example, this would be:
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
