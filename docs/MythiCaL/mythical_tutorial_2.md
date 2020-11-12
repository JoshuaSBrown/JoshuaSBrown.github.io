---
layout: mythical_default
--- 


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

# Overview

In the previous simulation, I showed how to simulate a single charge
moving along a 1d lattice. In this tutorial, we will expand on what we
did previously but with several improvements. 

1. We will use a 3d lattice, with periodic boundaries on the y and z directions
2. We will use the Marcus rate equation with the Gaussian Disorder model to generate our rates
3. We will simulate several charges as opposed to just 1
4. We will show how to calculate the current transient

# 1. Creating a Cubic Lattice 

<img src="/assets/mythical_tutorial_II_a.jpg"  width="90%" style="border:30px solid white" />

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

One of the chief approximations of Gaussian Disorder Model used by B&auml;ssler is
that the energies assigned to sites are uncorrelated. This means we can
randomly assign energies from the DOS to each of the sites without considering
that sites in close proximity might be energetically similar. Thankfully the
c++ standard library comes with a nice random number generator.

In the code snippet below, we know the total number of sites, so a vector instance equal
to the number of sites is first initialized. Each site has an index and thus each index of the
vector (**site_energies**) is assigned the energy associated with that particular
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

The next step involves defining the hopping rates that govern charge movement between sites. We
will calculate these rates using the semiclassical Marcus rate equation, though the Miller and 
Abrahams rate equation would also be appropriate.

$$ k_ij = \frac{2\pi}{\hbar} \frac{1}{\sqrt(2 \pi \lambda k_B T)} |H_{AB}|^2 exp( - \frac{(\lambda + \varepsilon_j - \varepsilon_i)^2}{4 \lambda k_B T} ) $$

* $$k_ij$$ represents the hopping rate [1/s]
* $$\hbar$$ is plank's constant with a value of $$6.582119569 \times 10^{−16} $$ [eV s]
* $$\lambda$$ is the reorganization energy [eV]
* $$k_B$$ is Boltzmann's constant with a value of $$8.617333262145 \times 10^{−5}$$ [eV/K]
* $$T$$ is the temperature [K]
* $$H_{AB}$$ is the electronic coupling between initial and final states [eV]
* $$\varepsilon_i$$ is the energy of the site the charge is hopping from [eV]
* $$\varepsilon_j$$ is the energy of the site the charge is hopping to [eV]

I will not go into detail about the Marcus rate equation and how it is derived as there are 
numerious other resources available to the interested reader. However, an explanation of 
how the $$H_{AB}$$ is calculated is warranted. We will assume that the $$H_{AB}$$ is the 
same between every site with only a distance dependence. Such that $$H_{AB}$$ has the following
form. 

$$|H_{AB}|^2 = J_0 e^(- 2 \alpha r_{ij} )$$

* $$J_0$$ is a prefactor [eV]
* $$\alpha$$ is a tunneling constant [1/nm]
* $$r_{ij}$$ is the distance between sites $$i$$ and $$j$$

This approximation is sufficient for our purposes. Deriving appropriate values for $$H_{AB}$$ 
is a bit annoying and requires Quantum Chemistry calculations. You could use the energy 
splitting in dimer method mentioned [here](https://joshuasbrown.github.io/docs/CATNIP/catnip_theory.html) to calculate $$H_{AB}$$.
For a more sophisticated approach one could use the CATNIP program with Gaussian, I have provided
a small [tutorial](https://joshuasbrown.github.io/docs/CATNIP/catnip_tutorial3.html) 
showing how to caculate the constants shown in the equation above. Some Quantum Chemistry
packages also provide a means of calcuating $$|H_{AB}|$$ natively such as NWCHem.

In the following code snippet we show how the rates are prepared to run a Time of Flight 
simulation using MythiCaL. The rates are stored in a nested unordered_map. The keys of
both maps are the site indices. 

We have defined a cutoff distance of $$2.0$$ nm for neighboring sites. Only sites within
this cutoff distance will be considered a potential hopping site. We then proceed to 
define several of the constants. 

I have made use of the Marcus rate equation pacakged
with MythiCaL to actually calculate the rates, the appropriate include line is `#include <mythical/charge_transport/marcus.hpp>`. 
Additionally, the site energies that are
passed to the Marcus rate equation are adjusted to account for effects of an external
electric field applied in the x direction of our lattice.

$$ \varepsilon^{field}_{ij} = q E_{x} \Delta x_{ij} $$ 

* \varepsilon^{field}_{ij} is the energy contribution to add to site $$j$$ when moving from site $$i$$ [eV]
* $$q$$ is the charge, because we are using units of [eV] this will be $$1$$ for holes and $$-1$$ for electrons
* $$E_{x}$$ is the electric field [eV/nm]
* $$\Delta x_{ij}$$ is the change in x distance in moving from site $$i$$ to site $$j$$ [nm]

```c++
unordered_map<int,unordered_map<int,double>>
  calculateRates(const myct::Cubic & lattice, std::vector<double> site_energies) {
    std::cout << "- Calculating rates between sites." << std::endl;
    // Calculate rates between neighboring sites
    double cutoff_dist = 2.0; // nm
    double lambda = 0.02; // eV
    double Temp = 300; // K

    // Here we are have H_AB = A * exp( -alpha * r_ij )
    double alpha = 6; // 1/nm
    double A = 8; // eV
    auto marcus = myct::Marcus(lambda, Temp);

    double electric_field = 0.008; // eV/nm
    double charge = 1.0; // q because hole

    unordered_map<int,unordered_map<int,double>> distances = lattice.getNeighborDistances(cutoff_dist);
    // Here we are now going to assign rates based on the Marcus rate equation
    unordered_map<int,unordered_map<int,double>> rates;
    for ( auto site_neighs : distances ) {
      int site_i = site_neighs.first;
      int x_i = lattice.getX(site_i);
      double energy_i = site_energies.at(site_i);
      for ( auto site_dist : site_neighs.second ) {
        int site_j = site_dist.first;
        int x_j = lattice.getX(site_j);
        double x_diff_dist = static_cast<double>(x_j - x_i) * lattice.getLatticeSpacing();
        double energy_j = site_energies.at(site_j) - electric_field * x_diff_dist * charge;
        double r_ij = site_dist.second;
        double H_AB = A*std::exp( -1.0 * alpha * r_ij );
        double rate_ij = marcus.getRate(energy_i, energy_j, H_AB);
        double rate_ji = marcus.getRate(energy_j, energy_i, H_AB);
        rates[site_i][site_j] = rate_ij;
        rates[site_j][site_i] = rate_ji;
      }
    }
    return rates;
  }

```
