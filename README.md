# Klipper Configuration for Mingda D2 (MD D301 V1.0 Board)

Complete Klipper `printer.cfg` configuration for the **Mingda D2** 3D printer equipped with the **MD D301 V1.0** (`STM32F103ZET6`) motherboard, onboard **TMC2208** stepper drivers running in **UART** mode, and a **BLTouch** auto-leveling sensor.

> **Important Note on TMC2208 UART:**
> Without the `[tmc2208]` UART sections configured, the onboard drivers default to 1/8 microstepping in standalone mode and run at high hardware reference current, causing severe driver overheating, motor resonance, and doubled travel distances when `microsteps: 16` is set. This configuration initializes all 5 drivers via software UART (`PD6`, `PC11`, `PA8`, `PG6`, `PG2`) with factory RMS currents (`580mA` XYZ / `600mA` E0) extracted directly from Mingda's Marlin source code.

> **⚠️ BLTouch / 3DTouch Required:**
> This configuration is set up for a printer equipped with a **BLTouch** probe (using `probe:z_virtual_endstop` for Z homing, `[bed_mesh]`, and `[z_tilt]`).
> * **If your printer does NOT have a BLTouch** (using the stock mechanical Z endstop switch), change `endstop_pin: probe:z_virtual_endstop` under `[stepper_z]` to `endstop_pin: ^!PG0`, add `position_endstop: 0`, and comment out the `[bltouch]`, `[bed_mesh]`, `[z_tilt]`, and `[screws_tilt_adjust]` sections.

## Hardware Specifications
* **MCU:** `STM32F103ZET6` (72 MHz, 512KB Flash)
* **Stepper Drivers:** 5x `TMC2208` with single-wire UART (Independent Dual Z: `Z` and `Z1`)
* **Build Volume:** 230 x 230 x 260 mm
* **Z Probe:** BLTouch / 3DTouch required (`PB1` sensor / `PB0` servo)

## MD D301 V1.0 Pinout Reference

| Component | Pin | Notes |
| :--- | :--- | :--- |
| **X Stepper** | Step: `PD2`, Dir: `PC10`, En: `!PD3` | UART: `PD6`, Endstop: `^!PF12` |
| **Y Stepper** | Step: `PA10`, Dir: `PA9`, En: `!PC12` | UART: `PC11`, Endstop: `^!PF14` |
| **Z Stepper (Left)** | Step: `PG8`, Dir: `!PG7`, En: `!PC9` | UART: `PA8`, Stock Endstop: `^!PG0` |
| **Z1 Stepper (Right)**| Step: `PG4`, Dir: `!PG3`, En: `!PG5` | UART: `PG6` |
| **Extruder (E0)** | Step: `PD12`, Dir: `PD11`, En: `!PD13`| UART: `PG2` |
| **Hotend** | Heater: `PC6`, Thermistor: `PC4` | EPCOS 100K |
| **Heated Bed** | Heater: `PC8`, Thermistor: `PC5` | EPCOS 100K |
| **Part Cooling Fan** | `PC7` | Hardware PWM |
| **BLTouch** | Sensor: `PB1`, Control: `PB0` | Required for default config |
| **Filament Sensor** | `^PG1` | Mechanical switch |
| **Beeper / Piezo** | `PG11` | PWM capable |
| **TFT Backlight** | `PG10` | Stock ILI9488 FSMC display backlight |
| **PS_ON / Neopixel** | PSU: `PA1`, Neopixel: `PB9` | Optional headers |

## Features Included
* Full **TMC2208 UART** control with StealthChop, 1/256 interpolation, and reduced hold current.
* **BLTouch** virtual Z-endstop homing, `[bed_mesh]` leveling, and automatic dual-Z gantry alignment via **`[z_tilt]`**.
* Bed corner leveling helper via **`[screws_tilt_adjust]`**.
* Stock TFT backlight switch (`lcd_backlight`) to turn off the unused screen.
