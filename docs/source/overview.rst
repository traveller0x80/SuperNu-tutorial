SuperNu Overview
=============

Summary of SuperNu Physics
--------------------------

SuperNu is a program for simulating time-dependent non-linear radiative transfer interacting in matter. It applies the methods of Implicit Monte Carlo (IMC) [1] and Discrete Diffusion Monte Carlo (DDMC) [2] for static or homologously expanding spatial grids. The radiation field affects the internal energy and matter state but does not affect the motion of the fluid. SuperNu may be applied to simulate radiation transport for supernovae with ejecta velocities that are not affected by radiation momentum [3]. SuperNu is motivated by the ongoing research into the effect of variation in supernova and kilonova models on observables: the brightness and shape of light curves and the temporal evolution of the spectra. Consequently, the code may be used to post-process data from hydrodynamic simulations. SuperNu does not include any capabilities or methods that allow for non-trivial hydrodynamics.

.. line-block::
   
    [1] JA Fleck Jr, JD Cummings Jr, "An implicit Monte Carlo scheme for calculating time and frequency dependent nonlinear radiation transport", (1971)
    [2] JD Densmore, TJ Urbatsch, TM Evans, MW Buksas, "A hybrid transport-diffusion method for Monte Carlo radiative-transfer simulations", (2007)
    [3] RT Wollaeger, DR van Rossum, "Radiation Transport for Explosive Outflows: Opacity Regrouping", (2014)


Basic Workflow
--------------

Insert some diagrams exlaining workflow


Code Features
-------------

The time-dependent equations for photon transport and diffusion are solved with Monte Carlo (MC) by discretizing the radiation energy in the system in the form of MC packets (particles).
These particles are transported over the computational domain (spatial grid) where they interact with the gas according to multi-frequency opacities (e.g. multigroup discrete opacities).
Particles that leave the domain are tallied per timestep and flux wavelength bin, and written to disk.

TODO: update the following.

**Particles**:
    - MC Particles are generated in locations where energy sources are non-zero.
    - Energy sources include:
        - boundary and volume sources,
        - manufactured-solution sources,
        - tabular sources,
        - inline energy deposition by radioactive decay.
    - Particles that get censused at the end of a timestep are stored in the particle array.

**Transport**:
    - Particle transport methods are:
        - Implicit Monte Carlo (IMC),
        - Discrete Diffusion Monte Carlo (DDMC).
    - DDMC is used in optically thick regions of a domain.
    - Geometries:
        - 1D,3D spherical,
        - 2D,3D cylindrical,
        - 3D cartesian.
    - particle transport is parallelized using a hybrid of MPI and OpenMP

**Spatial grid**:
    - The grid is the domain over which MC particles are tracked.
    - The grid can either be a spatial or a velocity mesh.
    - If the grid is physically moving:
        - the grid has units of velocity,
        - the velocity grid is not affected by the radiation field,
        - the velocity grid is constant.
    - If the grid is physically static:
        - the grid is in units of length,
        - the domain is static (no velocity field).

Gas:
    - Gas properties are domain decomposed.
    - Gas properties are updated after transport steps.
    - Gas properties include:
        - chemical composition,
        - density,
        - material temperature,
        - heat capacity,
        - opacity: Thomson scattering, and multi-frequency absorption.
    - Leakage (DDMC) and Planck opacities are calculated from scattering and absorption opacities.
    - Mutli-group absorption opacity includes bound-bound (bb), bound-free (bf), and free-free (ff) data:
        - line data for bound-bound opacities are taken from http://kurucz.harvard.edu/atoms.html.

**Groups**:
    - By default opacity frequency dependence is discretized with multi-group.
        - The frequency-resolved opacities are averaged within each group.

**IO**:
    - input is divided in two categories:
        - model specific input files are named input.* and (see the Input/ directory):
            - input.par: runtime parameters,
            - input.str: velocity-density-composition structure on the computational domain.
        - model independent data files are named data.* (see the Data/ directory):
            - data.bf_verner: bound-free cross section data (Verner et al. 1996, ApJ 465, 487)
            - data.ff_sutherland: free-free gaunt factor data (Sutherland 1998, MNRAS 300, 321)
            - data.ion: atomic level data (from http://kurucz.harvard.edu/atoms.html)
            - Atoms/data.atom.* bound-bound transition data
    - output files are named output.*
        - stdout is written to output.log unless disabled in by an input parameter.
        - flux variables are saved as output.flx_*
        - grid variables are saved as output.grd_*
        - total (integrated over the domain) energy budget numbers are saved as output.tot_energy


SuperNu Output
--------------

SuperNu files generated from simulations include the following.

**Flux**:
SuperNu writes flux output in ascii (output.flx_luminos) as a sequence of spectra, one spectrum per line, one line per viewing angle, repeated in each time step.  So the number of columns equals the number of wl-bins, and the number of rows equals nmu*nphi*ntimestep, where mu=cos(theta).  The luminos values are in units [erg/s].  The 'output.flx_grid' file describes the wavelength, viewing angle, and time bins.

**Energy totals**:
SuperNu writes output of energy totals in ascii (output.tot_energy) each time step, with each column corresponding to a particular energy value.

The first column is energy conservation error.  These values can be used to find unintended energy leaks or sources in simulations.

**Grid-based variables**:
Using parameter in_io_nogriddump, SuperNu optionally writes grid-based variables in ascii (output.grd_*).

Grid variables include material temperature (output.grd_temp), radiation energy density (output.grd_eraddens), and Planck opacity (output.grd_capgrey).

The grid variables can be mapped to spatial cells (with an (i,j,k) index) with output.grd_grid.

As headers, output.grd_grid has (in row order) the geometry index (grd_igeom), the number of spatial cells along each dimension (grd_nx,grd_ny,grd_nz),
and the total number of array cells used to yield an optimal row size (ncpr), column size (nrow) to minimize padding cells per write
(the final header has nrow*ncpr,nrow,ncpr).

The next 3 rows are space or velocity values at the cell edges in the x,y,z dimensions.

The following integers are the cell indices of grid variables for spatial index locations (i,j,k), where the column index is the x-dimension (i),
and the row index is the serialized index for the y (j) and z (k) dimensions (the row index is j+(k-1)*ny).

Thus, the cell padding information can be used to remove extra padding values from the grid output, and the grid index mapping to the (i,j,k)
spatial cell index can used to reconstruct spatial profiles of the padding-stripped grid data.

