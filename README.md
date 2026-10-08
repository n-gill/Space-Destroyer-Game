# Space Destroyer

[Space Destroyer Game - Google Slides](https://docs.google.com/presentation/d/1IH4_T6WE_UoEuoKrWXi-Y2QASrMYliuBAjHxkgwGBeA/edit?usp=sharing)

A hardware-level arcade space shooter written in Verilog HDL, designed for execution on Intel/Altera FPGA development boards like the DE1-SoC. The project directly drives a VGA monitor for rendering and uses onboard pushbuttons and 7-segment displays for player input and score tracking.

## Architecture and Modules

* **VGA Object Rendering:** Utilizes Finite State Machines (FSM) to handle the complete drawing lifecycle (Draw → Wait → Erase → Update Coordinates → Draw) for the 640x480 VGA standard. Sprites, including the player's ship, are mapped into bit arrays for pixel-by-pixel rendering.
* **Movement Controllers (`ship_clk`):** Steps down the base 50MHz hardware clock using a 22-bit counter to generate a precise enable signal, governing horizontal movement speeds smoothly.
* **Weapon System (`ply_bul`):** Tracks the player's live X/Y coordinates to spawn 3x10 pixel projectiles when the fire key is triggered. The module handles the vertical translation of the bullet across the VGA grid and manages the single-fire pulse logic.
* **Pseudo-Random Number Generation (`random_gen`):** An 8-bit Linear Feedback Shift Register (LFSR) used to generate unpredictable 8-bit values, driving enemy spawn coordinates and randomized game events.
* **Score Tracking (`score` & `display`):** A survival-time scoring system that increments a 4-digit counter every 3 seconds (150,000,000 clock cycles at 50MHz). The data is passed through a decoder to drive the onboard HEX0-HEX3 7-segment displays.

## Hardware & Simulation Requirements

* **Target Hardware:** Intel/Altera Cyclone-series FPGA (optimized for DE-series boards).
* **I/O Peripherals:** Standard VGA Monitor, onboard pushbuttons (`KEY`), onboard 7-segment displays (`HEX`).
* **Simulation Configuration:** The code features parameterized animation speeds (`KK` and `MM` variables) that can be swapped between ModelSim execution (faster simulated ticks) and physical hardware/DESim execution (standard timing).
