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



Here we have created a 20 site system. Each site is connected to it's two
nearest neighbors with the exception of the two end sites. 

<img src="/assets/mythical_tutorial_I_a.png" width="100%" />

A charge is created which inherits from the random walker class. A single
charge is created from the Charge class and is given an id of 0 and initially
placed on site 1. The charge is then allowed to hop between the sites using the
built in KMC engine by calling the hop function. The charges dwell time and the
site it occupies is updated each time the hop() function is called. The rates
have been setup so that it is much more favorable to hop towards the
neighboring site with the largest id. 

## Code
```c++
// 1D_example.cpp
#include <mythical/coarsegrainsystem.hpp>
#include <mythical/walker.hpp>

#include <unordered_map>
#include <vector>
#include <iostream>
#include <fstream>

using namespace std;

namespace my = mythical;

typedef std::pair<int,shared_ptr<my::Walker>> walker_t;

unordered_map<int,unordered_map<int, double>>
generateRates(const int number_of_sites) {
  unordered_map<int,unordered_map<int, double>> rates;
  for(int i=1 ; i<number_of_sites; ++i){
    rates[i][i+1]=3.0;
    rates[i+1][i]=1.0;
  }
  return rates;
}

int main(){

  int number_of_sites = 20;

  unordered_map<int,unordered_map<int, double>> rates =  generateRates(number_of_sites);

  my::CoarseGrainSystem CGsystem;
  CGsystem.setTimeResolution(3.0);
  CGsystem.initializeSystem(rates);

  class Charge : public my::Walker {};
  
  int charge_id;
  vector<walker_t> charges;
  charges.emplace_back(charge_id, std::shared_ptr<my::Walker>( new Charge));
  charges.back().second->occupySite(1);
  CGsystem.initializeWalkers(charges);

  const int iterations = 50;

  ofstream traj_file;
  traj_file.open("Trajectory.txt");
  traj_file << "Number of Sites: " << number_of_sites << endl;
  traj_file << "Number of iterations: " << iterations << endl;
  traj_file << endl;
  traj_file << "Site    Dwell Time" << std::endl;

  for(int iteration = 0; iteration<iterations;++iteration){
    CGsystem.hop(charges.at(0));
    const int site_id = charges.at(0).second->getIdOfSiteCurrentlyOccupying();
    const double dwell_time = charges.at(0).second->getDwellTime();
    cout << "Charge occupying site: " << site_id << " dwell time " << dwell_time << endl;
    traj_file << site_id << "      " << dwell_time << endl;
  }

  CGsystem.removeWalkerFromSystem(charges.at(0));
  traj_file.close();

  return 0;
}
```

## Compilation

To compile directly you could enter something like what is shown below where the
**-I** and **-L** are followed by the paths to the includes and the library. 

```bash
 gcc 1D_example.cpp -o 1D_example -lstdc++ -I/usr/local/include/mythical -L/usr/local/lib/mythical -lmythical
```

Using our recommended approach you could have a simple cmake file.

```cmake
cmake_minimum_required(VERSION 3.12)
project(1D_example)

enable_language(CXX)

find_package(MythiCaL REQUIRED)
add_executable(1D_example 1D_example.cpp)

target_link_libraries(ToF_example PUBLIC mythical::mythical)
``` 

And then run in the terminal:

```bash
$ cmake -S . -B build
$ cmake --build build
```

## Running

Notice that the dwell time varies during each hop even though the rates are the
same off each site. This is because though the sites will have a time constant
that is the same the dwell time is calculated from the dwell time using a
random number X with the equation:

$$ t_{dwell} = -log(X) \tau_{dwell} $$

The random number $$X$$ is a value between 0 and 1 and changes each time the
hop function is called. 

```bash
$ ./1D_example
Charge occupying site: 2 dwell time 0.118066
Charge occupying site: 3 dwell time 0.0297392
Charge occupying site: 2 dwell time 0.00954765
Charge occupying site: 3 dwell time 0.944914
Charge occupying site: 4 dwell time 0.735009
Charge occupying site: 5 dwell time 0.107547
:
Charge occupying site: 19 dwell time 0.211318
Charge occupying site: 20 dwell time 0.256244
Charge occupying site: 19 dwell time 0.181556
Charge occupying site: 20 dwell time 0.278988
Charge occupying site: 19 dwell time 0.150574
```

<video width="480" height="240" controls="controls">
  <source src="/assets/output.mp4" type="video/mp4">
</video>
