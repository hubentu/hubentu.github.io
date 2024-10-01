---
layout: post
date: 2019/12/17
tag:
  - rstats
  - tidytuesday
---

# TidyTuesday: adoptable dogs with geom_tile and geofacet 

```r
library(tidyverse)
library(geofacet)
library(knitr)
opts_knit$set(base.url = "/")

dog_moves <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-12-17/dog_moves.csv')
dog_ie <- dog_moves %>% filter(inUS) %>%
    mutate(location = replace(location,
                              location=="Washington DC",
                              "District of Columbia")) %>%
    select(location, exported, imported, total)
```


```r
## block per dog
tc <- dog_ie %>% pull(total) %>% max %>% sqrt %>% ceiling
dog_blocks <- dog_ie %>% select(location, exported, imported, total) %>%
    pmap_dfr(function(location, exported, imported, total){
        df <- data.frame(state = rep(location, tc^2),
                         x = rep(seq(tc), each = tc),
                         y = rep(seq(tc), tc),
                         z = NA,
                         stringsAsFactors = FALSE)
        df[seq(total), "z"] <- "total"
        if(!is.na(imported)){
            df[seq(imported), "z"] <- "imported"
        }
        if(!is.na(exported)){
            df[tail(seq(tc^2), exported), "z"] <- "exported"
        }
        na.omit(df)
    })

dog_blocks %>% filter(state == "New York") %>%
    ggplot() + geom_tile(aes(x, y, fill=z), width=0.8, height=0.8)
```

![plot of chunk dogs_geofacet](/assets/figure/dogs_geofacet-1.png)

```r
ggplot(dog_blocks) +
    geom_tile(aes(x, y, fill=z), width=0.8, height=0.8) +
    theme_bw() + theme(legend.position = "none") +
    labs(x = "", y = "") +
    facet_geo( ~ state, grid = "us_state_grid2", label = "code") +
    theme_minimal() +
    theme(legend.position = "top",
          axis.title.x=element_blank(),
          axis.text.x=element_blank(),
          axis.ticks.x=element_blank(),
          axis.title.y=element_blank(),
          axis.text.y=element_blank(),
          axis.ticks.y=element_blank()) +
    labs(x = "", y = "", title = "Adoptable dogs (imported, exported and total numbers)", fill = "")
```

![plot of chunk dogs_geofacet](/assets/figure/dogs_geofacet-2.png)
