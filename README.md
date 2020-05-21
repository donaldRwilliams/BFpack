
<img src="man/figures/logo_BFpack.png" width = 550 />

# 

[![CRAN
Version](http://www.r-pkg.org/badges/version/BFpack)](https://cran.r-project.org/package=BFpack)
[![Downloads](https://cranlogs.r-pkg.org/badges/BGGM)](https://cran.r-project.org/package=BFpack)

The `R` package **BFpack** contains a set of functions for Bayesian
exploratory (e.g., equal vs negative vs postive) and confirmatory
(equality and/or order constraints) hypothesis testing under commonly
used statistical models, including Bayesian t testing, Bayesian
(M)AN(C)OVA, multivariate/univariate linear regression, Bayesian
correlation analysis, Bayesian multilevel analysis, or Bayesian
generalized linear models (e.g., logistic regression). The main function
`BF` needs a fitted model (e.g., an object of class `lm` for a linear
regression model) and (optionally) the argument `hypothesis`, a string
which specifies a set of equality/order constraints on the parameters.
By applying the function `get_estimates`on a fitted model, the names of
the parameters are returned on which constrained hypotheses can be
formulated. Bayes factors and posterior probabilities are computed for
the hypotheses of interest.

## Installation

Install the latest release version of `BFpack` from CRAN:

``` r
install.packages("BFpack")
```

The current developmental version can be installed with

``` r
if (!requireNamespace("remotes")) { 
  install.packages("remotes")   
}   
remotes::install_github("jomulder/BFpack")
```

## Example analyses

Below several example analyses are provided using **BFpack**.

### Bayesian t testing

First a classical one sample t test is executed for the test value
\(\mu = 5\) on the therapeutic data

``` r
ttest1 <- bain::t_test(therapeutic, alternative = "greater", mu = 5)
```

The `t_test` function is part of the ***bain*** package. The function is
equivalent to the standard `t.test` function with the addition that the
returned object contains additional output than the standard `t.test`
function.

To perform a Bayesian t test plug the fitted object into the `BF`
function.

``` r
library(BFpack)
BF1 <- BF(ttest1)
```

This executes an exhaustive test around the null value: `H1: mu = 5`
versus `H2: mu < 5` versus `H3: mu > 5` assuming equal prior
probabilities for `H1`, `H2`, and `H3` of 1/3. The output presents the
posterior probabilities for the three hypotheses.

The same test would be executed when the same hypotheses are explicitly
specified using the `hypothesis` argument.

``` r
hypothesis <- "mu = 5; mu < 5; mu > 5"
BF(ttest1, hypothesis = hypothesis)
```

When testing hypotheses via the `hypothesis` argument, the output also
presents an `Evidence matrix` containing the Bayes factors between the
hypotheses.

### Bayesian analysis of variance

First an analysis of variance (ANOVA) model is fitted using the `aov`
fuction in `R`.

``` r
aov1 <- aov(price ~ anchor * motivation, data = tvprices)
```

Next a Bayesian test can be performed on the fitted object.

``` r
BF(aov1)
```

By default posterior probabilities are computed of whether main effects
and interaction effects are present.

### Bayesian logistic regression

First a logistic regression model is fitted using the `glm`
function

``` r
fit_glm <- glm(sent ~ ztrust + zfWHR + zAfro + glasses + attract + maturity +
               tattoos, family = binomial(), data = wilson)
```

The names of the regression coefficients on which constrained hypotheses
can be formualted can be extracted using the `get_estimates` function.

``` r
get_estimates(fit_glm)
```

Two different hypotheses are formulated with competing equality and/or
order constraints on the parameters of interest. These hypotheses are
motivated in Mulder et al. (2019)

``` r
BF_glm <- BF(fit_glm, hypothesis = "ztrust > (zfWHR, zAfro) > 0;
             ztrust > zfWHR = zAfro = 0")
summary(BF_glm)
```

By calling the `summary` function on the output object of class `BF`,
the results of the exploratory tests are presented of whether each
separate parameter is zero, negative, or positive and the results of the
confirmatory test of the hypotheses in the `hypothesis` argument are
provided. When the hypotheses do not cover the complete parameter space,
by default the complement hypothesis is added which covers the remaining
parameter space that is not covered by the constraints under the
hypotheses of interest. In the above example, the complement hypothesis
covers the parameter space where neither `"ztrust > (zfWHR, zAfro) > 0"`
holds, nor where `"ztrust > zfWHR = zAfro = 0"` holds.
