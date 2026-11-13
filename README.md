This repository contains codes developed to implement and extend the ES1 code described in *Plasma Physics via Computer Simulation* by C. K. Birdsall and A. B. Langdon.

The repository serves as a pedagogical foundation for developing a hybrid Particle-in-Cell (PIC) solver, ultimately intended for applications in Hall-effect thrusters. The next stage of development is to restructure the implementation into a modular codebase, which will be followed by converting the solver to FORTRAN and adapting its architecture for parallel computing.

## `es1_v1.ipynb`

`es1_v1` is my first Python implementation of Birdsall's ES1 code, originally written in FORTRAN. It considers the classical problem of small-amplitude cold plasma oscillations as an initial test case, with the numerical results compared against the analytical solution.

Electrons are initially distributed uniformly across a one-dimensional periodic domain and given a small sinusoidal displacement from their equilibrium positions. The resulting charge-density perturbation generates an electric field, which drives the plasma oscillations.

### Numerical methods

The implementation follows the standard Eulerian–Lagrangian PIC framework, coupling particle motion to fields defined on a spatial grid.

- Cloud-in-Cell (CIC): Particle charges are distributed between the two nearest grid points using linear weighting to obtain the grid-based charge density. A stationary neutralising ion background is represented by removing the mean charge density. The grid-based electric field is interpolated to particle positions using the same linear weighting scheme.

- Spectral Poisson solver: Poisson's equation is solved in Fourier space using the Fast Fourier Transform (FFT). The electric field is subsequently obtained from the potential through spectral differentiation.

- Leapfrog integration: Particle velocities and positions are advanced using the leapfrog method, with velocities staggered by half a timestep relative to positions. The initial half-step velocity is appropriately initialised.

### Validation

The simulated particle displacement is compared with the linear cold-plasma analytical solution:

$$
x_i(t) - x_{i,0} = \delta x_0 \cos(x_{i,0}) \cos(\omega_p t)
$$

where $\delta x_0$ is the initial perturbation amplitude and $\omega_p$ is the electron plasma frequency.

The numerical mode amplitude is extracted by projecting the particle displacements onto the initial spatial mode. Its temporal evolution is then compared with the analytical prediction. Kinetic energy, electric-field energy and total energy are also recorded as diagnostics.
