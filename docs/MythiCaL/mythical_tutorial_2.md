---
layout: mythical_default
--- 

# Overview

In the previous simulation, I showed how to simulate a single charge
moving along a 1d lattice. In this tutorial, we will expand on what we
did previously but with several improvements. 

1. We will use a 3d lattice, with periodic boundaries on the y and z directions
2. We will use the Marcus rate equation with the Gaussian Disorder model to generate our rates
3. We will simulate several charges as opposed to just 1
4. We will show how to calculate the current transient

# 1. Creating a cubic lattice 

<img src="/assets/mythical_tutorial_II_a.jpg" margin="40px" width="100%" />

Here we demonstrate a cubic lattice of sites. A normal or Gaussian distribution
is then used to assign energies randomly to each of the sites in the lattice.
Thank fully the c++ library comes with a very nice random number generator. To
create our lattice we will begin by creating an object called Lattice that is
able to store the sites. Ok, I know it looks a bit overwhelming but really its
function is quite simple. This class essentially given a three dimensional
lattice point defined by x, y and z will simply return a unique identifier. It
will also do the opposite, given the unique id it will turn the position on the
lattice as a vector. The only other thing the class does is assign energies
randomly to a lattice based on a Normal distribution. 
