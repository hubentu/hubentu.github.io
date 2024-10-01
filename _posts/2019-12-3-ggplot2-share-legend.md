---
layout: post
date: 2019/12/3
tag:
  - ggplot2
  - rstats
author: Qiang
location: Buf
---

# How to share a legend for combined ggplot2 plots?

```r
library(ggplot2)
p1 <- ggplot(mpg) + geom_point(aes(x = class, y = hwy, color = drv))
p2 <- ggplot(mpg) + geom_boxplot(aes(x = class, y = hwy, fill = drv))
p3 <- ggplot(mpg) + geom_bar(aes(class, fill = drv))
```

## [patchwork](https://patchwork.data-imaginist.com/)

```r
library(patchwork)
(p1 + p2) / p3 + plot_layout(guides = "collect")
```

![plot of chunk patchwork12.3](/assets/figure/patchwork12.3-1.png)

## [cowplot](https://cran.r-project.org/web/packages/cowplot/vignettes/introduction.html)

```r
library(cowplot)
pw <- plot_grid(
    plot_grid(p1 + theme(legend.position="none"),
              p2 + theme(legend.position="none"), ncol = 2),
    p3 + theme(legend.position="none"), ncol = 1)
legend_b <- get_legend(p1 + theme(legend.position="bottom"))
plot_grid(pw, legend_b, ncol = 1, rel_heights = c(1, .1))
```

![plot of chunk cowplot12.3](/assets/figure/cowplot12.3-1.png)

## [ggpubr](https://rpkgs.datanovia.com/ggpubr/index.html)

```r
library(ggpubr)
ggarrange(plot_grid(p1 + theme(legend.position="none"),
                    p2 + theme(legend.position="none")),
          p3, ncol = 1, common.legend = TRUE, legend = "bottom")
```

![plot of chunk ggpubr12.3](/assets/figure/ggpubr12.3-1.png)

Ref: 
<https://stackoverflow.com/questions/13649473/add-a-common-legend-for-combined-ggplots>
