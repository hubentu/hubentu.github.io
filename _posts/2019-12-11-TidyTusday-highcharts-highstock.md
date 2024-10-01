---
layout: post
date: 2019/12/11
tag:
  - tidytuesday
  - rstats
---

# TidyTuesday: Highcharter for time series data

## Load the diseases data

```r
library(tidyverse)
library(highcharter)
diseases <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-12-10/diseases.csv")
```

## Plot with highcharter stock type

```r
rates <- diseases %>% group_by(disease, year) %>%
    summarise(rate = sum(count, na.rm = T) / sum(population, na.rm = T) * 1e6) %>%
    pivot_wider(names_from = disease, values_from = rate) %>%
    mutate(year = as.Date(as.character(year), format = "%Y"))
rates
```

```
## # A tibble: 84 x 8
##    year       `Hepatitis A` Measles Mumps Pertussis Polio Rubella Smallpox
##    <date>             <dbl>   <dbl> <dbl>     <dbl> <dbl>   <dbl>    <dbl>
##  1 1966-12-12          167.  1036.    NA       0     0      231.        NA
##  2 1967-12-12          195.   302.    NA       0     0      215.        NA
##  3 1968-12-12          228.   115.   718.      0     0      241.        NA
##  4 1969-12-12          229.   120.   405.      0     0      262.        NA
##  5 1970-12-12          272.   225.   477.      0     0      265.        NA
##  6 1971-12-12          287.   350.   556.      0     0      207.        NA
##  7 1972-12-12          234.   145.   316.      0     0      111.        NA
##  8 1973-12-12          231.   122.   298.      0     0      130.        NA
##  9 1974-12-12          179.    99.1  259.      7.65  7.65    52.0       NA
## 10 1975-12-12          156.   111.   269.      6.46  6.46    72.5       NA
## # … with 74 more rows
```

```r
hc <- highchart(type = "stock") %>%
    hc_title(text = "Disease rate per million in U.S.") 
walk(colnames(rates)[-1], function(dis){
    dat <- rates %>% select(year, dis) %>%
        column_to_rownames("year") %>% as.xts
    hc <<- hc %>% hc_add_series(dat, name = dis)
})
```

<iframeComp ihtml="/widgets/highcharter_diseases.html"></iframeComp>
