
<!-- README.md is generated from README.Rmd. Please edit that file -->

# cronologia

<!-- badges: start -->

[![R-CMD-check](https://github.com/feddelegrand7/cronologia/workflows/R-CMD-check/badge.svg)](https://github.com/feddelegrand7/cronologia/actions)
[![Codecov test
coverage](https://codecov.io/gh/feddelegrand7/cronologia/branch/master/graph/badge.svg)](https://codecov.io/gh/feddelegrand7/cronologia?branch=master)
[![License: AGPL
v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![CRAN
status](https://www.r-pkg.org/badges/version/cronologia)](https://CRAN.R-project.org/package=cronologia)
[![metacran
downloads](https://cranlogs.r-pkg.org/badges/cronologia)](https://cran.r-project.org/package=cronologia)
[![metacran
downloads](https://cranlogs.r-pkg.org/badges/grand-total/cronologia)](https://cran.r-project.org/package=cronologia)
[![R
badge](https://img.shields.io/badge/Build%20with-♥%20and%20R-violet)](https://github.com/feddelegrand7/cronologia)

<!-- badges: end -->

The goal of `cronologia` is to create an interactive timeline widget in
RMarkdown documents and Shiny applications.

## Installation

You can install the stable version which is available on CRAN however,
for now you should install the development version as it has support for
smaller devices responsiveness:

``` r
remotes::install_github("feddelegrand7/cronologia")
```

# Introduction

The `cronologia` package has three functions:

-   `create_tml()` : used to create simple text-based timelines.
-   `create_tml_img()`: used to create timelines that include images.
-   `create_tml_2()`: works the same way as `create_tml()` except that
    it adds and additional description component.

# Examples

## `create_tml()`

In order to showcase the package’s features, let’s create a simple data
frame:

``` r
batman_data <- data.frame(


  date_release = c("May 31, 2005",
                   "July 14, 2008",
                   "July 16, 2012 "),

  title = c("Batman Begins",
                  "The Dark Knight",
                  "The Dark Knight Rises")
)

batman_data
#>     date_release                 title
#> 1   May 31, 2005         Batman Begins
#> 2  July 14, 2008       The Dark Knight
#> 3 July 16, 2012  The Dark Knight Rises
```

Now, using `create_tml()`, we can create easily a timeline as follows:

``` r
library(cronologia)


create_tml(df = batman_data, # the data frame
           smr = "title", # the column that will be used in the summary 
           dsc = "date_release" # the column that will be used in the description
           )
```

![](man/figures/example1.gif)

You can easily customize the appearance of the time line using the
parameters provided:

``` r
create_tml(df = batman_data,
           smr = "title", # summary
           dsc = "date_release", # description
           smr_col = "blue", # summary text color
           smr_bgcol = "orange", # summary background color
           dsc_col = "white", # description text color
           dsc_bgcol = "black", # description background color
           dsc_size = "30px" # description size
           )
```

![](man/figures/example2.gif)

### ❗❗❗

If you want to make all the summary components open by default, you can
set the `open` parameter to `TRUE`. The parameter is available in all
the functions.

## `create_tml_img()`

If you want to include images within your timeline, you can use the
`create_tml_img()` function. To illustrate this function, we’ll use the
[radous](https://github.com/feddelegrand7/radous) package that fetch the
[randomuser.me](https://randomuser.me/) API and returns a data frame
that contains many information (including images’ URLs).

> Disclaimer: All the generated images are extracted from the authorized
> section of UI Faces.

``` r
library(radous)

df <- get_data(n = 4, seed = "123")

df[c('name_last', 
     'location_street_name',
     'picture_large',
     'name_last')]
#> # A tibble: 4 x 4
#>   name_last location_street_na~ picture_large                          name_last
#>   <chr>     <chr>               <chr>                                  <chr>    
#> 1 Campos    Rua Três            https://randomuser.me/api/portraits/m~ Campos   
#> 2 Jackson   Armagh Street       https://randomuser.me/api/portraits/w~ Jackson  
#> 3 Ruona     Hämeentie           https://randomuser.me/api/portraits/w~ Ruona    
#> 4 Steward   Henry Street        https://randomuser.me/api/portraits/w~ Steward
```

Now we will proceed as previously except that we need to provide two
additional arguments:

-   `imgsrc`: the column that indicates the source of the images.
-   `imgalt`: the column indicating the `alt` attribute of the images.
    For accessibility reasons I decided to make this argument mandatory.
    Use a column that contains `""` if the images do not need the `alt`
    attribute.

``` r
df <- radous::get_data(4, seed = "123")

create_tml_img(df, 
               smr = "name_last", 
               dsc = "location_street_name", 
               imgsrc = "picture_large", 
               imgalt = "name_last", 
               imgwidth = "150px", 
               imgheight = "150px", 
               dsc_size = "20px")
```

![](man/figures/example3.gif)

## `create_tml_2()`

Following the idea of
[Tobias](https://twitter.com/toeb18/status/1355104693299634181?s=20) for
creating an interactive CV I thought that two (2) description components
would be more appropriate. The function is similar to `create_tml()`
except that it adds another description paragraph to the Timeline.

Let’s go through an example:

``` r
cv <- data.frame(
  
  
  jobs = c("Game tester at Nintendo", "Food tester at Ferrero", "Movies tester at Netflix"), 
  
  period = c("2020-2022", "2022-2024", "2026-2030"),
  
  todos = c("Playing Zelda all day", "Eating Bueno all day", "Watching the Office all day")
  
)


cv
#>                       jobs    period                       todos
#> 1  Game tester at Nintendo 2020-2022       Playing Zelda all day
#> 2   Food tester at Ferrero 2022-2024        Eating Bueno all day
#> 3 Movies tester at Netflix 2026-2030 Watching the Office all day
```

``` r
create_tml_2(cv, 
             smr = "jobs", 
             dsc = "period", 
             dsc2 = "todos", 
             dsc2_col = "white",
             dsc2_bgcol = "peru") # yes, peru is also a color
```

![](man/figures/example4.gif)

# TODOS

-   [x] Writing unit tests
-   [ ] Creating a hex sticker
-   [x] Sharing with RWeekly
-   [ ] Talk about it at a virtual event

# Code of Conduct

Please note that the `cronologia` project is released with a
[Contributor Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
