---
layout: post
date: 2019/12/13
tag:
  - rstats
  - takehomessage
---

# Index blog topics with `rvest` and `fuzzywuzzyR` package

* Scrape blog titles and corresponding URLs by [`rvest`](https://rvest.tidyverse.org/).
* Fuzzy search topics by [fuzzywuzzyR](https://github.com/mlampros/fuzzywuzzyR).

To get the node paths of the titles, right-click one title, then inspect. In the Elements tab, right-click - copy - Copy selector.

```r
library(rvest)
library(purrr)
library(knitr)
th <- read_html("https://takehomessage.com")
th %>% html_nodes("#base-list-layout > div > div:nth-child(1) > div.ui-post-title > a")
```

```
## {xml_nodeset (1)}
## [1] <a href="/2019/12/13/rvest-index-blog-topics/" class="nav-link">Index blo ...
```

Let's scrape the useful contents.

```r
IndexBlog <- function(blog, pages){
    topics <- set_names(seq(pages)) %>%
        map_dfr(function(p){
            url <- ifelse(p == 1, blog,
                          paste0(blog, "/page/", p))
            th <- read_html(url) %>%
                html_nodes("#base-list-layout div.ui-post-title > a")
            urls <- th %>% html_attr("href")
            titles <- th %>% html_text()
            data.frame(titles = titles,
                       urls = paste0("[", titles, "](", urls, ")"),
                       stringsAsFactors = FALSE)
        })
    return(topics)
}
topics <- IndexBlog("https://takehomessage.com", 4)
kable(head(topics[,"urls",drop=F]))
```



|urls                                                                                                                    |
|:-----------------------------------------------------------------------------------------------------------------------|
|[Index blog topics with rvest and fuzzywuzzyR package](/2019/12/13/rvest-index-blog-topics/)                            |
|[TidyTuesday: Highcharter for time series data](/2019/12/11/tidytusday-highcharts-highstock/)                           |
|[Have some fun with JavaScript emoji library using the V8 R package.](/2019/12/10/r-package-v8/)                        |
|[How to read number column with comma as thousands separator in CSV file?](/2019/12/09/r-read-csv-thousands-separator/) |
|[Keeping structure when copy folder/file in Linux.](/2019/12/09/linux-copy-folder-with-structure/)                      |
|[How to include a htmlWidget to vuepress markdown?](/2019/12/06/iframe-component-htmlwidget/)                           |

A `howto` function to search the topics.

```r
library(fuzzywuzzyR)
howto <- function(keywords, blog = "https://takehomessage.com",
                  pages = 4, ...){
    topics <- IndexBlog(blog, pages)
    ms <- GetCloseMatches(string = keywords,
                          sequence_strings = topics$titles,
                          ...)
    topics[match(ms, topics$titles), "urls", drop=F]
}
```

```r
kable(howto("combine ggplot2", cutoff = 0.1, n = 2))
```



|   |urls                                                                                   |
|:--|:--------------------------------------------------------------------------------------|
|11 |[How to combine multiple ggplots?](/2019/12/02/combine-ggplots/)                       |
|10 |[How to share a legend for combined ggplot2 plots?](/2019/12/03/ggplot2-share-legend/) |
