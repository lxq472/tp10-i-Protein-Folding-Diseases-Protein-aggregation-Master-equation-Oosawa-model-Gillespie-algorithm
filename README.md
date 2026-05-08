# Oosawa Aggregation Models (Gillespie Simulations)

## Overview

This project implements stochastic simulations (Gillespie algorithm) of protein aggregation based on the Oosawa model.

We study four cases:

1. Nucleation + Elongation
2. Dissociation
3. Fragmentation
4. Full model

We analyze:

* Aggregate mass M(t)
* Number of aggregates P(t)
* Size distribution f(j)

---

## Physical Interpretation

* Increasing nucleation rate increases the number of aggregates but reduces their average size.
* Increasing elongation rate increases growth speed and aggregate size.
* Dissociation introduces a steady state with finite aggregate sizes.
* Fragmentation leads to autocatalytic growth and a large number of aggregates.

---

## Scientific Insight

Physics-based models allow us to connect microscopic mechanisms (nucleation, growth, fragmentation) with macroscopic observables (mass, number, distributions).

However, different mechanisms can produce similar macroscopic behavior, so reliable conclusions require rich datasets (time evolution + size distributions).

---

## Report Summary

In this project, we simulated protein aggregation using stochastic Gillespie dynamics.

The basic model (nucleation + elongation) produces sigmoidal growth and large aggregates. Adding dissociation introduces a steady state with finite aggregate sizes and exponential distributions. Fragmentation dramatically accelerates aggregation by generating new growth sites, leading to a broader size distribution and a larger number of aggregates.

The full model exhibits a balance between all processes, resulting in a dynamic steady state with stable but broad distributions. The simulations are in qualitative agreement with theoretical expectations and demonstrate how microscopic mechanisms influence macroscopic observables.

---
## 📚 References
* Prof. Ala Trusina, MSc Course: Biophysics of Molecular Diseases, University of Copenhagen, Niels Bohr Institute
