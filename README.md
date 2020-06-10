
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Project Status: WIP – Initial development is in progress, but there
has not yet been a stable, usable release suitable for the
public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![R build
status](https://github.com/cont-limno/LAGOSUS/workflows/R-CMD-check/badge.svg)](https://github.com/cont-limno/LAGOSUS/actions)
[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/LAGOSUS)](https://cran.r-project.org/package=LAGOSUS)
[![CRAN RStudio mirror
downloads](http://cranlogs.r-pkg.org/badges/LAGOSUS)](https://cran.r-project.org/package=LAGOSUS)

# LAGOSUS <img src="man/figures/logo.png" align="right" height=140/>

The `LAGOSUS` package provides an R interface to download LAGOS-US data,
store this data locally, and perform a variety of filtering and
subsetting operations.

LAGOS-US contains data for 479,950 lakes and reservoirs larger than 1 ha
in continental US. The database includes 4 data modules for: lake
location and physical characteristics for all lakes; ecological context
(i.e., the land use, geologic, climatic, and hydrologic setting of
lakes) for all lakes; in situ measurements of lake water quality for a
subset of the lakes from the past 3 decades for approximately Y-Z lakes
depending on the variable (see
[References](https://github.com/cont-limno/LAGOSUS#references)); and
depth for a subset of all lakes (forthcoming).

## Installation

``` r
# install development version from Github
# install devtools if not found - install.packages("devtools")
devtools::install_git(
  "https://gitlab.msu.edu/stachel2/lagosus", dependencies = TRUE)
```

### Data

Until the LAGOS-US datasets have been made available in a public
repository, LAGOSUS users will need to use the `lagosus_compile`
function (not `lagosus_get`) and supply the path to their local `locus`,
`limno`, `geo`, and `depth` data folders. Replace the paths in the
example below with the path to each respective folder on your system.
Most people will have access to these folders through Dropbox. For
example, the `locus_folder` would be assigned to something like:
`C:/Users/FWL/Dropbox/CL_LAGOSUS_exports/LAGOSUS_LOCUS`

Files are “compiled” to an `R` data format in the location specified by
the `dest_folder` argument. Recommended setting is `lagosus_path()`.
Data only needs to be downloaded one time per version per machine. Each
`LAGOSUS`
[module](https://cont-limno.github.io/LAGOSUS/articles/lagosus_structure.html)
has a unique version number.

``` r
library(LAGOSUS)

lagosus_compile(
  locus_version = "1.1",
  locus_folder = "~/Downloads/LAGOSUS_LOCUS/LOCUS_v1.1",
  depth_version = "0",
  depth_folder = "~/Downloads/LAGOSUS_DEPTH", 
  dest_folder = lagosus_path())
```

## Usage

### Load Package

``` r
library(LAGOSUS)
```

### Load data

The `lagosus_load` function returns a named list of `data.frame`
objects. Use the `names()` function to see a list of available data
frames `names(lg)`.

``` r
lg <- lagosus_load(modules = c("locus", "depth"))
names(lg)
```

<!-- ```{r load_data_cached, eval=FALSE, echo=FALSE} -->

<!-- dt <- readRDS(system.file("lagos_test_subset.rds", package = "LAGOSUS")) -->

<!-- names(dt) -->

<!-- ``` -->

<!-- #### Locate tables containing a variable  -->

<!-- ```{r eval=FALSE} -->

<!-- query_lagos_names("secchi") -->

<!-- ``` -->

<!-- ```{r echo=FALSE, eval=FALSE} -->

<!-- query_lagos_names("secchi", dt = dt) -->

<!-- ``` -->

#### Preview a table

``` r
head(lg$locus$locus_characteristics)
```

#### Preview a specific lake

``` r
lake_info(name = "Pine Lake", state = "Michigan")
# or using a lagoslakeid
# lake_info(lagoslakeid = 4389)
```

#### Map specific lakes

``` r
library(mapview)
mapview(coordinatize(lake_info(name = "Pine Lake", state = "Michigan")))
```

#### Read table metadata

``` r
# lookup which table(s) contain a column name 
query_lagos_names("ws_meanwidth", dt = lg)
# load help file for a table
?locus_watersheds
```

<!-- ```{r load printr, echo=FALSE,message=FALSE,results='hide', eval=FALSE} -->

<!-- loadNamespace("printr") -->

<!-- ``` -->

<!-- ```{r Read metadata for individual tables, eval=FALSE} -->

<!-- help.search("datasets", package = "LAGOSUS") -->

<!-- ``` -->

<!-- ```{r unload printr, echo=FALSE, eval=FALSE} -->

<!-- unloadNamespace("printr") -->

<!-- ``` -->

## References

Soranno, P.A., Bacon, L.C., Beauchene, M., Bednar, K.E., Bissell, E.G.,
Boudreau, C.K., Boyer, M.G., Bremigan, M.T., Carpenter, S.R., Carr, J.W.
Cheruvelil, K.S., and … , 2017. LAGOS-NE: A multi-scaled geospatial and
temporal database of lake ecological context and water quality for
thousands of US lakes. GigaScience,
<https://doi.org/10.1093/gigascience/gix101>
