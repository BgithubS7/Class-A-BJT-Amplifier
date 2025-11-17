# Class A Audio Amplifier

High-linearity Class A BJT amplifier designed, simulated, and validated from first principles.  
Target use: low-power audio / lab reference stage.

---

## 1. Project Overview

This repo documents the **end-to-end design flow** for a single-stage Class A amplifier:

- Define performance requirements (gain, bandwidth, load, supply).
- Hand-calculate a bias point that keeps the transistor in **active region** over the signal swing.
- Verify performance using **LTspice / Ngspice** simulations (DC, AC, transient, distortion).
- Implement a **PCB**, build hardware, and compare **lab measurements** vs simulation.

---

## 2. Specs (Initial Targets)

- Supply: `VCC = 12 V`
- Load: `RL = 8 Ω` (speaker) or `2 kΩ` (lab load – update as needed)
- Midband small-signal gain: `|Av| ≈ 10–20 V/V`
- Bandwidth: `20 Hz – 20 kHz` (audio band)
- Max THD at 1 kHz, 1 Vrms out: `< 2%` (stretch goal)
- Quiescent current: sized so that signal swing does **not** clip for chosen load

---

## 3. Directory Structure

- `docs/` – Design docs: requirements, biasing, architecture, measurement plan, and results.
- `schematics/` – Schematic files for the Class A stage and bias network.
- `sim/` – SPICE simulations (LTspice and/or Ngspice).
- `pcb/` – Layout files + fab outputs.
- `src/analysis/` – Small Python helpers for biasing, THD, and plotting.
- `lab/` – Raw measurement data and Jupyter notebook analysis.
- `scripts/` – Utility scripts (e.g., auto-export plots from CSV).

---

## 4. Design Flow

1. **Requirements & Topology**  
   - Choose topology: common-emitter Class A with emitter degeneration and coupling capacitors.  
   - Set target gain and load conditions.

2. **Bias Point Design**  
   - Choose `ICQ` and `VCEQ` such that:
     - `VCEQ ≈ VCC / 2` for max symmetric swing.
     - `ICQ` high enough to support peak load current without saturation or cutoff.
   - Compute `RC`, `RE`, and the base bias divider (`R1`, `R2`).

3. **AC Design**  
   - Select coupling and bypass capacitors (`Cin`, `Cout`, `CE`) from low-frequency cutoff targets:
     - `f_L ≈ 1 / (2π R_eq C)`
   - Run **AC analysis** to verify midband gain and bandwidth.

4. **Transient & Distortion**  
   - Run **transient simulations** with a 1 kHz sine input.
   - Measure output clipping, THD, and thermal dissipation in `RC` and the transistor.

5. **PCB & Lab Validation**  
   - Lay out a simple PCB with good grounding and short high-current loops.
   - Measure:
     - DC bias points (Vb, Ve, Vc)
     - Small-signal gain vs frequency
     - THD vs output level

---

## 5. Quick Start

### 5.1 Simulations

1. Open `sim/ltspice/classA_amp.asc` in **LTspice**.
2. Run:
   - **.op** analysis for DC operating point.
   - **AC** sweep to confirm gain and bandwidth.
   - **Transient** analysis with a 1 kHz sine.
  
   - ## LAB MEASUREMENT
   - ![Image](https://github.com/user-attachments/assets/756a2189-358b-4ceb-99c2-8b375514160d)
  
3.## ![Image](https://github.com/user-attachments/assets/b9d8e379-7380-44f9-883b-cd105103d69e)

4.## ![Image](https://github.com/user-attachments/assets/6f0a8908-33d1-4d08-85a0-b6eb487cc845)

5. ## ![Image](https://github.com/user-attachments/assets/acaf015c-4684-4030-a84d-57ff48c4cfd6)

6.## ![Image](https://github.com/user-attachments/assets/a24a02ff-80d5-4b74-8a2b-683d8a3ee774)


