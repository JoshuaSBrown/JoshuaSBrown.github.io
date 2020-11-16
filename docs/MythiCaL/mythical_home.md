---
layout: mythical_home
--- 

# Monte Carlo Coarse-Graining Library (MythiCaL)

Library is designed to coarse grain kinetic Monte Carlo Simulations. Primary motivation for this library is to reduce the number of unnecessary compute cycles.

This library is still under development. 

## The Problem

Though kinetic Monte Carlo simulations are a very powerful tool they can at
times suffer from significant performance hits. This can occur in particular
when a relationship between two events dominates the dynamics. As an
illustration consider an electron moving between molecules. If the rate of
transfer from (molecule A -> molecule B) and (molecule B -> molecule A) is much
greater than the rates to any other molecule C, D, ...etc a signifcant number
of compute cycles will be wasted simply moving the charge back and forth
between these two molecules. This can become such a resource intensive
situation that 99% of the compute cycles are spent moving the charge between
molecule A and B. Thus making simulations of relevant time scales infeasible.
This library attempts to mitigate this problem by coarse graining out these
rapid oscillations during the simulation. 

<img src="/assets/MythicalProblem.png" width="100%" />

## Example Applications

The library is designed to work with rates and could for instance be used with:
 * Monte Carlo Charge Transport Simulations
 * Tracking the spread of disease
 * Modeling the circadian cycle
 
## Dependencies

The library makes use of c++14 features so requires gcc 6 or a compiler with similar support. 

## Chapters

1. [Downloading & Building](./mythical_downloading_building.html)
2. [Importing the library with CMake](./mythical_importing_cmake.html)
3. [Tutorial 1 Charge Transport Background](./mythical_background.html)
4. [Tutorial 1 1D Monte Carlo Charge Transport](./mythical_tutorial_1.html)
5. [Tutorial 2 Time of Flight](./mythical_tutorial_2.html)

## Author List

* Joshua Brown
* Riley Hadjis
