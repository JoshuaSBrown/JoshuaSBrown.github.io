# Errors when running tutorials

Be sure that you follow the download instructions, and note that the tutorial files are now stored separately on google drive. Previously, they were accessible through git-lfs this feature has now been disabled since version 1.6 as it wasn't working anyway. The tutorial files can now be found here: [GAUSSIANFILES.zip](https://drive.google.com/open?id=1C3CRQJxXyDgmEBgjvQ7_J4HzuS63ppQG).

# Errors when running calc_J

Be sure that the Gaussian calculation finished normally if you don't see the "Normal termination" at the bottom of the .log file your calculations may not have completed correctly. 

# Errors during run time

In the case that you run into an error like this:

_Counter poise has not been specified and the total number of basis functions in your monomer files is greater than the the number of basis functions in your dimer_

It means that the number of basis functions are not being conserved between the different files. If you are running a counter poise calculation the number of basis functions should be exactly the same between all three calculations.
If you are not running a counter poise calculation then the sum of the basis functions in Monomer A and Monomer B should equal the total number in the dimer calculation.

There are two different reasons this error might arise.

## 1. Gaussian auto removal of some basis functions

Gaussian might be **removing** some basis functions if their coefficients are **small**. To detect if this is the problem, you can use grep to check the _NBsUse_ and _NBasis_ entries:
```
grep -B 2 NBs <Gaussian Log file>
```
The difference in basis functions can be avoided by using the `iop(3/60=N)` keyword. [Here](http://gaussian.com/overlay3/#iop_(3/60)) is a link to the Gaussian keyword on their official website.

## 2. Non uniform basis function description between files

Gaussian might be using **different types of Gaussian basis functions** in your calculations. There are two flavors of Gaussian type functions those written in their Cartesian form and those written in Spherical form. 

E.g. For the D shell at total of 6 Catersian Gaussian basis functions are used (XX, YY, ZZ, XY, XZ, YZ), whereas in the Spherical form a total of 5 Gaussian basis functions are used (z^2, x^2-y^2 , xy, xz, yz). This is the difference between the 5D shell basis functions and the 6D shell basis functions. The same is true for the 7F (Spherical) and 10F (Cartesian) Gaussian type functions.

They are almost the same thing, but you need to be sure that Gaussian is **using the same combination** in both the **monomer and dimer calculations**. Check the [Gaussian documentation](http://gaussian.com/gen/) for more details.

NOTE: A word of caution, adding additional keywords can affect the results of your calculation, so be careful.



