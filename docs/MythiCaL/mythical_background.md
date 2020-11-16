---
layout: mythical_default
--- 

# Monte Carlo Charge Transport Simulations (Under Active Development)

In this tutorial, the KMC library is explained and utilized to run a KMC charge transport simulation. 

## Background

The earliest known use of Monte Carlo simulations occurred in the 1940s when
they were used to simulate radiation damage in metals. Since, they have been
applied to a wide number of applications and are now commonly used to model
charge transport. The effectiveness of kinetic Monte Carlo simulations in
capturing the electrical properties of disordered materials was most
effectively demonstrated by the pioneering work of Bassler, in the 1980s [1].
Kinetic Monte Carlo charge transport simulations have since been used to study
the electrical properties of small organic molecules [2], conductive polymers
[3], DNA and liquid crystals [4].

In metals, the atoms exist in a regular tightly spaced lattice, because the
atoms are closely packed, they are strongly electronically coupled. This leads
to delocalization of the electronic wave function allowing charges to easily
propagate through the metal as they are not strongly bound to anyone atom which
contrasts with transport in conductive disordered organic materials. Take a
polymer such as P3HT which is widely studied in the electronic literature.
These polymers are disordered, a result of the many conformational degrees of
freedom available to the polymer strands as well as the weak binding between
molecules. As a result, the polymer strands will exhibit regions of strong
electronic coupling where they are tightly packed and weak coupling where they
are not. Thus, the electronic wave functions will become localized to the
strongly coupled regions. It is difficult for charges to propagate through a
material when the wave functions are strongly bound. In such materials, charges
move through a thermally activated process where the temperature provides the
necessary energy to overcome the local binding energies of the molecules they
occupy. 

## Energy landscape

We can model disordered materials by treating the regions that the charges
localize as discrete sites. To impart material properties to the sites we can
assign energies to the sites based on the electronic density of states. The
density of states of disordered organic materials are commonly modeled with a
Gaussian distribution where the sigma ranges from 0.05 eV to 0.22 eV depending
on the material. 

## Marcus or Miller and Abrahams rate Equations

Before the library can be used you need to determine how the rates are too be
calculated. There are two commonly used  rate equations employed for modeling
charge transport. The first is the Miller and Abrahams rate equation and the
second is the semiclassical Marcus rate equation. 

## References

1.	H. Bassler, G. Schonherr, M. Abkowitz, and D. M. Pai, “Hopping transport in prototypical organic glasses,” Phys. Rev. B, vol. 26, no. 6, pp. 3105–3113, 1982.
2.	L. Wang, Q. Li, Z. Shuai, L. Chen, and Q. Shi, “Multiscale study of charge mobility of organic semiconductor with dynamic disorders,” Phys. Chem. Chem. Phys., vol. 12, no. 13, p. 3309, 2010.
3.	R. G. E. Kimber, E. N. Wright, S. E. J. O’Kane, A. B. Walker, and J. C. Blakesley, “Mesoscopic kinetic Monte Carlo modeling of organic photovoltaic device characteristics,” Phys. Rev. B, vol. 86, p. 235206, 2012.
4.	J. Kirkpatrick, V. Marcon, J. Nelson, K. Kremer, and D. Andrienko, “Charge mobility of discotic mesophases: A multiscale quantum/classical study,” Phys. Rev. Lett., vol. 98, p. 227402, Jan. 2007.



