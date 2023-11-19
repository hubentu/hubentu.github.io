---
layout: post
date: 2019/12/24
tag:
  - tidytuesday
  - rstats
---

# TidyTuesday: WordCloud for Christmas Music Billboards

```r
library(tidyverse)
library(tidytext)
data(stop_words)
library(wordcloud2)

tuesdata <- tidytuesdayR::tt_load("2019-12-24")
lyrics <- tuesdata$christmas_lyrics
```

```r
lyc <- lyrics %>% unnest_tokens(word, lyric) %>%
    anti_join(stop_words) %>%
    count(word, sort = TRUE)
```

```
## Joining, by = "word"
```

```r
wordcloud2(lyc)
```

<iframeComp ihtml="/widgets/Christmas_song_wordcloud.html"></iframeComp>
