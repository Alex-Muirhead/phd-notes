# Flow-field/Radiation Coupling Analysis for Huygens Probe Entry into Titan atmosphere

## Tags

  - [[tags#Comparison]]
  - [[tags#Transport]]

## Notes

Uses the PARADE spectral properties code
  - Combined with TINA (flow solver) and HERTA (radiative transfer code)
  - Finally outside of NASA!

Cassini-Huygens mission, Saturn system, Titan moon entry
  - Blunted nose conical aeroshell design, 2.7m diameter
  - Atmosphere composition: 95% N2, 2% Ar, 3% CH4
  - CO2 not present

Simulation configuration
  - Steady state **viscous** flow field (not laminar!)
  - Fully catalytical walls (see [3])
  - Radiative emission and absorption based on vibrational-electronic energy
  - Radiation treated as an _explicit source term_

Radiation transfer solver
  - 1-dimensional:
    - Integration along the stagnation line
    - PARADE performs a routine to integrate flux using _thin-slab approximation_
  - 2-dimensional (axisymmetric)
    - HETRA code uses Monte-Carlo Method 
    - Used for calibration of corrective factors

Used to create a conversion factor from 1D (thin-slab / tangent slab) to 3D
