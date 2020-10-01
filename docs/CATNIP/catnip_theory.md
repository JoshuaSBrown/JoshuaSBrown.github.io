
**WARNING**

**This is a work in progress**

**DISCLAIMER**

**1. In this section I hope to clarify what and how the transfer integral is calculated. Be aware that I am presenting my understanding, I will try my best to be accurate, but the reader is always advised to refer to peer-reviewed literature. I would recommend that the reader read through the papers by Baumeier and Valeev. 2. If you notice inconsistencies in your understanding and the theory presented here please contact me so that it can be corrected or the documentation can be made clearer.**

# Theory

## Introduction

The charge transfer integral is written mathematically as:

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: [
      "MathMenu.js",
      "MathZoom.js",
      "AssistiveMML.js",
      "a11y/accessibility-menu.js"
    ],
    jax: ["input/TeX", "output/CommonHTML"],
    TeX: {
      extensions: [
        "AMSmath.js",
        "AMSsymbols.js",
        "noErrors.js",
        "noUndefined.js",
      ]
    }
  });
</script>

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


<p align=center> 
$$J = \langle \Psi_A | \hat{H} | \Psi_B \rangle$$ (1)
</p>

Here, &Psi;<sub>A</sub> and &Psi;<sub>B</sub> represent the wavefunctions of two different quantum states which are orthonormal and &Hcirc; represents the Hamiltonian operator. J represents the electronic coupling between states A and B. The larger the coupling the larger the probability that a charge in state A will move to state B and vice versa.


There are different methods for calculating the charge transfer integrals that make use of various approximations, these include the energy splitting in dimer method among others [1].

The calculation for the DIPRO method, from my understanding was first clearly articulated by E. Valeev in 2006 [2]. It was developed to help address the failure of the energy splitting in dimer method to account for polarization effects. 

## Energy Splitting in Dimer Method

It would be helpful at this point to explain what the energy splitting dimer method does. It makes a few important assumptions:

1. _the two molecules are chemically symmetric_

2. _the two molecules are spatially symmetric_

Now if we remember that when we bring two molecules together the Pauli exclusion principle states that no more than two electrons can occupy the same orbital and hence the orbitals will split. Take the simple case of two Helium atoms that approach one another, at a distance they will each have a single occupied orbital. As the atoms approach the orbital will split into two orbitals, the highest occupied molecular orbital (HOMO) and HOMO-1 of the dimer.

![]({{ "/assets/HeliumBonding.svg" | absolute_url }})<!-- .element height="60%" width="60%" -->

The magnitude of the split in energy is a result of the energy level of the HOMO of each of the Helium atoms which we will call the site energy. The second contribution arises from the interaction of the two HOMO levels. It is this interaction that we call the transfer integral. In this simple case because both atoms are identical, they will have the same site energies. This means that the split between the HOMO and HOMO-1 is purely a result of the transfer integral. 

$$ J = \frac{ E_{HOMO} - E_{HOMO-1} }{2} $$ (2)

Here, $$E_{HOMO}$$ and $$E_{HOMO-1}$$ represent the molecular orbital energies of the HOMO and HOMO-1. 

As powerful as this technique is for calculating the transfer integral its limitations are very apparent. Say you have two molecules that are not chemically and spatially symmetric, the dimer splitting method in this case cannot resolve the correct contributions from the site energies and transfer integrals as you cannot assume the site energies and or the transfer integrals will be equal. Furthermore, note that we have assumed that the HOMO and HOMO-1 energy split is purely a result of the interaction of the HOMO orbitals of the two isolated Helium atoms. This is also a limitation. For instance, if you start with an isolated molecule which has a HOMO and HOMO-1 that are close in energy, when the molecule forms a dimer both orbitals of the HOMO and HOMO-1 may interact it will then be difficult to correctly resolve which contributions to the Pauli splitting arise from which transfer integrals. 

## Accounting for Polarization

The starting point for finding the transfer integrals of a system of molecules is to solve the Shr&uodinger equation for equation 1 shown at the top. This can be done, but we need some idea of what the quantum states $$\psi_A$$ and $$\psi_B$$ should look like. To help illustrate what I mean consider the exchange reaction. 

$$K^+ + K \rightleftharpoons K + K^+$$ (3)

We have the state of the charge potassium and the state of the uncharged potassium. At some point these atoms come into proximity and become electronically coupled. It is the transition at this point that we want to calculate the transfer integral.

<p align=center> 
| State A	| State B	|
| ------- | ------- |
| $$K^+$$	| $$K$$   |
</p>

For state $$A$$, we will use the wavefunction $$\psi_A$$ associated with the highest **unoccupied** orbital of the positively charged potassium atom. For state $$B$$, we will use the wavefunction $$\psi_B$$ associated with the highest **occupied** orbital of the neutral potassium atom. 
At this point, we know what wave functions we need but to calculate them we can use Quantum Chemistry calculations. Running Quantum Chemistry calculations on the neutral and positively charged potassium atoms in isolation will yield the diabatic states, where the orbitals of interest are associated only with their respective atoms.
At this point we have everything we need to solve equation 1, we just need to find an appropriate Hamiltonian operator and do a substantial amount of math (Don’t worry we will use a trick from the Baumeier paper to get around most of the more involved math). There is one problem that remains, because most Quantum Chemical calculations will generate quantum states from a linear combination of atomic orbitals (LCAO). These states will not by default be orthonormal to one another and thus the transfer integral is not quite right.

## Basis sets and orthonormalization

To understand the next couple of steps it is essential that the reader has a thorough grasp of what a basis set is and why orthonormalization is important. The basis set describes the degrees of freedom that are available for describing our system. In Quantum Chemistry calculations, we are interested in describing the electrons. Each electron is defined by a wavefunction. In time independent quantum chemistry, this wavefunction will be described in the 3 dimensions of space. If we are smart we can approximate the shape of the wavefunction closely using algebraic functions that are easier to manipulate and also closely mimic the shape of the actual wavefunction, these functions are what we call the basis functions. Other approximations can also be used to limit the number of basis functions needed to describe a particular system but I digress. 
The point is the basis functions represent the degrees of freedom that are available in a Quantum Chemistry calculation for describing the electronic system. 
Orthonormalization of the molecular orbitals is the mathematical means used to enforce the correct physics of treating electrons as fermions which follow the Pauli exclusion rules. 

# References

1.  J. Kirkpatrick, “An approximate method for calculating transfer integrals based on the ZINDO Hamiltonian,” Int. J. Quantum Chem., vol. 108, no. 1, pp. 51–56, 2008.
2.  E. F. Valeev, V. Coropceanu, D. a da Silva Filho, S. Salman, and J.-L. Brédas, “Effect of electronic polarization on charge-transport parameters in molecular organic semiconductors.,” J. Am. Chem. Soc., vol. 128, no. 30, pp. 9882–6, Aug. 2006.
