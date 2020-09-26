---
layout: default
---

# QC_Tools/CATNIP

## Motivation

These tools are being developed to post-process data from electronic structure calculations. Ultimately it is the desire of this project to streamline the calculation of parameters such as the: reorganization energy and charge transfer integral. These parameters are needed to use the Marcus rate equation to calculate the charge mobility in materials such as conjugated polymers. 

### Charge Transfer Integral

To understand the theory used to calculate the charge transfer integrals please refer to the following papers:

[1] B. Baumeier, J. Kirkpatrick, and D. Andrienko, “Density-functional based determination of intermolecular charge transfer properties for large-scale morphologies,” Phys. Chem. Chem. Phys., vol. 12, no. 36, p. 11103, 2010.  
[2] E. F. Valeev, V. Coropceanu, D. a da Silva Filho, S. Salman, and J.-L. Brédas, “Effect of electronic polarization on charge-transport parameters in molecular organic semiconductors.,” J. Am. Chem. Soc., vol. 128, no. 30, pp. 9882–6, Aug. 2006.  

## calc_J

This executable is designed to calculate the transfer integral between two monomers. Currently, it only takes output from the Gaussian quantum chemistry software package. If you have another package you would want to use it with please let me know and we can work on implementing it. The calculation will calculate the transfer integral correctly between any two molecules. They do not need to be the same molecule or orientated symmetrically with respect to each other.

[back](./)

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

$$ J = \bra{ \Psi } | \hat{H} | \ket{ \Psi } $$
