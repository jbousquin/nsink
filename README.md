nsink
================

<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- badges: start -->

[![R build
status](https://github.com/jhollist/nsink/workflows/R-CMD-check/badge.svg)](https://github.com/jhollist/nsink/actions)
[![Codecov test
coverage](https://codecov.io/gh/jhollist/nsink/branch/master/graph/badge.svg)](https://codecov.io/gh/jhollist/nsink?branch=master)
[![Lifecycle:
stable](https://img.shields.io/badge/lifecycle-stable-brightgreen.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
<!-- badges: end -->

# Statement of need

The `nsink` package is an R implementation of the methods described in
[Kellogg et. al (2010)](https://doi.org/10.1016/j.ecoleng.2010.02.006).
Previous implementation of this approach relied on a manual, vector
based approach that was time consuming to prepare. This approach uses a
hybrid raster-vector approach that takes relatively little time to set
up for each new watershed and relies on readily available data. Total
run times vary, but range from minutes up to 5 hours depending on
options selected. Previous versions took weeks of manual data
manipulation. Thus, `nsink` was developed to satisfy the need for
quicker implementation of the NSink method as described in [Kellogg et.
al (2010)](https://doi.org/10.1016/j.ecoleng.2010.02.006).

# `nsink` functionality

As of 2021-10-26 user functions for the `nsink` package are:

-   `nsink_get_huc_id()`: A function for searching the name of a USGS
    Watershed Boundary Dataset Hydrologic Unit
    (<https://www.usgs.gov/core-science-systems/ngp/national-hydrography/watershed-boundary-dataset>)
    and retrieving its 12-digit Hydrologic Unit Code (HUC).  
-   `nsink_get_data()`: Using any acceptable HUC ID (e.g. 2-digit to
    12-digit), this function downloads the NHDPlus, SSURGO, NLCD Land
    Cover, and the NLCD Impervious for that HUC.  
-   `nsink_prep_data()`: `nsink` needs data in a common coordinate
    reference system, from mutliple NHDPlus tables, and from different
    portions of SSURGO. This function completes these data preparation
    steps and outputs all data, clipped to the HUC boundary.
-   `nsink_calc_removal()`: Quantifying relative N removal across a
    landscape is a key aspects of an `nsink` analysis. The
    `nsink_calc_removal()` function takes the object returned from
    `nsink_prep_data()` and calculates relative N removal for each
    landscape sink. See Kellogg et al \[-@kellogg2010geospatial\] for
    details on relative N removal estimation for each sink.
-   `nsink_generate_flowpath()`: This function uses a combination of
    flow determined by topography, via a flow-direction raster, for the
    land-based portions of a flow path and of downstream flow along the
    NHDPlus stream network.  
-   `nsink_summarize_flowpath()`: Summarizing removal along a specified
    flow path requires relative N removal and a generated flow path.
    This function uses these and returns a summary of relative N removal
    along a flow path for each sink.
-   `nsink_generate_static_maps()`: This function analyzes N removal at
    the watershed scale by summarizing the results of multiple flow
    paths. Four static maps are returned: 1)removal efficiency;
    2)loading index; 3)transport index; 4)delivery index. Removal
    efficiency is a rasterized version of the `nsink_calc_removal()`
    output. Loading index is N sources based on NLCD categories.
    Transport index is a heat map with the cumulative relative N removal
    along flow paths originating from a grid of points, density set by
    the user, across a watershed, highlighting the gradient of
    downstream N retention. Delivery index is the result of multiplying
    the loading index and the transport index, and shows potential N
    delivery from different sources, taking into account the relative N
    removal as water moves downstream.
-   `nsink_plot()`: A function that plots each raster in the list
    returned from `nsink_generate_static_maps()`.  
-   `nsink_build()`: One of the drivers behind the development of the
    `nsink` package was to provide `n-sink` analysis output that could
    be used more broadly (e.g. within a GIS). The `nsink_build()` runs a
    complete `nsink` analysis and outputs R objects, shapefiles and/or
    TIFFs.
-   `nsink_load()`: Essentially the inverse of the `nsink_build()`
    function, this function takes a folder of files, likely created by
    `nsink_build()`, and reads them into R.

# Installation instructions

At this time we plan on maintaining the `nsink` package as a GitHub only
package and thus it won’t be available directly from CRAN. You may use
the `install_github()` function from the `remotes` package to install
it. The code below will take care of installing `remotes` and installing
`nsink` from the GitHub repository.

``` r
install.packages("remotes")
remotes::install_github("jhollist/nsink", dependencies = TRUE, build_vignettes = TRUE)
```

And then to load up the package:

``` r
library(nsink)
```

# Documentation and examples

All functions are documented, with examples, and that documentation may
be accessed, in R, via the usual help functions. Additionally, an
introduction to the `nsink` package with a more detailed workflow is
documented in a vignette.

``` r
# Load up package
library(nsink)

# Access package level help
help(package = "nsink")

# Access the Introduction to `nsink` vignette
vignette("intro", package = "nsink")
```

## Note on 7-zip

7-zip is needed to extract NHD+ files. It can been installed in normal
ways for your OS and if not installed, `nsink` will provide suggestions.
On linux systems without sudo rights you can attempt to build from
source and copy the bin folder.

    cd
    git clone https://github.com/btolab/p7zip
    cd p7zip
    cp makefile.linux_any_cpu makefile.linux
    make all_test
    cd

    # if No user folder create it
    mkdir usr
    cp -r p7zip/bin usr/.
