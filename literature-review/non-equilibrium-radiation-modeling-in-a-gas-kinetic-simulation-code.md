# Non-equilibrium radiation modeling in a gas kinetic simulation code

## Tags

  - [[tags#Transport]]

## Notes

Uses Monte-Carlo to solve the RTE equation
Fluid solver (PICLas) solves the **boltzmann** equation for fluid interactions, and implements a Direct Simulation Monte-Carlo solver (DSMC) for the collision term

  - This makes it significantly easier to integrate a monte-carlo radiation solver,
    giving the existing modules
  - The remaining fields and fluid forces are solved in a "self consistent manner"
    over a tetra-grid. Suggests a more traditional FVM / FEM approach?
  - Has the ability to track _particles_ through the domain, which was adapted to 
    ray-tracing. Could the same be done for Eilmer (i.e. adding in particles)?

Methods for selecting initial position

 1. Fixed energy for each _photon bundle_, and the total number of bundles over the entire grid is predetermined. The number of bundles per cell is then proportional to the radiative energy emission within the cell. 
 2. Fixed number of photon bundles _per cell_, and the energy of each bundle is proportional to the radiative energy emission within the cell.
 - Different from choosing cell location based on distribution of temperature, but how much of a difference?
   - Could my algorithm be tweaked for an assumption of multiple bundles per cell?

Uses the deterministic energy absorption method (i.e. dumping in all cells along path using an exponential calc. based on path length and absorption).
Initial orientation uses a uniform distribution of direction vectors $s$ in a spiral, which is then rotated by a random vector. 
