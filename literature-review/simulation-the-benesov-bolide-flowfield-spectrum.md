# Simulation the Benesov bolide flowfield spectrum at altitudes of 47 and 57km

## Tags

  - [[tags#Comparison]]
  - [[tags#Emission]]
  - [[tags#LAURA]]
  - [[tags#HARA]]
  - [[tags#GPU]]
  - [[tags#Ray Tracing]]

## Notes

Entry of 1991 Benesoz meteor (fragment cloud) into upper atmosphere
Uses LAURA solver for fluid dynamics, HARA for radiation
Fluid simulation config
 - Assumes laminar flow through shock layer and wake
 - Lack of models that "accurately account for turbulent species diffusion"
 - Above is the primary impact of turbulence of radiative heating
 - Negative ions (i.e. O2-) found to have negligible impact on flowfield & 
   emitted radiation (interesting!). Impact is higher for lower velocities
 - Park two-temperature model
 - 20 km/s, 1.43e-3 kg/m3

Found that chemical noneq. is significantly more important than thermal noeq. for
modelling fhe flowfield
Radiation coupling **very** important in the wake, massively reduces temperatures
  - _NOTE: I didn't expect it to reduce in the wake_

HARA coupled solver steps:
  1. Calculate electronic state populations (quasi-steady state approx.)
  2. Compute the absorbtion and emission spectrum
  3. Solve the RTE using either tangent-slab or infinite cylinder approximations
  4. Send divergence of radiative flux back to fluid solver
  5. Repeat until flow-field convergence

Atomic line transitions were simulated on GPU
  - Unsure if this is just generating the spectra, or computing the radiative flux

Uses ray-tracing to compute ground-impact radiation

