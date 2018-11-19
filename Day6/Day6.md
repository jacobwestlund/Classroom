Day6
================
2018-11-19

``` r
library(tidyverse)
library(knitr)
```

More dental care
================

``` r
# Load data
dental_data <- read_csv2("../Class_files/Statistikdatabasen_2018-01-23 14_46_26.csv", skip = 1, n_max = 580)
#pop_data <- read_csv2("../Class_files/BE0101A5.csv")
```

Sytembolaget
============

We will know look at changes to Sytembolaget's assortment. We start by loading data and create dataframes for new beverages and beverages that have been removed from the assortment.

``` r
# Load data 
sortiment_11_19 <- read_csv("systembolaget2018-11-19.csv")
sortiment_10_08 <- read_csv("../Class_files/systembolaget2018-10-08.csv")

# Dataframe of newly added beverages
sortiment_nytt <- setdiff(sortiment_11_19, sortiment_10_08)

# Dataframe newly removed beverages
sortiment_borta <- setdiff(sortiment_10_08, sortiment_11_19)
```

Nowthat we have created dataframes. Let us look at what types of beverages have been removed and what have been added.

``` r
# List 5 categories with most added beverages
new_table <- sortiment_nytt %>% 
  group_by(Varugrupp) %>%
  summarize(num_added = n()) %>%
  arrange(desc(num_added), Varugrupp) %>%
  select(Varugrupp, num_added) %>%
  head(5) %>% 
  kable(col.names = c("Varugrupp", "Beverages added"))

# List 5 categories with most removed beverages
removed_table <- sortiment_borta %>%
  group_by(Varugrupp) %>%
  summarize(num_removed = n()) %>%
  arrange(desc(num_removed), Varugrupp) %>%
  select(Varugrupp, num_removed) %>%
  head(5) %>% 
  kable(col.names = c("Varugrupp", "Beverages removed"))
```

### Five categories with most new beverages

| Varugrupp       |  Beverages added|
|:----------------|----------------:|
| Rött vin        |              604|
| Vitt vin        |              339|
| Öl              |              269|
| Whisky          |              203|
| Mousserande vin |              133|

### Five categories with most removed beverages

| Varugrupp       |  Beverages removed|
|:----------------|------------------:|
| Rött vin        |                598|
| Vitt vin        |                335|
| Öl              |                293|
| Whisky          |                203|
| Mousserande vin |                108|

Pokémon
=======

More birdwatching
=================
