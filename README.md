#  Piano Movers Problem — Ant Colony Simulation 🐜

> A stochastic multi-agent simulation of cooperative cargo transport, inspired by how real ant colonies navigate large objects through confined environments.

<img width="871" height="606" alt="image" src="https://github.com/user-attachments/assets/c7363fe8-b434-4d54-8be6-40e9837c002a" />

---

## Overview

This project simulates the **Piano Movers Problem** — moving a large rigid body through a cluttered environment — using a biologically-grounded model of ant collective transport. The simulation models the emergent, decentralized physics of real *Paratrechina longicornis* ant colonies carrying oversized cargo, as studied by Feinerman et al.

Ants have no global map. Direction emerges from local force signals, stochastic role-switching, and a small fraction of "informed" ants who carry knowledge of the nest direction and gradually forget it.

---

## Features

- **Gillespie stochastic time evolution** — event-driven dynamics with variable timestep
- **Puller / Lifter role model** — ants dynamically switch roles based on local force signals via sigmoid probability functions
- **Informed vs. uninformed ants** — directional information injected by newly-attached ants, decaying at rate K_forget = 0.1 s⁻¹
- **Overdamped cargo dynamics** — translational and rotational velocity updated at each event
- **Wall collision handling** — penalty-method repulsion with a dense perimeter shield to prevent tunneling
- **Extended scenarios:**
  - Obstacle-free transport (base case)
  - L-shaped corridor navigation
  - Inclined surface transport with slope-dependent slip forces
  - Variable cargo mass experiments
- **Phase portrait analysis** — lateral velocity vs. position, Hopf bifurcation characterization
- **Matplotlib animation** — real-time visualization of cargo, ant roles, and trajectory

---

## Physics Model

The cargo is modeled as a rectangular rigid body. At each Gillespie event, the following rates are computed:

| Event | Rate |
|---|---|
| Attachment | R_att = K_on · N_av · N_empty |
| Detachment | R_det = N_att (K¹_off δ(f_loc) + K²_off (1 − δ(f_loc))) |
| Role conversion | R_con = K_c Σ (puller→lifter + lifter→puller terms) |
| Reorientation | R_orient = K_c · N_pullers |
| Forgetting | R_forget = K_forget · N_informed |

Cargo translational and rotational velocity:

$$V_{cm} = \frac{f_0 \sum n_p(\theta_i, \varphi_i)\cos(\theta_i + \varphi_i) - f_{kin}}{\gamma}$$

$$\omega = \frac{f_0 \sum n_p(\theta_i, \varphi_i)\sin(\varphi_i) - \tau_{kin}}{\gamma_{rot}}$$

Puller/lifter probability at attachment is governed by a sigmoid on the local force signal, scaled by the individuality parameter F_ind.

---

## Key Parameters

| Parameter | Value | Description |
|---|---|---|
| F_0 | 2.0 | Per-ant force magnitude |
| F_ind | 15.0 | Individuality parameter |
| K_c | 0.5 s⁻¹ | Basal role-conversion rate |
| K_forget | 0.1 s⁻¹ | Informed-ant forgetting rate |
| β | 0.5 | Friction reduction per lifter |
| γ | 5.0 | Translational damping |
| γ_rot | 40.0 | Rotational damping |
| F_kin | 10.0 | Kinetic friction threshold |
| φ_max | 52° | Max puller angular offset |

---

## Installation

```bash
git clone https://github.com/M-Karthiga/Piano-Movers-problem.git
cd Piano-Movers-problem
pip install numpy matplotlib
```

---

## Usage

Open and run `end_term.ipynb` in Jupyter:

```bash
jupyter notebook end_term.ipynb
```

The notebook contains:
1. Base simulation (obstacle-free, wide corridor)
2. L-shaped wall environment
3. Slope transport with stamina dynamics
4. Phase portrait and arrival time analysis across slope angles

---

## Results

- Cargo successfully navigates narrow corridors through emergent collective steering
- Arrival time scales quadratically with slope angle: T(θ) = 5.89 + 0.025θ + 0.00029θ², R² = 0.994
- Heavier cargo recruits a higher mean puller fraction to compensate
- Phase portraits confirm Hopf bifurcation structure: small groups exhibit random-walk dynamics, large groups show persistent directed motion
 [Final Simulation link ](https://drive.google.com/file/d/114XtbaE0PhLkrdEmfpdR2fAh-z3P28cS/view?usp=sharing)
### Known Limitations

- **Wall tunneling** under Gillespie time evolution when drift velocity is high and the stochastic timestep is large
- **Beam sticking** at walls when wall repulsion lacks a tangential escape component
- Directed transport degrades with very low informed-puller counts (< 3)

---

## References

- Feinerman, O. et al. (2018). *The physics of cooperative transport in groups of ants.* Nature Physics.
- Gelblum, A. et al. (2015). *Ant groups optimally amplify the effect of transiently informed individuals.* Nature Communications.
- Strogatz, S.H. *Nonlinear Dynamics and Chaos.* (Hopf bifurcation analysis)

---

## Authors

| Name | Roll No. |
|---|---|
| Karthiga M | ED23B028 |
| Soumika | ED23B067 |
| Kiruthik | NA23B026 |

*AM 5700 Course Project — IIT Madras*
