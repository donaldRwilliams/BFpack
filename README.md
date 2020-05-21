
<img src="man/figures/logo_BFpack.png" width = 400 />

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

## Illustrative Examples

Below several example analyses are provided using **BFpack**.

### Bayesian t testing

First a classical one sample t test is executed for the test value
\(\mu = 5\) on the therapeutic data

``` r
ttest1 <- t_test(therapeutic, alternative = "greater", mu = 5)
```

By default `BF` executes an exehaustive test around the null value of
\(H_1:\mu=5\) versus \(H_2:\mu<5\) versus \(H_3:\mu>5\) assuming equal
prior probabilities for \(H_1\), \(H_2\), and \(H_3\) of
\(\frac{1}{3}\).

``` r
BF1 <- BF(ttest1)
```

The same test would be executed when the hypotheses would be explicitly
formulated using the `hypothesis` argument.

``` r
hypotheses <- "mu = 5; mu < 5; mu > 5"
BF1 <- BF(ttest1, hypothesis = hypotheses)
```
