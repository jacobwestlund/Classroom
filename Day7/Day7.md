Day7
================

``` r
library(DBI)
library(tidyverse)
```

SL SQL
======

Let us start by connecting to sthlm\_metro database.

``` r
con <- dbConnect(RSQLite::SQLite(), "../Class_files/sthlm_metro.sqlite")
dbListTables(con)
```

    ## [1] "Line"         "LinePlatform" "Platform"

We that the database contains three different tables. To answer the question how they relate to eachother, let us load some rows from each.

``` r
dbGetQuery(con, "SELECT * FROM Line LIMIT 3")   # Line table
```

    ##   LineNumber                LineName
    ## 1         10  tunnelbanans blå linje
    ## 2         11  tunnelbanans blå linje
    ## 3         13 tunnelbanans röda linje

``` r
dbGetQuery(con, "SELECT * FROM LinePlatform LIMIT 3")   # LinePlatform table
```

    ##   LineNumber PlatformNumber Direction
    ## 1         10           3031         1
    ## 2         10           3051         1
    ## 3         10           3131         1

``` r
dbGetQuery(con, "SELECT * FROM Platform LIMIT 3")   # Platform table
```

    ##   PlatformNumber StationName Longitude Latitude
    ## 1           1011     Slussen  18.07174 59.31960
    ## 2           1012     Slussen  18.07174 59.31958
    ## 3           1021  Gamla stan  18.06716 59.32340

We can know see how they connect to eachother:

-   `Line` connects to `LinePlatform` through `LineNumber`.
-   `Platform` connects to `LinePlatform` through `PlatformNumber`.
-   `Line` does not connect to `Platform`.

**The first task is to query for the LineName from table Line where LineNumber is 18.**

``` r
line_name_18 <- dbGetQuery(con, "SELECT LineName FROM Line WHERE LineNumber = 18") %>%
  pull()
```

The line name of line number 18 is *"tunnelbanans gröna linje"*. Let us move on to the next task.

**Query for the number of stops on LineNumber 18 Direction 1.**

``` r
count_stops <- dbGetQuery(con, "SELECT COUNT(PlatformNumber) FROM LinePlatform WHERE LineNumber = 18 AND Direction = 1") %>%
  pull()
```

Line number 18 has *37* stops in direction 1.

**Query for the five most southern StationName, where position is measured as the average Latitude of its PlatformNumber.**

``` r
southern_stations <- dbGetQuery(con, "SELECT StationName, AVG(Latitude) as avgLatitude 
                                FROM Platform 
                                GROUP BY StationName 
                                ORDER BY avgLatitude
                                LIMIT 5")

knitr::kable(southern_stations, col.names = c("Station", "Latitude"))
```

| Station       |  Latitude|
|:--------------|---------:|
| Farsta strand |  59.23552|
| Alby          |  59.23950|
| Hallunda      |  59.24333|
| Norsborg      |  59.24379|
| Farsta        |  59.24478|

The stations that we are given look reasonable. At least they are all south of Stockholm.

**Query for all StationName on LineNumber 18 Direction 1.**

``` r
dbGetQuery(con, "SELECT DISTINCT(StationName)
           FROM Platform 
           LEFT JOIN LinePlatform ON Platform.PlatformNumber = LinePlatform.PlatformNumber
           WHERE LineNumber = 18 AND Direction = 1
           ORDER BY StationName")
```

    ##         StationName
    ## 1      Abrahamsberg
    ## 2             Alvik
    ## 3        Blackeberg
    ## 4            Blåsut
    ## 5        Brommaplan
    ## 6            Farsta
    ## 7     Farsta strand
    ## 8      Fridhemsplan
    ## 9        Gamla stan
    ## 10        Gubbängen
    ## 11     Gullmarsplan
    ## 12    Hässelby gård
    ## 13  Hässelby strand
    ## 14       Hökarängen
    ## 15         Hötorget
    ## 16    Islandstorget
    ## 17      Johannelund
    ## 18     Kristineberg
    ## 19 Medborgarplatsen
    ## 20         Odenplan
    ## 21          Råcksta
    ## 22     Rådmansgatan
    ## 23    S:t Eriksplan
    ## 24        Sandsborg
    ## 25        Skanstull
    ## 26 Skogskyrkogården
    ## 27     Skärmarbrink
    ## 28          Slussen
    ## 29     Stora mossen
    ## 30      T-Centralen
    ## 31       Tallkrogen
    ## 32     Thorildsplan
    ## 33        Vällingby
    ## 34        Ängbyplan
    ## 35          Åkeshov

End by disconnect from database.

``` r
dbDisconnect(con)
```
