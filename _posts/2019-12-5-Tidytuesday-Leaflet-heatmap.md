---
layout: post
date: 2019/12/5
tag:
  - tidytuesday
  - rstats
---

# TidyTuesday: Leaflet heatmap for Philadelphia Parking Violations


```r
library(tidyverse)
library(leaflet)
library(leaflet.extras)
tickets <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-12-03/tickets.csv")
```


```r
llm <- tickets %>% summarise(lat_m = mean(lat), lon_m = mean(lon))

leaflet(tickets) %>%
    addProviderTiles(providers$CartoDB.Positron) %>%
    setView(llm$lon_m, llm$lat_m, 11) %>%
    addHeatmap(lng = ~lon, lat = ~lat, intensity = ~fine,
               blur = 20, max = 0.05, radius = 15)
```

![plot of chunk leaft_heatmap](/assets/figure/leaft_heatmap-1.png)

Ref: 

1. <https://rpubs.com/bhaskarvk/leaflet-heat>
2. <https://github.com/hubentu/tidytuesday/tree/master/data/2019/2019-12-03>
