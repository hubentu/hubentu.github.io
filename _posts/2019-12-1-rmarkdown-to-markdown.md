---
layout: post
date: 2019/12/1
tag:
  - rstats
author: Qiang
---

# How to convert Rmarkdown (Rmd) to markdown (md)?

```r
knitr:knit("rmarkdown.Rmd")
```

Or

```r
library(rmarkdown)
render("rmarkdown.Rmd", md_document("markdown_github"))
```
