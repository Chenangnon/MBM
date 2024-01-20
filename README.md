# mrbglm
Multistage Binomial Modeling in R

This repository provides supplementary material to the paper ``A multistage binomial model with measurement errors: application to protein viability prediction``

The zip file ``mrbglm_0.1.2.tar.gz`` is a source R package (``mrbglm``) for fitting Multiplicative Risk Binomial (MRB), a special case of Models Multistage Binomial Models when each stage has only one predictor. The package is required to reproduce the results in the indexed paper.
This package requires R (>= 4.0.0) and depend on a few other R packages. To install all dependencies:

``` r
# Install devtools if not yet
if(!require("devtools")) install.packages("devtools")

# Install dependencies for wavefinder
install.packages(c('Rdpack', 'grDevices', 'graphics', 'lattice', 'matrixcalc', 'optimx', 'numDeriv', 'boot', 'parallel'), dependencies = TRUE)
```

To install ``mrbglm`` on your computer, first download the files ``mrbglm_0.1.2.tar.gz`` from the repository.

Then, run the following line with ``~`` replaced by the path to the directory where ``mrbglm_0.1.2.tar.gz``was saved on your computer.

``` r
# Install the library mrbglm
install.packages("~/mrbglm_0.1.2.tar.gz", repos = NULL, type = "source")
```

## Example 1
Simulate some data
``` r
library(mrbglm)
set.seed(123)
mrbdata = sim.mrb (beta = c(-2, -1),
                   x = cbind(x1 = runif(10000, min = -5, max = 10),
                             x2 = runif(10000, min = -5, max = 10)),
                   delta = qlogis(.6))$data[,c("y", "x1", "x2")]

head (mrbdata)
```

Logistic fit with upper limit of the success probability to 1 (glm)
``` r
MRBfit0 = glm (y ~ x1 + x2,
               family = binomial(probit),
               data = mrbdata)
MRBfit0

summary(MRBfit0)
```


Including a maximum success probability (default for glm.mrb)
``` r
MRBfit1 = glm.mrb (y ~ x1 + x2,
                   data = mrbdata)

summary(MRBfit1)
```

AIC and R-squared
``` r
AIC(MRBfit0, MRBfit1)
NagelkerkeR2(MRBfit0, MRBfit1)
```

Contour plot of probability levels
``` r
contour(MRBfit1, xlim = c(-2, 3),
        colorkey = list(at = c(0, 0.2, 0.4, 0.6, 0.8)))
```

3-D plot of the probability surface using 'curves' ('curves' is only available for one or two predictors)
``` r
curves(MRBfit1, xlim = c(-3, 4))
```

Add measurement errors to covariates
``` r
mrbdata$Err1 <- abs(rnorm(length(mrbdata$x1), sd = 2))
mrbdata$Err2 <- abs(rnorm(length(mrbdata$x2), sd = 2))
mrbdata$x1err <- mrbdata$x1 + rnorm(length(mrbdata$x1), sd = mrbdata$Err1)
mrbdata$x2err <- mrbdata$x2 + rnorm(length(mrbdata$x2), sd = mrbdata$Err2)
```

Ignoring the measurement errors
``` r
MRBfite0 = glm.mrb (y ~ x1err + x2err,
                    data = mrbdata)

summary(MRBfite0)
```

Including measurement errors (known constants)
``` r
MRBfite1 = glm.mrb (y ~ x1err + x2err,
                    x.with.me = c(TRUE, TRUE),
                    me.offsets = ~ Err1 + Err2,
                    data = mrbdata)

summary(MRBfite1)
```

AIC and R-squared
``` r
AIC(MRBfite0, MRBfite1)
NagelkerkeR2(MRBfite0, MRBfite1)
```
