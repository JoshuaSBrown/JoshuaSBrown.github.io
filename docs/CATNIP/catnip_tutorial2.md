## Addressing Counter Poise Error
### What is the counterpoise error
The counterpoise error occurs as a result of using a different number of basis functions for the monomers and the dimer. Generally, a calculation will be more accurate if more basis functions are present. Thus, in and ideal situation you would use the same number and type of basis functions for your dimer calculation as you do for your monomers. Typically, this is not a large error especially if the monomers are separated by distances greater than 4 Angstroms. Even at distances less than 4 Angstroms the counter poise error remains small. Regardless, if you are concerned about this error it can be addressed with calc_J. 

### Setup
Similar to Tutorial 1, you will need 3 input files for monomer 1, monomer 2 and the dimer. As an example I have included some sample input files with the ethylene system used previously in Tutorial 1. They can be found in: GAUSSIANFILES/CounterPoise/. This assumes you have already downloaded [GAUSSIANFILES.zip](https://drive.google.com/open?id=1C3CRQJxXyDgmEBgjvQ7_J4HzuS63ppQG) and unzipped it.

| Molecule | File       |
| -------- | ---------- |
| MonomerA | ref.gjf    |
| MonomerB | 1_B.gjf    |
| Dimer    | 1_pair.gjf |

Notice that there is nothing new in the 1_pair.gjf input file, which contains the dimer coordinates. However, both the ref.gjf and 1_B.gjf files have been changed. These files contain all the atoms for the full dimer system with the exception that the -Bq label has been appended to the atoms belonging to monomer B in A.gjf and to the atoms belonging to monomer A in B.gjf. 

The -Bq symbol in essence allows the Gaussian program to use the basis functions for any atom without including the charge from the nucleus or it's electrons. Thus:

H-Bq 

Will add the basis functions for a hydrogen atom without it's charges. H-Bq can be thought of as a ghost atom. Hence, ref.gjf file contains the coordinates basis functions and charge of monomer A, it also contains the coordinates basis functions but NOT the charge of monomer B. For 1_B.gjf the opposite holds true. 

### Output Files
After running the simulation you should end up with 3 log files and 3 fort.7 files, one for each of the calculations. The fort.7 files have been rewritten with a .pun extension and the name changed so it is obvious which calculation they came from. 

| Molecule  | Input File | Log File   | Pun File   |
| --------- | ---------- | ---------- | ---------- |
| Monomer A | ref.gjf    | ref.log    | ref.pun    |
| Monomer B | 1_B.gjf    | 1_B.log    | 1_B.pun    |
| Dimer     | 1_pair.gjf | 1_pair.log | 1_pair.pun |

### Calculating the Transfer Integral with Counter Poise Correction
Similar to tutorial 1 the same commands can be run all you need to do to calculate the counterpoise correction is append the `-cp` flag to the command, this will let calc_J know to use the extra basis functions output from the Gaussian run. 

Below, I have demonstrated what such a command might look like for the example system provided with calc_J.  

    calc_J -cp -p_1 CounterPoise/Dist_1Ang/ref.pun -p_2 CounterPoise/Dist_1Ang/1_B.pun -p_P CounterPoise/Dist_1Ang/1_pair.pun 

This calculation was run with two ethylene molecules at a distance of 1 Angstrom to demonstrate how the transfer integral is affected at small distances. The output yields a transfer integral of value: 4.50589 eV

Let us compare this with the case that I did not use ghost atoms (files not provided). The value of the transfer integral would then be  4.53803 eV. A total error of about ~ 0.7 % which is negligible in most cases. 
