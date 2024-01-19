# mrbglm
Multistage Binomial Modeling in R

This repository provides supplementary material to the paper ``A multistage binomial model with measurement errors: application to protein viability prediction``

The zip file ``mrbglm_0.1.2.tar.gz`` is a source R package (``mrbglm``) for fitting Multistage Binomial Models (MRB) required to reproduce the results in the paper.
This package requires R (>= 4.0.0) and depend on a few other R packages. To install all dependencies:

``` r
# Install devtools if not yet
if(!require("devtools")) install.packages("devtools")

# Install dependencies for wavefinder
install.packages(c('Rdpack', 'grDevices', 'graphics', 'lattice', 'matrixcalc', 'optimx', 'numDeriv', 'boot', 'parallel'), dependencies = TRUE)

To install ``mrbglm`` on your computer, first download the files ``mrbglm_0.1.2.tar.gz`` from the repository.

Then, run the following line with ``~`` replaced by the path to the directory where ``mrbglm_0.1.2.tar.gz``was saved on your computer.

``` r
# Install the required libraries BSEIR and wavefinder
install.packages("~/mrbglm_0.1.2.tar.gz", repos = NULL, type = "source")
```

## Example 1
