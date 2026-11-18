# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

KIT Applied Engineering Project — design and construction of an experimental setup to measure residual stresses in thin films over −50 °C to +100 °C using the **substrate-curvature method** (Multi-beam Optical Stress Sensor, MOSS). This is a hardware/CAD engineering project; there are no build, lint, or test commands.

## Repository structure

- `literature/` — reference documents for the project (PDFs)
  - `Thin_Film_Stress_Measurement_System.pdf` — system description: physics, apparatus, data processing, calibration
  - `Copper_stage_design_report.pdf` — stage design working report: thermal stage, sample supports, heater/cooling concepts, materials

## Core physics (Stoney's equation)

Film stress is extracted from substrate curvature change Δκ = κ_coated − κ_bare:

```
σ_f = (M_s · t_s²) / (6 · t_f) · κ       M_s = E_s / (1 − ν_s)
```

The MOSS method measures beam spacing `d` on the CCD. Curvature is recovered from the spacing ratio:

```
κ = (cos α / 2L) · (d/d₀ − 1)
```

where `L` is the optical lever arm and `d₀` is the flat-sample reference spacing. Tilt and vibration cancel because they shift all beams equally; only curvature changes their *spacing*.

Temperature dependence: `dσ_f/dT ≈ −M_f (α_f − α_s)`. Hysteresis between heating and cooling indicates relaxation, plasticity, or phase changes in the film.

## System architecture

**Optical signal chain:** Diode laser (635–670 nm) → MOSS etalon (3 parallel beams, spacing d₀ ≈ 3.9 mm) → fold mirrors + chamber window → sample inside thermal chamber → CCD (measures spacing d) → PC/DAQ outputs σ_f(T).

**Thermal control loop:** PID controller → Peltier stack (heating and cooling) → Pt100 sensor at sample → feedback. Ramp rate target: 1–2 K/min to limit thermal gradients.

## Copper stage design (Person C work package)

**Requirements:** −50 to +100 °C, sample footprint 30 × 10 mm, substrate thickness 100–300 µm, film thickness 20 µm, vacuum (no helium backfill), LN₂ cooling, sample free to bend (no clamping or gluing).

**Stage material:** Cu OFE C10100 — density 8930 kg/m³, specific heat 390 J/(kg·K), conductivity 394 W/(m·K), CTE 17.7 × 10⁻⁶/K (20–300 °C range; use NIST fits within their stated range only).

**Three-point sample support:** Spherical-crown dome inserts with cylindrical foot seated in a flat-bottom recess in the stage. Side clearance allows free thermal bending; bottom contact provides the thermal path. Two provisional layouts under evaluation:
- Layout A (equal end distances): contacts at (−10, −3), (−10, +3), (+10, 0) mm
- Layout B (equal nominal loads): contacts at (−6, −3), (−6, +3), (+12, 0) mm

**Heater options:** flat foil heater clamped to underside, or embedded cartridge heater. Cooling via LN₂ through copper foil/braid thermal links with solid terminals.

**Structural rods:** thin stainless steel or qualified ceramic (choose by stiffness and heat-leak together).

## Key design constraints and open points

- Thin-film approximation (Stoney) requires t_f ≪ t_s. The 20 µm film against 100–300 µm substrate (ratio 0.067–0.20) is at the edge; validate before applying classical Stoney conversion.
- Sample temperature (not block temperature alone) is the critical measurement quantity — confirm Pt100 placement or separate sample sensor.
- Stress resolution target ≲ 1 MPa must be confirmed with supervisors.
- Vacuum pressure, substrate material/orientation, beam footprint, permitted curvature error, and heater power limits are still open inputs before final sizing.
- Creo material library entries for copper are starting points only — verify grade, temper, and temperature range before using for structural assessment.
