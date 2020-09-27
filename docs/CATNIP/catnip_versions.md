# Version 1.9 

Release 08/20/2019

 * Fixed bug in log energy reader which was causing the following error: 

> ERROR Matrix_Multiply only allowed for nxm by lxn matrices
> second matrix must have same number of colums as first
> matrix has number of rows

# Version 1.8

Release 3/6/2019

 * Cleaned up code and made more efficient by making use of const references
 * Replaced testing name from regression to integration in cmake files as it is more appropriate
 * Added badges: travis, codecov, codacy, downloads
 * Created binary releases, and packed source file release
 * Tagged release
 
# Version 1.7

Release 2/2/2019

 * Fixed problem with travis testing 
 * Moved GAUSSIANFILES tutorial files to google drive 
 * Added version flag `calc_J --version`
 * Separated unit and regression testing adding flags (-DENABLE_TESTS=ON and -DENABLE_REGRESSION_TESTS=ON) with cmake 

# Version 1.6

Release 1/24/2019

 * Fixed problem with file readers correctly reading in log file and orbital file information
 * Also added more verbose errors, as well as better error detection 

# Version 1.5

Released 8/15/2018

 * Addressed problem with failure to compile in fedora due to missing math include

# Version 1.4

Released 7/16/2018

 * Fixed problem with calculating transfer integral when the number of basis functions is not equal to the number of molecular orbitals
 * Corrected output of eA and eB values for some reason they were swapped

# Version 1.3

Released 7/14/2018

 * Added citation flag, so users know how to cite the software
 * There was a bug that was preventing calc_J from reading the output of Gaussian .log files when the `#p` was used in the command line options. In this version this problem was addressed.

# Version 1.2

Released 4/26/2018

 * Added counterpoise calculation



