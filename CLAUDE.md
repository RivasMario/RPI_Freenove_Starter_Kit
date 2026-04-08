# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the **Freenove Ultimate Starter Kit for Raspberry Pi** — a learning kit with code examples in C, Python (RPi.GPIO), Python (GPIOZero), Processing, and Scratch 3. All code is intended to run on a Raspberry Pi, not on the development machine.

## Repository Structure

- `Code/C_Code/` — C examples using the WiringPi library (one directory per project)
- `Code/Python_Code/` — Python examples using `RPi.GPIO` (BOARD pin numbering by default)
- `Code/Python_GPIOZero_Code/` — Python examples using `gpiozero` (BCM pin numbering by default)
- `Libs/C-Libs/ADCDevice/` — C++ shared library for ADC modules (PCF8591 and ADS7830), must be built and installed before compiling ADC-related C projects
- `Libs/Python-Libs/` — Python ADCDevice library (`.tar.gz`)
- `Processing/Sketches/` — Processing IDE sketches for visualization/control
- `Processing/Apps/` — Compiled Processing apps
- `Scratch3/` — Scratch 3 `.sb3` project files

## Running Code (on Raspberry Pi)

**Python (RPi.GPIO):**
```bash
sudo python3 Code/Python_Code/<project>/<script>.py
```

**Python (GPIOZero):**
```bash
python3 Code/Python_GPIOZero_Code/<project>/<script>.py
```

**C — compile and run:**
```bash
gcc Code/C_Code/<project>/<file>.c -o <output> -lwiringPi
sudo ./<output>
```
For C++ projects (ADC, MPU6050):
```bash
g++ Code/C_Code/<project>/<file>.cpp -o <output> -lwiringPi -lADCDevice
sudo ./<output>
```

**Stop any running script:** `Ctrl+C` — Python examples use `KeyboardInterrupt` to clean up GPIO.

## ADCDevice Library Setup (required for ADC projects)

```bash
cd Libs/C-Libs/ADCDevice
sudo bash build.sh
```
This compiles `libADCDevice.so`, installs it to `/usr/lib`, and copies `ADCDevice.hpp` to `/usr/include/`.

For Python ADC projects, install from `Libs/Python-Libs/ADCDevice-*.tar.gz`:
```bash
cd Libs/Python-Libs
pip3 install ADCDevice-*.tar.gz
```

## Key Architecture Notes

**Project numbering:** Directory names follow the pattern `XX.Y.Z_ProjectName` where XX is the chapter, Y is the lesson, Z is the variant. The same lesson number appears across C, Python, and GPIOZero code trees.

**Pin numbering conventions:**
- `RPi.GPIO` examples default to **BOARD** (physical pin) numbering via `GPIO.setmode(GPIO.BOARD)`
- `gpiozero` examples default to **BCM** (GPIO number) numbering
- WiringPi uses its own pin numbering scheme

**I2C device detection:** Several C projects (ADC, LCD, MPU6050) auto-detect the connected I2C device at runtime using `i2cdetect -y 1`. Common addresses: PCF8591 at `0x48`, ADS7830 at `0x4b`.

**WebIO project** (`26.1.1_WebIO`): Requires updating `host_name` in `WebIO.py` to the Raspberry Pi's actual IP address before running.

**Hardware dependencies:** Each project corresponds to a specific circuit wiring described in `Tutorial.pdf` or `Tutorial_GPIOZero.pdf`. Code alone is not sufficient — the correct components must be wired to the correct pins.
