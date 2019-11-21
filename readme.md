
<!-- README.md is generated from README.Rmd. Please edit that file -->

# baggr

<!-- badges: start -->

[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version-last-release/baggr?color=green)](http://cran.r-project.org/package=baggr)
[![Travis build
status](https://travis-ci.org/wwiecek/baggr.svg?branch=cran)](https://travis-ci.org/wwiecek/baggr)
[![codecov](https://codecov.io/gh/wwiecek/baggr/branch/master/graph/badge.svg)](https://codecov.io/gh/wwiecek/baggr)
[![](https://cranlogs.r-pkg.org/badges/baggr)](https://cran.rstudio.com/web/packages/baggr/index.html)
<!-- badges: end -->

This is *baggr*, an [R package](https://www.r-project.org/) for Bayesian
meta-analysis using [Stan](https://mc-stan.org/). *Baggr* is intended to
be user-friendly and transparent so that it’s easier to understand the
models you are building and criticise them.

*Baggr* provides a suite of Bayesian aggregation models that work with
both summary data and full data sets, to synthesise evidence collected
from different groups, contexts or time periods. The `baggr()` command
automatically detects the data type and, by default, fits Rubin’s (1981)
partial pooling model with weakly informative priors by calling Stan
under the hood to carry out Bayesian inference. Modelling of variances
or quantiles, standardisation and transformation of data is also
possible.

The current version (v0.2, October 2019) is a stable prototype of a tool
that’s in active development so we are counting on your feedback.

## Installation

Before starting, `baggr` will not work if you don’t have RStan. In that
case, please follow [the installation instructions for
RStan](https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started).

You can install the most up-to-date version of `baggr` directly from
GitHub; for this, you will need the `devtools` package.

``` r
# compilation of models should take 5-10 minutes
devtools::install_github("wwiecek/baggr", 
                         build_vignettes = TRUE,
                         build_opts = c("--no-resave-data", "--no-manual"))
```

## Basic use case

`baggr` is designed to work well with both individual-level (“full”) and
aggregate/summary (“group”) data on treatment effect. In basic cases
only the summary information on treatment effects (such as means and
their standard errors) is needed. Data are always specified in a single
data frame and the same `baggr()` function is used for different models.

For the “standard” cases of modelling means, the appropriate model is
detected from the shape of data.

``` r
library(baggr)
df_pooled <- data.frame("tau" = c(28,8,-3,7,-1,1,18,12),
                        "se"  = c(15,10,16,11,9,11,10,18))
bg <- baggr(df_pooled, pooling = "partial")
```

You can specify the model type from several choices, the pooling type
(`"none"`, `"partial"` or `"full"`), and certain aspects of the priors,
as well as other options for data preparation, prediction and more. You
can access the underlying `stanfit` object through `bg$fit`.

Flexible plotting methods are included, together with an automatic
comparison of multiple models (e.g. comparing no, partial and full
pooling) through `baggr_compare()` command. Various statistics can be
calculated: in particular, the `loocv()` command automatically performs
leave-one-group-out cross-validation, allowing us to compare and select
models.

Try `vignette('baggr')` for an overview of these functions and an
example of meta-analysis workflow with `baggr`.

## Current and future releases

Included in baggr v0.2:

  - Hierarchical models for continuous outcomes
  - Meta-analysis-specific summaries and plots
  - Compatibility with `rstan` and `bayesplot` features
  - Both full and aggregate data sets can be used
  - Automatic choice of priors or “plain-text” choice of priors
  - Automatic calculation of pooling metrics
  - Cross-validation (including leave-one-out)
  - Prior and posterior predictive distributions

In the coming months we will be including more models and more features
such as:

  - Binary data
  - Modelling of quantiles and of SE’s
  - Modelling of log-normal data
  - Automatic standardisation of variables
  - New plotting styles and more model diagnostics
