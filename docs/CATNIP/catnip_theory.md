
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
$$J = \langle \Psi_A | \hat{H} | \Psi_B \rangle$$
</p>

Here, $$\Psi_A$$ and $$\Psi_B$$ represent the wavefunctions of two different quantum states which are **orthogonal** and $$\hat{H}$$; represents the Hamiltonian operator. $$J$$ represents the electronic coupling between states $$A$$ and $$B$$. The larger the coupling the larger the probability that a charge in state $$A$$ will move to state $$B$$ and vice versa.


There are different methods for calculating the charge transfer integrals that make use of various approximations, these include the energy splitting in dimer method among others [1].

The calculation for the DIPRO method, from my understanding was first clearly articulated by E. Valeev in 2006 [2]. It was developed to help address the failure of the energy splitting in dimer method to account for polarization effects. 

## Energy Splitting in Dimer Method

It would be helpful at this point to explain what the energy splitting dimer method does. It makes a few important assumptions:

1. _the two molecules are chemically symmetric_

2. _the two molecules are spatially symmetric_

Now if we remember that when we bring two molecules together the Pauli exclusion principle states that no more than two electrons can occupy the same orbital and hence the orbitals will split. Take the simple case of two Helium atoms that approach one another, at a distance they will each have a single occupied orbital. As the atoms approach the orbital will split into two orbitals, the highest occupied molecular orbital (HOMO) and HOMO-1 of the dimer.

![]({{ "/assets/HeliumBonding.svg" | absolute_url }})<!-- .element height="60%" width="60%" -->

The magnitude of the split in energy is a result of the energy level of the HOMO of each of the Helium atoms which we will call the site energy. The second contribution arises from the interaction of the two HOMO levels. It is this interaction that we call the transfer integral. In this simple case because both atoms are identical, they will have the same site energies. This means that the split between the HOMO and HOMO-1 is purely a result of the transfer integral. 

$$ J = \frac{ E_{HOMO} - E_{HOMO-1} }{2} $$

Here, $$E_{HOMO}$$ and $$E_{HOMO-1}$$ represent the molecular orbital energies of the HOMO and HOMO-1. 

As powerful as this technique is for calculating the transfer integral its limitations are very apparent. Say you have two molecules that are not chemically and spatially symmetric, the dimer splitting method in this case cannot resolve the correct contributions from the site energies and transfer integrals as you cannot assume the site energies and or the transfer integrals will be equal. Furthermore, note that we have assumed that the HOMO and HOMO-1 energy split is purely a result of the interaction of the HOMO orbitals of the two isolated Helium atoms. This is also a limitation. For instance, if you start with an isolated molecule which has a HOMO and HOMO-1 that are close in energy, when the molecule forms a dimer both orbitals of the HOMO and HOMO-1 may interact it will then be difficult to correctly resolve which contributions to the Pauli splitting arise from which transfer integrals. 

## Accounting for Polarization

The starting point for finding the transfer integrals of a system of molecules is to solve the Shr&ouml;dinger equation for equation 1 shown at the top. This can be done, but we need some idea of what the quantum states $$\Psi_A$$ and $$\Psi_B$$ should look like. To help illustrate what I mean consider the exchange reaction. 

$$K^+ + K \rightleftharpoons K + K^+$$

We have the state of the charge potassium and the state of the uncharged potassium. At some point these atoms come into proximity and become electronically coupled. It is the transition at this point that we want to calculate the transfer integral.

| State A	| State B	|
| ------- | ------- |
| $$K^+$$	| $$K$$   |

For state $$A$$, we will use the wavefunction $$\Psi_A$$ associated with the highest **unoccupied** orbital of the positively charged potassium atom. For state $$B$$, we will use the wavefunction $$\Psi_B$$ associated with the highest **occupied** orbital of the neutral potassium atom. 
At this point, we know what wave functions we need but to calculate them we can use Quantum Chemistry calculations. Running Quantum Chemistry calculations on the neutral and positively charged potassium atoms in isolation will yield the diabatic states, where the orbitals of interest are associated only with their respective atoms.
At this point we have almost everything we need to solve equation 1, we just need to find an appropriate Hamiltonian operator and do a substantial amount of math (Don’t worry we will use a trick from the Baumeier paper to get around most of the more involved math). There is one problem that remains, because most Quantum Chemical calculations will generate quantum states from a linear combination of atomic orbitals (LCAO). These states will not by default be **orthogonal** to one another as there will be overlap between the basis set of $$\Psi_A$$ and $$\Psi_B$$ and thus when we use equation 1 to calculate the transfer integral the value we calculate will not be quite right. 

### Basis sets and Orthogonalization

To understand the next couple of steps it is essential that the reader has a thorough grasp of what a **basis set** is and why **orthogonalization** is important. The **basis set** is a set of functions. In Quantum Chemistry calculations, we are interested in solving the Shr&ouml;dinger equation for systems of atoms. The wavefunction of our system can be represented with a vector of coefficients, where the coefficients represent the weight of each **basis function**. The larger the basis set is the easier it is to accurately represent the exact wavefunction.

To find an accurate solution, the basis functions must **span** the function space. It is easier to illustrate this in terms of a **vector space**. Imagine that you want to describe movement in 3 dimensions, and you have a total of 3 vectors $$u$$, $$v$$ and $$w$$, but all three vectors lie on a plane (light blue triangle). These vectors would not be **linearly independent**, and it would only be possible to describe movement along the plane. To **span** the **vector space** at least one of the three vectors must extend above or below the plane, as is shown in the second image where $$v$$ is now moved to the $$v'$$ position. In the second picture, you can describe movement in 3 dimensions, albeit you cannot say that $$u$$ **uniquely** describes movement in the x direction because all three vectors have an x component. 

<img src="/assets/LinearIndependentVec.svg" width="100%" />

We can take this picture one step further and ensure that each vector describes a **unique** direction by moving $$v$$ to the $$v''$$ position and $$w$$ to the $$w'$$ position (in other words they are made perpendicular), then we can say that the vectors are **orthogonal**. When we **orthogonalize** the vectors it becomes clear that each vector is **uniquely** associated with movement in a particular dimension. This same picture holds true for **basis functions** in the **function space**.

There are different ways of orthogonalizing a basis set. Again, I will use vector space to illustrate the difference. The image below depicts a vector basis set made up of two vectors that are not othogonal to one another. One possible way to make them orthogonal is to choose one of the vectors and move the next vector such that it is at a 90 degree angle. If we had a third vector we could then move that vector and make sure that it was in turn orthogonal to the other two vectors. This orthgonalization technique is called **Grahm Schmidt** orthogonalization. Another technique would be to move both vectors in a symmetric manner from their starting position this is known as **Symmetric** orthogonalization, there is another way to do this called **Cannonical** orthogonalization but I digress. 

<img src="/assets/Orthogonalization.svg" width="100%" />



# References

1.  J. Kirkpatrick, “An approximate method for calculating transfer integrals based on the ZINDO Hamiltonian,” Int. J. Quantum Chem., vol. 108, no. 1, pp. 51–56, 2008.
2.  E. F. Valeev, V. Coropceanu, D. a da Silva Filho, S. Salman, and J.-L. Brédas, “Effect of electronic polarization on charge-transport parameters in molecular organic semiconductors.,” J. Am. Chem. Soc., vol. 128, no. 30, pp. 9882–6, Aug. 2006.
  
