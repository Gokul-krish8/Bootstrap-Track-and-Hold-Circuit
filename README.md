# Bootstrap Track-and-Hold with Nakagome Charge Pump

## Overview

This project demonstrates the design and simulation of a **Bootstrap Track-and-Hold circuit** using a **Nakagome Charge Pump**, implemented in **LTspice** with **TSMC 180nm technology**. The design is inspired by concepts from modern data converter architectures and includes boosted clock techniques for enhanced signal tracking.

## Nakagome Charge Pump – Working Principle

The Nakagome charge pump circuit comprises:
- Cross-coupled MOSFETs (M1 and M2)
- Capacitors (C1 and C2)
- Inverter (A2)

### Operation:

- Initially, the capacitors are uncharged.
- When `clkbar` is LOW, the output of inverter A2 is **Vdd**, turning on M1 and charging C1 to **Vdd - Vth**.
- When `clkbar` goes HIGH, M2’s gate reaches **2Vdd - Vth**, charging C2 and C1 to **Vdd**.
- This configuration generates **boosted clock signals**: `clk + Vdd` and `clkbar + Vdd`.

## Why Bootstrap Instead of Simple MOS Switch

A conventional MOS switch may fail to track signals correctly because:
- Gate is driven from 0 to Vdd.
- Source is a time-varying input.
- **Ron ∝ 1 / (Vgs - Vth)**: when source ≈ gate, **Ron** becomes too large (switch appears open).

The **bootstrap circuit** maintains a constant Vgs, ensuring low Ron and accurate signal tracking.

## Bootstrap Circuit – Working Principle

- A capacitor (C3) charged to Vdd is placed between the gate and source of the tracking NMOS.
- With **Vin** at the source and **Vdd + Vin** at the gate, **Vgs = Vdd**, and **Ron remains low**.
- M9 (PMOS) and M8 (NMOS) control charging/discharging of C3.
  - When `clk` is LOW: M9 off, M8 on → C3 charges to Vdd.
  - When `clk` is HIGH: M9 on, M8 off → bottom of C3 at Vin, top at **Vdd + Vin**.
- This **boosted signal** is applied to the gate of M14 (NMOS), enabling reliable signal tracking.

## Simulation Details

- **Tool**: LTspice
- **Technology**: TSMC 180nm
- **Reference**:  
  A. Abo et al., “A 1.5-V, 10-bit, 14.3-MS/s CMOS Pipeline Analog-to-Digital Converter,” *IEEE Journal of Solid-State Circuits*

## Acknowledgment

Special thanks to my professor, who constantly encouraged us to read IEEE papers to stay updated with current advancements and deepen our understanding.

## Folder Structure

```bash
├── schematics/
│   ├── nakagome_charge_pump.asc
│   ├── bootstrap_th.asc
│
├── waveforms/
│   ├── charge_pump_output.png
│   ├── bootstrap_tracking.png
│
└── README.md

