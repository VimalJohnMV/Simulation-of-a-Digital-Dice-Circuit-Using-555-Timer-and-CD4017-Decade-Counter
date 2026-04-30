# Simulation of a Digital Dice Circuit Using 555 Timer and CD4017 Decade Counter

**An open-source mixed-signal circuit simulation officially approved by the FOSSEE (Free/Libre and Open Source Software for Education) project, IIT Bombay.**

This repository contains the complete eSim workspace and modified SPICE netlist for a 6-sided digital dice. The project demonstrates the successful integration of analog clock generation with digital sequential logic, utilizing advanced netlist-level modifications to bypass graphical limitations and execute a mathematically flawless mixed-signal simulation.

## 🧠 Key Technical Highlights

This project goes beyond basic schematic capture by implementing several advanced simulation techniques:

* **The "Human Thumb" Hack (Mechanical Input Modeling):** Ngspice transient analysis cannot accept real-time mechanical switch inputs. To accurately simulate a user pressing and releasing a "roll" button, the main VCC supply is mathematically modeled as a Pulsed Voltage Source `PULSE(0 5 0 1u 1u 0.73 10)`. This creates a deterministic 730ms window where the analog clock runs, after which the digital logic accurately latches its final state.
* **Direct Netlist Optimization:** To overcome GUI-induced wire-routing glitches and iteration limit crashes, the raw `.cir.out` SPICE netlist was manually refactored. The digital logic loop and bridge inputs were hard-coded via text variables to ensure the math engine could resolve the matrix without convergence errors.
* **Mixed-Signal Bridging:** Successfully utilizes behavioral XSpice models, deploying `adc_bridge` and `dac_bridge` components to translate the analog astable multivibrator waveform into pure digital logic states, and back to analog forward-voltage drops for the LED loads.

## 📐 Circuit Architecture

The circuit is divided into two primary stages:
1. **Analog Clock Generator:** An LM555N timer IC configured in astable mode. The RC timing components ($R_1 = 1k\Omega$, $R_2 = 10k\Omega$, $C_1 = 10\mu F$) are tuned to generate a ~6.86 Hz square wave clock pulse.
2. **Digital Logic Sequencer:** A CD4017 decade counter. Clock pulses are fed into PIN 14. To restrict the count to exactly six states (representing six dice faces), the 7th output (Q6) is hardwired directly to the RESET pin (PIN 15), creating a continuous modulo-6 logic loop. 

## 🚀 How to Run the Simulation

**CRITICAL WARNING:** This project relies on a manually optimized `.cir.out` netlist. **DO NOT** click the "Generate Netlist" or "KiCad to Ngspice" buttons in the eSim dashboard, or it will overwrite the custom SPICE code and break the simulation.

1. Clone this repository to your local machine.
2. Open the **eSim** software.
3. Click **Open Project** and select the `Digital_Dice` directory.
4. From the main eSim dashboard, click the green **Simulate** button directly.
5. In the Python plotting window, select the 555 output node to view the clock generation, and the DAC bridge output nodes to view the cascading LED sequence.

## 📊 Results and Verification

The simulation accurately models:
* The 5 consecutive clock pulses generated during the 730ms active window.
* The sequential shifting of logic HIGH across the output pins.
* The proper analog modeling of the final state, showing a safe continuous driving current of ~3.15mA across the LED loads, respecting the theoretical forward voltage drop.

## 👨‍💻 Author
**Vimal John M V** *Electronics and Engineering Student | Open-Source Hardware Enthusiast*

*This project was developed and submitted as part of the FOSSEE eSim Circuit Simulation initiative.*
