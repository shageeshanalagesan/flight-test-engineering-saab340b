![Uploading IMG_8709.JPG…]()
# AERO317 – Flight Test & Model Validation

**Saab 340B+ Flight Laboratory | University of Liverpool**

A comprehensive flight test engineering project conducted aboard Cranfield University's instrumented Saab 340B+ (G-NFLB) flying laboratory. This assignment covers the full pipeline from in-flight data acquisition to aerodynamic model validation — including centre of gravity computation, longitudinal static stability analysis, drag polar estimation, and lateral-directional dynamic mode characterisation.

---

## 📋 Table of Contents

<details>
<summary>🔍 Click to expand navigation</summary>

- [✈️ Overview](#️-overview)
- [🛩️ Test Aircraft](#️-test-aircraft)
- [⚙️ Aircraft Parameters](#️-aircraft-parameters)
- [🎯 Analyses Performed](#-analyses-performed)
  - [1. Centre of Gravity](#1-centre-of-gravity)
  - [2. Static Stability Analysis](#2-static-stability-analysis)
  - [3. Drag Performance](#3-drag-performance)
  - [4. Lateral-Directional State-Space Model](#4-lateral-directional-state-space-model)
  - [5. Lateral-Directional Dynamic Modes](#5-lateral-directional-dynamic-modes)
- [📊 Key Results Summary](#-key-results-summary)
- [🔬 Assumptions & Test Conditions](#-assumptions--test-conditions)
- [📁 Repository Structure](#-repository-structure)
- [👤 Author](#-author)

</details>

---

## ✈️ Overview

This project is part of AERO317: Flight Dynamics & Control at the University of Liverpool. Students participated in a real flight laboratory aboard the Saab 340B+ flying testbed operated by Cranfield University's National Flying Laboratory Centre (NFLC). Onboard Surface Pro tablets provided live flight data via a custom SCITEK interface, recording airspeed, thrust, altitude, control surface angles, and inertial measurements.

The objectives were to:
- Measure and validate aircraft aerodynamic and stability characteristics from real flight data
- Build a theoretical lateral-directional state-space model and compare against flight test results
- Understand how linear models deviate from real-world behaviour due to nonlinear effects, turbulence, and pilot inputs

![Uploading IMG_8709.JPG…]() ![Uploading IMG_8737.JPG…]()


---

## 🛩️ Test Aircraft

**Saab 340B+ — Registration G-NFLB**

> A 33-seat regional turboprop, limited to 24 seats for flight lab operations. Modified with a full data acquisition suite including IMU, CSDB, AoA/sideslip vanes, GPS, IRS, and strain gauges.

| Specification | Value |
|---|---|
| Aircraft Type | Saab 340B+ Turboprop |
| Engines | 2× General Electric CT7-9B (1750 SHP each) |
| Max Take-Off Mass | 13,155 kg |
| Wing Area (Sw) | 41.8 m² |
| Mean Aerodynamic Chord (c̄) | 2.08 m |
| Wing Span | 21.44 m |
| Empty Mass (with pilots) | 8,695 kg |

---

## ⚙️ Aircraft Parameters

The following aerodynamic and geometric parameters were used throughout the analysis:

| Parameter | Symbol | Value | Units |
|---|---|---|---|
| Wing Lift Curve Slope | a | 5.15 | rad⁻¹ |
| Wing Aerodynamic Centre | h_AC | 0.25 | n.d. |
| Wing Zero Lift Pitching Moment | C_M0 | −0.0631 | n.d. |
| Wing Dihedral Angle | Γ | 7 | deg |
| Wing Sweep Angle | Λ | 33 | deg |
| Horizontal Tail Area | S_HT | 14.57 | m² |
| HT Lift Curve Slope | a₁_HT | 4.4 | rad⁻¹ |
| HT Moment Arm | d_HT | 8.86 | m |
| Downwash Gradient | dε/dα | 0.308 | n.d. |
| Vertical Tail Area | S_VT | 7.77 | m² |
| VT Lift Curve Slope | a₁_VT | 3.41 | rad⁻¹ |
| Ixx | — | 85,386.77 | kg·m² |
| Iyy | — | 699,503.97 | kg·m² |
| Izz | — | 739,543.78 | kg·m² |
| Ixz | — | 104,416.18 | kg·m² |

**Aerofoils used:** Wing — NASA MS(1)-0316/0312 | HT — NACA 0012

---

## 🎯 Analyses Performed

### 1. Centre of Gravity

The CG position was calculated from the aircraft load sheet using mass-moment summation across all seat rows, fuel, crew, demonstrator, and a 200 kg ballast box.

**Formula used:**

$$\frac{CG - LEMAC}{MAC} = \left[\frac{\sum m \cdot x}{\sum m} - 10.472\right] \times \frac{100}{2.085}$$

| Mass Component | Mass (kg) | Distance from Datum (m) | Moment (kg·m) |
|---|---|---|---|
| Aircraft + Crew (BEW) | 8,695 | 10.69 | 92,950 |
| Fuel | 1,666 | 11.18 | 18,626 |
| Demonstrator | 80 | 6.91 | 553 |
| Seat Rows 3–9 | 1,343 | various | 15,013 |
| Ballast Box | 200 | 17.12 | 3,424 |
| **Total** | **11,984** | — | **131,565** |

> ✅ **CG Position = 24.29% c̄**  (measured from leading edge of main wing)

---

### 2. Static Stability Analysis

**Flight Test Method (Stick-Fixed):**

Elevator angle versus weight coefficient (C_W) was recorded at multiple speeds for two CG positions (Groups A and E). The slope dη/dC_W at each CG was plotted to find the stick-fixed neutral point where the slope equals zero.

> 📍 Neutral Point (flight test): **45.28% MAC**
>
> **Static Margin (flight test) = 45.28 − 24.29 = 20.99%** → Aircraft is statically stable ✅

**Theoretical Method:**

Using standard static stability theory:

$$h_n = h_{ac} + \bar{V}_T \cdot \frac{a_1}{a}\left(1 - \frac{d\varepsilon}{d\alpha}\right) = 1.127$$

> 📍 Neutral Point (theory): **54.22% MAC**
>
> **Static Margin (theory) = 54.22 − 24.29 = 29.93%**

| Method | Neutral Point (% MAC) | Static Margin (% c̄) |
|---|---|---|
| Flight Test | 45.28 | 20.99 |
| Theory | 54.22 | 29.93 |

> ⚠️ The ~9% difference arises because the theoretical model ignores compressibility, fuselage contributions, and real downwash non-uniformity. Flight data also includes real-world asymmetries and instrument noise.

---

### 3. Drag Performance

Eight steady level-flight test points were flown across a speed range of 125–220 KEAS at ~3,670 ft pressure altitude. Thrust data from both engines (LH and RH) was used to compute drag under the assumption of non-accelerating, wings-level flight.

**Key equations:**

$$C_L = \frac{2L}{\rho_0 V_e^2 S_w}, \quad C_D = \frac{2D}{\rho_0 V_e^2 S_w}, \quad C_D = C_{D_0} + K C_L^2$$

**Flight Test Data:**

| Ve (m/s) | CL | CL² | Drag (N) | CD |
|---|---|---|---|---|
| 113.43 | 0.3597 | 0.1294 | 12,450 | 0.0378 |
| 102.94 | 0.4368 | 0.1908 | 10,945 | 0.0403 |
| 93.02 | 0.5349 | 0.2861 | 9,518 | 0.0430 |
| 82.13 | 0.6861 | 0.4707 | 9,025 | 0.0523 |
| 78.03 | 0.7601 | 0.5777 | 8,914 | 0.0572 |
| 71.91 | 0.8951 | 0.8011 | 8,564 | 0.0647 |
| 67.29 | 1.0222 | 1.0450 | 8,759 | 0.0756 |
| 64.93 | 1.0977 | 1.2050 | 9,635 | 0.0893 |

By linear regression of C_D against C_L², the drag polar constants were extracted:

> ✅ **C_D0 = 0.0308** (zero-lift drag coefficient)
>
> ✅ **K = 0.0453** (induced drag factor)

---

### 4. Lateral-Directional State-Space Model

A full 4-state lateral-directional model was computed using aerodynamic derivatives derived from the aircraft parameters spreadsheet. The state vector comprises sideslip velocity (v), roll rate (p), yaw rate (r), and bank angle (φ).

$$\dot{x} = A x, \quad x = [v, \; p, \; r, \; \phi]^T$$

$$A = \begin{bmatrix} \frac{Y_v}{mV} & \frac{Y_p}{mV} & \frac{Y_r}{mV}-1 & \frac{g\cos\theta_0}{V} \\ \frac{L_v}{I_{xx}} & \frac{L_p}{I_{xx}} & \frac{L_r}{I_{xx}} & 0 \\ \frac{N_v}{I_{zz}} & \frac{N_p}{I_{zz}} & \frac{N_r}{I_{zz}} & 0 \\ 0 & 1 & \tan\theta_0 & 0 \end{bmatrix}$$

The eigenvalues of matrix A yield the natural frequencies and damping ratios of the three lateral-directional modes: Dutch Roll, Spiral, and Roll.

---

### 5. Lateral-Directional Dynamic Modes

**Dutch Roll Mode — Flight Test:**

The yaw rate response following a rudder input was analysed. The 4th and 5th consecutive oscillation peaks were identified:

| Peak | Time (s) | Amplitude (°/s) |
|---|---|---|
| 4th | 2768.82 | 9.1662 |
| 5th | 2772.66 | 4.4822 |

$$f = \frac{1}{T} = \frac{1}{2772.66 - 2768.82} = 0.2604 \; \text{Hz}$$

$$\omega_n = 2\pi f = 1.636 \; \text{rad/s}, \quad \delta = \ln\!\left(\frac{A_1}{A_2}\right), \quad \zeta = \frac{\delta}{\sqrt{4\pi^2 + \delta^2}} = 0.1131$$

**Comparison: Flight Test vs. State-Space Model**

| Parameter | Flight Test | State-Space Model |
|---|---|---|
| Natural Frequency ωn | 1.636 rad/s | 1.746 rad/s |
| Damping Ratio ζ | 0.1131 | 0.401 |

> ⚠️ Both methods show close agreement on natural frequency (~6.7% difference). However, the model significantly over-predicts damping (ζ_model ≈ 3.5× ζ_flight). This is because the linear state-space model cannot capture nonlinear aerodynamic effects, real turbulence, or pilot-induced inputs present during the actual flight test.

---

## 📊 Key Results Summary

| Analysis | Result |
|---|---|
| CG Position | **24.29% c̄** |
| Stick-Fixed Neutral Point (flight) | **45.28% MAC** |
| Static Margin (flight test) | **20.99%** — statically stable ✅ |
| Static Margin (theory) | **29.93%** |
| Zero-Lift Drag Coefficient C_D0 | **0.0308** |
| Induced Drag Factor K | **0.0453** |
| Dutch Roll Frequency (flight) | **1.636 rad/s** |
| Dutch Roll Damping (flight) | **ζ = 0.1131** |
| Dutch Roll Frequency (model) | **1.746 rad/s** |
| Dutch Roll Damping (model) | **ζ = 0.401** |

---

## 🔬 Assumptions & Test Conditions

- **Flight altitude:** ~3,670 ft pressure altitude, ISA conditions assumed
- **OAT:** ~8–9.4°C during drag test
- **Level flight assumed:** cos θ ≈ 1, sin θ ≈ 0 for small pitch attitudes
- **ISA sea-level density:** ρ₀ = 1.225 kg/m³
- **Gravity:** g₀ = 9.80665 m/s²
- **EAS to m/s conversion:** multiply knots by 0.51444
- **Drag = Thrust** in non-accelerating, wings-level, constant-altitude flight
- **State-space model:** linear approximation about a trimmed equilibrium — does not capture nonlinear effects, coupling, or atmospheric disturbances

---

## 📁 Repository Structure

```
AERO317-FlightTest-ModelValidation/
├── data/
│   ├── drag_performance_data.csv        # Ve, CL, CD, thrust measurements
│   ├── static_stability_lss_data.csv    # Elevator angle vs Cw measurements
│   └── dutch_roll_yaw_rate.csv          # Yaw rate time-series for dynamic modes
├── analysis/
│   ├── cg_calculation.xlsx              # Load sheet & CG computation
│   ├── drag_polar.xlsx                  # CL² vs CD regression
│   └── state_space_model.m              # MATLAB state-space matrix computation
├── report/
│   └── AERO317_Report4_Alagesan_201683484.pdf
├── pre-brief/
│   └── PRE-BRIEF_Liverpool_2flt.pdf     # Flight laboratory briefing slides
└── README.md
```

---

## 👤 Author

**Shageeshan Alagesan**
Student ID: 201683484
Module: AERO317 — Flight Dynamics & Control
University of Liverpool

[![GitHub](https://img.shields.io/badge/GitHub-shageeshanalagesan-181717?style=flat&logo=github)](https://github.com/shageeshanalagesan)

---

*Flight data © Cranfield University / National Flying Laboratory Centre (NFLC). Aircraft: Saab 340B+ G-NFLB.*
