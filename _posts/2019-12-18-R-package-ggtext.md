---
layout: post
date: 2019/12/18
tag:
  - ggplot2
  - rstats
---

# Add markdown/html rich text to ggplot2 using ggtext

## Install [`ggtext`](https://github.com/clauswilke/ggtext)


```r
devtools::install_github("clauswilke/ggtext")
```

## Example to style title with both markdown and html

* Add **bold** and *italic* to different words in title.
* Add different colors to different words
* Add image to title


```r
library(ggplot2)
library(ggtext)

ggplot(mtcars) + geom_boxplot(aes(as.factor(vs), mpg, fill = as.factor(am))) +
    labs(title = "**mtcars**
<img src='https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/Sketchup_car.png/320px-Sketchup_car.png' width='50'></img>
: *mpg* _by_
<i style='color:blue'>vs</i> and
<strong style='color:red'>am</strong>") +
    theme(plot.title = element_markdown())
```

```
## Warning in e1[n] <- e2[n]: number of items to replace is not a multiple of
## replacement length
```

![plot of chunk ggtext_title](/assets/figure/ggtext_title-1.png)

More examples in [`ggtext`](https://github.com/clauswilke/ggtext)
