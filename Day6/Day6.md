Day6
================
2018-11-19

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.0     v purrr   0.2.5
    ## v tibble  1.4.2     v dplyr   0.7.7
    ## v tidyr   0.8.2     v stringr 1.3.1
    ## v readr   1.1.1     v forcats 0.3.0

    ## -- Conflicts ------------------------------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

More dental care
================

``` r
# Load data
dental_data <- read_csv2("../Class_files/Statistikdatabasen_2018-01-23 14_46_26.csv", skip = 1, n_max = 580)
```

    ## Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.

    ## Parsed with column specification:
    ## cols(
    ##   Region = col_character(),
    ##   Kön = col_character(),
    ##   `2010` = col_integer(),
    ##   `2011` = col_integer(),
    ##   `2012` = col_integer(),
    ##   `2013` = col_integer(),
    ##   `2014` = col_integer(),
    ##   `2015` = col_integer(),
    ##   `2016` = col_integer()
    ## )

``` r
#pop_data <- read_csv2("../Class_files/BE0101A5.csv")
```

Sytembolaget
============

``` r
# Load data 
sortiment_11_19 <- read_csv("systembolaget2018-11-19.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   nr = col_integer(),
    ##   Artikelid = col_integer(),
    ##   Varnummer = col_integer(),
    ##   Prisinklmoms = col_double(),
    ##   Volymiml = col_double(),
    ##   PrisPerLiter = col_double(),
    ##   Saljstart = col_date(format = ""),
    ##   Utgått = col_integer(),
    ##   Argang = col_integer(),
    ##   Ekologisk = col_integer(),
    ##   Etiskt = col_integer(),
    ##   Koscher = col_integer(),
    ##   Pant = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
sortiment_10_08 <- read_csv("../Class_files/systembolaget2018-10-08.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   nr = col_integer(),
    ##   Artikelid = col_integer(),
    ##   Varnummer = col_integer(),
    ##   Prisinklmoms = col_double(),
    ##   Volymiml = col_double(),
    ##   PrisPerLiter = col_double(),
    ##   Saljstart = col_date(format = ""),
    ##   Utgått = col_integer(),
    ##   Argang = col_integer(),
    ##   Ekologisk = col_integer(),
    ##   Etiskt = col_integer(),
    ##   Koscher = col_integer(),
    ##   Pant = col_double()
    ## )
    ## See spec(...) for full column specifications.

``` r
# Dataframe of newly added beverages
sortiment_nytt <- setdiff(sortiment_11_19, sortiment_10_08)

# Dataframe newly removed beverages
sortiment_borta <- setdiff(sortiment_10_08, sortiment_11_19)
```

Pokémon
=======

More birdwatching
=================
