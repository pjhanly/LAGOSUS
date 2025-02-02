
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![R build
status](https://github.com/cont-limno/LAGOSUS/workflows/R-CMD-check/badge.svg)](https://github.com/cont-limno/LAGOSUS/actions)
[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/LAGOSUS)](https://cran.r-project.org/package=LAGOSUS)
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
devtools::install_github("cont-limno/LAGOSUS", dependencies = TRUE)
```

### Data

General users currently have public access to the locus and depth
modules. They can be downloaded and stored to your local system with the
following command (note that an attempt will be made to download all
public modules, those that already exist will be skipped unless an
“overwrite” argument is specified):

``` r
library(LAGOSUS)

# only the locus and depth modules are currently public:
lagosus_get(dest_folder = lagosus_path())
```

Currently only the “locus” and “depth” modules of LAGOS-US has been
released in a public repository. Members of the development team who
have access to unreleased modules (limno, geo, etc), will need to use
the the `lagosus_compile` function (not `lagosus_get`) and supply the
path to their local `locus`, `limno`, `geo`, or `depth` data folders.
Replace the paths in the example below with the path to each respective
folder on your system. Most people will have access to these folders
through Dropbox. For example, the `locus_folder` would be assigned to
something like: `C:/Users/FWL/Dropbox/CL_LAGOSUS_exports/LAGOSUS_LOCUS`

<!-- dir("../../../Downloads/") -->

``` r
# an example for members of the dev team to specify local data folder paths
lagosus_compile(
  locus_version = "1.0",
  locus_folder = "~/Downloads/LAGOSUS_LOCUS/LOCUS_v1.0",
  limno_version = "2.1",
  limno_folder = "~/Downloads/LAGOSUS_LIMNO/US/LIMNO_v2.1/Final exports",
  depth_version = "0.1",
  depth_folder = "~/Downloads/LAGOSUS_DEPTH/DEPTH_v0.1",
  geo_version = "1.0",
  geo_folder = "~/Downloads/LAGOSUS_GEO/GEO_EXPORT_BETA_v1",
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
lg <- lagosus_load(modules = c("locus"))
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
head(lg$locus$lake_characteristics)
```

#### Preview a specific lake

``` r
lake_info(name = "Pine Lake", state = "California")
# or using a lagoslakeid
# lake_info(lagoslakeid = 4389)
```

#### Map specific lakes

``` r
library(mapview)
mapview(coordinatize(lake_info(name = "Pine Lake", state = "California")))
```

#### Read table metadata

``` r
# lookup which table(s) contain a column name
query_lagos_names("ws_meanwidth", dt = lg)
# load help file for a table
?lake_watersheds
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

## Legacy Versions

To install versions of `LAGOSUS` compatible with older versions of
LAGOS-US data modules, review the
[Changelog](https://cont-limno.github.io/LAGOSUS/news/index.html) to
find to package version associated with your desired module version. Run
the following command where `ref` is set to your desired version (in the
example, it is version 0.0.1):

``` r
# install devtools if not found
# install.packages("devtools")
devtools::install_github("cont-limno/LAGOSUS", ref = "v0.0.1")
```

## References

Cheruvelil, K.S., Soranno, P.A., McCullough, I.M., Webster, K.E.,
Rodriguez, L.K. and Smith, N.J., 2021. LAGOS‐US LOCUS v1. 0: Data module
of location, identifiers, and physical characteristics of lakes and
their watersheds in the conterminous US. Limnology and Oceanography
Letters, 6(5), pp.270-292.
