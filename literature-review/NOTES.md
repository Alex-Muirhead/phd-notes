# Literature Review Notes

Here are some keywords that appear to be useful / common:
 - Radiative Heating
 - Computational Fluid Dynamics Simulation
 - Shock Layers

## Uncertainty Analysis of Radiatve Heating Predictions for Titan Entry

**Tags:** [[tags#Comparison]], [[tags#Uncertainty Analysis]]

 - Entry to titan of HIAD (inflatable) vehicle
 - Uses LAURA and HARA (latter is non-equilibrium rad. code)
 - Using surrogate model for Uncertainty Quantification (UQ)
   - Stochastic Expansion (???)
   - Point-collocation Non-Intrusive Polynomial Chaos (NIPC) ???
   - Method has already seen extensive use in aerospace (4 refs provided)
 - Fluid simulation config
   - Steady state flow
   - Two temperature non-eq model
   - 1500K _supercatalytic_ (???) wall boundary
     - Supercatalytic: "Species mass fractions set to free stream conditions" [3]
   - 21 species composition model
   - 6 km/s, 1.5e-4 kg/m3, 150K
   - Fully laminar (assumed to have negligible effect [2])
 - Radiation simulation config
   - Using HARA
   - Uses tangent-slab approximation
   - Uses atomic levels & lines from NIST, Opacity Project databases, 
     and atomic bound-free cross-sections from TOPbase
   - Uses Smeared Rotational Band (SRB) spectra, rather than Line By Line (LBL)
     for reduced computational load. This **does** overpredict the surface radiation,
     and only appears correct in optically thin configurations.
 - Coupling
   - HARA calculates **radiative flux** and **divergence of rad. flux**
   - Included in the flow-field calculations
   - _NOTE: This appears to be a single step coupling i.e. not iterations?_
     - This is incorrect. Since the flow-field is assumed to be **steady-state**,
       the iterative solution is alread present.
   - Ref [1] to work showing coupling effects 
   - HARA uses "collisional radiative" or "non-Boltzmann" approach (???)
 - **How is this useful?**
   - The results of the UQ gives good insight as to what should be focused on
     - Focus on the flow-field reactions, then heavy-particle impact rates
   - Give a good test-case of the tangent slab approximation  
   - References to the impact & magnitude of radiative heating vs convective

[1]: https://arc.aiaa.org/doi/10.2514/1.10304 
"Impact of Flowfield-Radiation Coupling on Aeroheating for Titan Aerocapture"
[2]: https://arc.aiaa.org/doi/10.2514/1.A32254
"Radiative Heating Uncertainty for Hyperbolic Earth Entry, Part 1: Flight Simulation
Modeling and Uncertainty"
[3]: https://fun3d.larc.nasa.gov/session13.pdf#page=22
"Session 13: Thermochemical Nonequilibrium Simulations"

## Simulation the Benesov bolide flowfield spectrum at altitudes of 47 and 57km
 - Entry of 1991 Benesoz meteor (fragment cloud) into upper atmosphere
 - Uses LAURA solver for fluid dynamics, HARA for radiation
 - Fluid simulation config
    - Assumes laminar flow through shock layer and wake
    - Lack of models that "accurately account for turbulent species diffusion"
    - Above is the primary impact of turbulence of radiative heating
    - Negative ions (i.e. O2-) found to have negligible impact on flowfield & 
      emitted radiation (interesting!). Impact is higher for lower velocities
    - Park two-temperature model
    - 20 km/s, 1.43e-3 kg/m3
  - Found that chemical noneq. is significantly more important than thermal noeq. for
    modelling fhe flowfield
  - Radiation coupling **very** important in the wake, massively reduces temperatures
    - _NOTE: I didn't expect it to reduce in the wake_
  - HARA coupled solver steps:
    1. Calculate electronic state populations (quasi-steady state approx.)
    2. Compute the absorbtion and emission spectrum
    3. Solve the RTE using either tangent-slab or infinite cylinder approximations
    4. Send divergence of radiative flux back to fluid solver
    5. Repeat until flow-field convergence
  - Atomic line transitions were simulated on GPU
    - Unsure if this is just generating the spectra, or computing the radiative flux
  - Uses ray-tracing to compute ground-impact radiation

## Radiative Heating on the After-Body of Martian Entry Vehicles
 - Doesn't appear to use a **coupled** radiation model / weak coupling
 - Radiation is dominated by CO2 bands in the midwave infrared
 - Tangent slab shown to be overly conservative (in excess) by as much as 58%
   - This appears to be for the backbody, rather than frontbody
   - Complex wake-flows mean the tangent slab isn't always an upper limit
 - Details on HARA (NEQAIR alternative)
   - Line-by-line approach used for **atoms** and **optically thick molecules**
   - Smeared rotation band approach used for **optically thin molecules**
 - NEQAIR allows for reduced database by _pseudo-continuum_ approach
 - Shows the assymmetric distribution of radiant intensity at various points 
   on the capsule. 
   - Highlights how the tangent slab assumption doesn't apply well to these points. 
   - Only applies well (within 10%) to the stagnation line and aft-centerline
 - Required full angular integration resolution of ~1100 points
 - Radiance takes a **long** distance to drop to zero in the wake! ~20m

## Three-Dimensional Radiation Ray-Tracing for Shock-Lyaer Radiative Heating Simulations
 - Simulation is still coupled through tangent slab method between HARA and LAURA
 - Ray-tracing is done as a post-procesing step on surface-points
 - Goes into detail about the methods of ray-tracing and interpolation of cell data
 - Restricted to hex structured grids
 - Simulation configuration (FireII aeroshell):
   - Trajectory point at time 1636s
   - u = 11.3km/s
   - rho = 8.57E-05 kg/m3
   - T = 210K
   - 11 species air mix (mostly Nitrogen and Oxygen w/ electrons)

## Flow-field/Radiation Coupling Analysis for Huygens Probe Entry into Titan atmosphere
 - Uses the PARADE spectral properties code
   - Combined with TINA (flow solver) and HERTA (radiative transfer code)
   - Finally outside of NASA!
 - Cassini-Huygens mission, Saturn system, Titan moon entry
   - Blunted nose conical aeroshell design, 2.7m diameter
   - Atmosphere composition: 95% N2, 2% Ar, 3% CH4
   - CO2 not present
 - Simulation configuration
   - Steady state **viscous** flow field (not laminar!)
   - Fully catalytical walls (see [3])
   - Radiative emission and absorption based on vibrational-electronic energy
   - Radiation treated as an _explicit source term_
 - Radiation transfer solver
   - 1-dimensional:
     - Integration along the stagnation line
     - PARADE performs a routine to integrate flux using _thin-slab approximation_
   - 2-dimensional (axisymmetric)
     - HETRA code uses Monte-Carlo Method 
     - Used for calibration of corrective factors
 - Used to create a conversion factor from 1D (thin-slab / tangent slab) to 3D

## Non-equilibrium radiation modeling in a gas kinetic simulation code
 - Uses Monte-Carlo to solve the RTE equation
 - Fluid solver (PICLas) solves the **boltzmann** equation for fluid interactions, and
   implements a Direct Simulation Monte-Carlo solver (DSMC) for the collision term
    - This makes it significantly easier to integrate a monte-carlo radiation solver,
      giving the existing modules
    - The remaining fields and fluid forces are solved in a "self consistent manner"
      over a tetra-grid. Suggests a more traditional FVM / FEM approach?
    - Has the ability to track _particles_ through the domain, which was adapted to 
      ray-tracing. Could the same be done for Eilmer (i.e. adding in particles)?
 - Methods for selecting initial position
    1. Fixed energy for each _photon bundle_, and the total number of bundles over the
       entire grid is predetermined. The number of bundles per cell is then proportional
       to the radiative energy emission within the cell. 
    2. Fixed number of photon bundles _per cell_, and the energy of each bundle is 
       proportional to the radiative energy emission within the cell.
    - Different from choosing cell location based on distribution of temperature, but how
      much of a difference?
      - Could my algorithm be tweaked for an assumption of multiple bundles per cell?
 - Uses the deterministic energy absorption method (i.e. dumping in all cells along path
   using an exponential calc. based on path length and absorption).
 - Initial orientation uses a uniform distribution of direction vectors $s$ in a spiral,
   which is then rotated by a random vector. 
