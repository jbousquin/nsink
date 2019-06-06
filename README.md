
<!-- README.md is generated from README.Rmd. Please edit that file -->

# nsink

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

The `nsink` package is an R implementation of the methods described in
[Kellogg et. al (2010)](https://doi.org/10.1016/j.ecoleng.2010.02.006).
Previous implementation of this approach relied on a vector based
approach that was time consuming to prepare. This approach uses a hybrid
raster-vector approach that takes very little time to set up for each
new watershed and relies on readily available data. Thus, its
application is national for the United States.

Proposed (as of 2019-06-06) functions for the `nsink` package are:

  - `nsink_get_data()`

  - 
## Installation

When released to CRAN, you can install the released version of nsink
from [CRAN](https://CRAN.R-project.org) with:

``` r
install.packages("nsink")
```

And the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("jhollist/nsink")
```
