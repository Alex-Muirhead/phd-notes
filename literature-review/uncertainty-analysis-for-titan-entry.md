---
title: Uncertainty Analysis of Radiatve Heating Predictions for Titan Entry
year: ???
---

# Uncertainty Analysis of Radiative Heating Predictions for Titan Entry

## Tags

 - [[tags#Comparison]]
 - [[tags#Uncertainty Analysis]]

## Notes

Entry to titan of HIAD (inflatable) vehicle
Uses LAURA and HARA (latter is non-equilibrium rad. code)
Using surrogate model for Uncertainty Quantification (UQ)
 - Stochastic Expansion (???)
 - Point-collocation Non-Intrusive Polynomial Chaos (NIPC) ???
 - Method has already seen extensive use in aerospace (4 refs provided)

Fluid simulation config
  - Steady state flow
  - Two temperature non-eq model
  - 1500K _supercatalytic_ (???) wall boundary
    - Supercatalytic: "Species mass fractions set to free stream conditions" [3]
  - 21 species composition model
  - 6 km/s, 1.5e-4 kg/m3, 150K
  - Fully laminar (assumed to have negligible effect [2])

Radiation simulation config
  - Using HARA
  - Uses tangent-slab approximation
  - Uses atomic levels & lines from NIST, Opacity Project databases, 
    and atomic bound-free cross-sections from TOPbase
  - Uses Smeared Rotational Band (SRB) spectra, rather than Line By Line (LBL)
    for reduced computational load. This **does** overpredict the surface radiation,
    and only appears correct in optically thin configurations.

Coupling
  - HARA calculates **radiative flux** and **divergence of rad. flux**
  - Included in the flow-field calculations
  - _NOTE: This appears to be a single step coupling i.e. not iterations?_
    - This is incorrect. Since the flow-field is assumed to be **steady-state**,
      the iterative solution is alread present.
  - Ref [1] to work showing coupling effects 
  - HARA uses "collisional radiative" or "non-Boltzmann" approach (???)

## How is this useful?

The results of the UQ gives good insight as to what should be focused on
  - Focus on the flow-field reactions, then heavy-particle impact rates
Give a good test-case of the tangent slab approximation  
References to the impact & magnitude of radiative heating vs convective

[1]: https://arc.aiaa.org/doi/10.2514/1.10304 
"Impact of Flowfield-Radiation Coupling on Aeroheating for Titan Aerocapture"
[2]: https://arc.aiaa.org/doi/10.2514/1.A32254
"Radiative Heating Uncertainty for Hyperbolic Earth Entry, Part 1: Flight Simulation
Modeling and Uncertainty"
[3]: https://fun3d.larc.nasa.gov/session13.pdf#page=22
"Session 13: Thermochemical Nonequilibrium Simulations"

