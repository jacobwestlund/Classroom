Day10
================

``` r
library(httr)
library(knitr)
library(tidyverse)
library(jsonlite)
library(listviewer)
```

a-pi
====

Write a function

``` r
# Function digits of pi
get_pi <- function(start, numberOfDigits){
  numberDigits <- start + numberOfDigits - 1
  url <- paste0("https://api.pi.delivery/v1/pi?start=", start, "&numberOfDigits=", numberDigits)  
  pi_response <- GET(url)
  pi_json <- content(pi_response, "text")
  as.numeric(fromJSON(pi_json)$content) 
}
```

``` r
# Example
get_pi(0,10)
```

    ## No encoding supplied: defaulting to UTF-8.

    ## [1] 3.141593

TimeEdit
========

What teacher spends most time in room 14?

``` r
# Get JSON data
schema_response <- GET("https://cloud.timeedit.net/su/web/stud1/ri107355X07Z07Q5Z76g0Y40y6076Y31Q09gQY5Q54777.json")
schema_json <- content(schema_response, "text")
schema_df <- fromJSON(schema_json)$reservations
schema_df %>% mutate(sal = map_chr(columns, 3), 
                     kurs = map_chr(columns, 1), 
                     tid = paste(starttime, endtime, sep = " - "),
                     lärare = map_chr(columns, 5)) %>% 
    select(kurs, datum = startdate, tid, sal, lärare) %>% 
    group_by(lärare) %>%
    summarise(`tillfällen sal 14` =n()) %>%
    arrange(desc(`tillfällen sal 14`), lärare) %>%
    kable()
```

| lärare                          |  tillfällen sal 14|
|:--------------------------------|------------------:|
| Anders Hagberg                  |                 32|
|                                 |                 18|
| Torbjörn Tambour                |                 17|
| Dan Petersen                    |                 14|
| Yishao Zhou                     |                 12|
| Wushi Goldring                  |                  7|
| Pavel B Kurasov                 |                  4|
| Tom Britton                     |                  4|
| Anna Montaruli                  |                  3|
| Håkan Granath                   |                  3|
| Ola G H Hössjer                 |                  3|
| Peter LeFanu Lumsdaine          |                  3|
| Erik Palmgren                   |                  2|
| Felix Wahl, Måns Karlsson       |                  2|
| Helena Flinck                   |                  2|
| Mitja Nedic                     |                  2|
| Samuel Lundqvist                |                  2|
| Annemarie Luger                 |                  1|
| Benjamin Allévius, Martin Sköld |                  1|
| Filip Lindskog                  |                  1|
| Gabriele Balletti               |                  1|
| Jacopo Emmenegger               |                  1|
| Jonathan Rohleder               |                  1|
| Kristofer Lindensjö             |                  1|
| Lukas Runsäter                  |                  1|
| Mathias Millberg Lindholm       |                  1|
| Sara Woldegiorgis               |                  1|
| Theresa Stocks                  |                  1|

SCB
===

Latest day at Bromma
====================
