# Oosawa Model — Gillespie Implementation

**Question 1:** Write code implementing the Oosawa model using the Gillespie approach.

---

## Overview

This script implements the **Oosawa model** for protein aggregation using the **Gillespie stochastic algorithm**. The Oosawa model describes two fundamental processes: nucleation (slow) and association (fast), which together reproduce the characteristic sigmoidal kinetics observed experimentally in protein misfolding diseases.

The Gillespie algorithm treats each reaction as a discrete stochastic event, making it ideal when molecule numbers are small — exactly the regime where ODEs fail and fluctuations dominate.

---

## The Model

The Oosawa model considers two reactions:

| Reaction | Description | Rate |
|----------|-------------|------|
| **Nucleation** | $n_c$ monomers → one polymer of size $n_c$ | $k_n \cdot m(t)^{n_c}$ |
| **Association** | polymer$(j)$ + monomer → polymer$(j+1)$ | $k_a \cdot m(t) \cdot P(t)$ |

### State variables tracked at every event

| Variable | Meaning |
|----------|---------|
| $m(t)$ | free monomers $= m_0 - M(t)$ |
| $P(t)$ | number of polymers (increases only via nucleation) |
| $M(t)$ | total polymer mass (increases via both reactions) |

---

## The Gillespie Algorithm

For a system with reactions of rate $r_1, r_2, \ldots$:

**Step 1 — Calculate rates**
```
r_nucl  = kn * m(t)^nc       # requires nc monomers to collide
r_assoc = ka * m(t) * P(t)   # each polymer competes for one monomer
r_total = r_nucl + r_assoc
```

**Step 2 — Sample time to next event**

The waiting time between consecutive events follows an exponential distribution:

$$\tau = -\frac{1}{r_{total}} \ln(\text{rand})$$

**Step 3 — Choose which reaction fires**
```
u = random number in [0, 1]
if u < r_nucl / r_total  →  nucleation:   m -= nc,  M += nc,  P += 1
else                      →  association:  m -= 1,   M += 1
```

**Step 4 — Advance time and record state**
```
t += τ
record m(t), M(t), P(t)
```

**Step 5 — Repeat** until $m = 0$ or `max_events` is reached.

---

## Key observations from the output

- **$M(t)$ and $m(t)$ are complementary** — they always sum to $m_0$
- **$P(t)$ grows in steps** — each jump corresponds to a single nucleation event; association does not create new polymers
- **Lag phase → fast growth** — early on, nucleation is slow because it requires $n_c$ monomers to collide simultaneously ($\propto m^{n_c}$); once enough polymers exist, association takes over and $M(t)$ rises sharply
- **Stochastic fluctuations** are visible in $P(t)$ — this is the biologically relevant regime where the ODE approximation breaks down

---

## Parameters

| Parameter | Symbol | Default value | Effect |
|-----------|--------|---------------|--------|
| Nucleation rate constant | $k_n$ | `1e-4` | Controls length of lag phase |
| Association rate constant | $k_a$ | `1e-2` | Controls speed of growth phase |
| Critical nucleus size | $n_c$ | `2` | Exponential effect on nucleation rate |
| Initial monomers | $m_0$ | `200` | Sets total available mass |

All parameters are defined at the top of the script and straightforward to modify.

---

## Background

The Gillespie algorithm (Gillespie, 1977) is the exact stochastic simulation method for well-mixed chemical systems. It is equivalent to solving the Chemical Master Equation, which describes the time evolution of the probability distribution over discrete molecular states. When molecule counts are large, Gillespie results converge to the ODE solution — but for small counts (as during the nucleation lag phase), stochastic fluctuations are essential and ODEs miss the biology.

**Key references:**  
- Gillespie, D.T. (1977). *J. Phys. Chem.*, 81(25), 2340–2361 — original algorithm  
- Oosawa & Asakura (1975) — polymer nucleation model  
- Lecture notes for the course "Physics of Molecular Diseases", Prof. Ala Trusina, Niels Bohr Institute, 2020
