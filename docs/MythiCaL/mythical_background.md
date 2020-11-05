---
layout: mythical_default
--- 

# Monte Carlo Charge Transport Simulations (Under Active Development)

In this tutorial, the KMC library is explained and utilized to run a KMC charge transport simulation. 

## Background

Originally, Monte Carlo simulations were used to simulate radiation damage in metals. Since they have been applied to a wide number of applications including charge transport. The effectiveness of kinetic Monte Carlo simulations in capturing the electrical properties of disordered materials was most effectively demonstrated by pioneering work of Bassler in the 1970s. Kinetic Monte Carlo charge transport simulations have since been used to study the electrical properties of conductive polymers, chalcogenide glasses, DNA and liquid crystals. 

In metals the atoms exist in a regular tightly spaced lattice, because the atoms are closely packed they are strongly electronically coupled. This leads to delocalization of the electronic wave function allowing charges to easily propagate through the metal as they are not strongly bound to anyone atom. This is in contrast to transport in conductive disordered organic materials. Take a polymer such as P3HT which is widely studied in the electronic literature. These polymers are disordered, a result of the many conformational degrees of freedom available to the polymer strands as well as the weak binding between molecules. As a result, the polymer strands will exhibit regions of strong electronic coupling where they are tightly packed and weak coupling where they are not. Thus the electronic wave functions will become localized to the strongly coupled regions. Because the wave functions are strongly bound it is difficult for charges to move through the material. Hence, charges move through a thermally activated process where the temperature provides the necessary energy to overcome the local binding energies of the molecules they occupy. 

## Energy landscape

We can model disordered materials by treating the regions that the charges localize as discrete sites. To impart material properties to the sites we can assign energies to the sites based on the electronic density of states. The density of states of disordered organic materials are commonly modeled with a Gaussian distribution where the sigma ranges from 0.05 eV to 0.22 eV depending on the material. 

## Marcus or Miller and Abrahams rate Equations

Before the library can be used you need to determine how the rates are too be calculated. There are two commonly used  rate equations employed for modeling charge transport. The first is the Miller and Abrahams rate equation and the second is the semiclassical Marcus rate equation. 



