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

# 1. Creating a Cubic Lattice 

<img src="/assets/mythical_tutorial_II_a.jpg"  width="100%" style="border:30px solid white" />

Here we demonstrate a cubic lattice of sites. MythiCaL carries with it for 
convenience a class specefically for this purpose which can easily be imported by
including the right header file. 

```c++
#include <mythical/charge_transport/cubic_lattice.hpp>
```

Once the header is imported we can define out lattice. Below we have defined
a lattice 100 by 80 by 50 sites where the spacing between each of the sites is
a single nm and our y and z boundaries are periodic. 

```c++ 
const int wid = 80;
const int hei = 50;
const double inter_site_dist = 1; // nm
const int total_num_sites = len * wid * hei; 
myct::BoundarySetting x_b = myct::BoundarySetting::Fixed;
myct::BoundarySetting y_b = myct::BoundarySetting::Periodic;
myct::BoundarySetting z_b = myct::BoundarySetting::Periodic;
myct::Cubic lattice(len, wid, hei, inter_site_dist, x_b, y_b, z_b);
```

# 2. Impart Material Qualities to the Lattice

Having created a lattice, the next step involves making the lattice represent a
material we are actually interested in. Each of our lattice points must have an
energy associated with it. If we can accurately calculate what that energy
should be then we are one step closer to correctly modeling the electrical
properties of a real material. 
 
A normal or Gaussian distribution is often used to represent the Density of
States (DOS) of disordered semiconductors. We will use the that approximation
here, but if one actually knew the shape of the DOS it could be used instead. 

One of the chief approximations of Gaussian Disorder Model used by Bassler is
that the energies assigned to sites are uncorrelated. This means we can
randomly assign energies from the DOS to each of the sites without considering
that sites in close proximity might be energetically similar. Thankfully the
c++ library comes with a very nice random number generator.

In the code snippet below we know the total number of sites so a vector equal
to the number of sites, each site has an index and thus each index of the
vector **site_energies** contains the energy associated with that particular
site.  

```c++
std::vector<double> generateSiteEnergies(const int & total_num_sites) {
  // Define our DOS as a normal distribution 
  const double mean = -1.2; // eV
  const double std_deviation = 0.07; // eV
  std::normal_distribution<double> distribution(mean, std_deviation);

  // Randomly assign energies from our DOS to our lattice sites
  std::default_random_engine generator;
  std::vector<double> site_energies(total_num_sites);
  for(size_t index = 0; index < total_num_sites; ++index){
    site_energies.at(index) = distribution(generator);
  }
  return site_energies;
}
``` 

# 3. Calculate the Rates between Sites 
