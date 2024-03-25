
<!-- README.md is generated from README.Rmd. Please edit that file -->

# crash-data-vis-demo

<!-- badges: start -->
<!-- badges: end -->

The goal of crash-data-vis-demo is to demonstrate how to get and
visualise data on road traffic casualties and road geometries.

``` r
# Install the remotes package if not yet installed:
if (!"remotes" %in% installed.packages()) {
  install.packages("remotes")
}
```

``` r
# Check you have the packages installed:
remotes::install_cran(c("tidyverse", "stats19"))
#> Skipping install of 'tidyverse' from a cran remote, the SHA1 (2.0.0) has not changed since last install.
#>   Use `force = TRUE` to force installation
#> Skipping install of 'stats19' from a cran remote, the SHA1 (3.0.3) has not changed since last install.
#>   Use `force = TRUE` to force installation
```

``` r
library(tidyverse) # for data manipulation and visualisation
#> ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.4          ✔ readr     2.1.5     
#> ✔ forcats   1.0.0          ✔ stringr   1.5.1     
#> ✔ ggplot2   3.4.4.9000     ✔ tibble    3.2.1     
#> ✔ lubridate 1.9.3          ✔ tidyr     1.3.1     
#> ✔ purrr     1.0.2          
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
library(stats19) # for getting road casualty data
#> Data provided under OGL v3.0. Cite the source and link to:
#> www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
```
