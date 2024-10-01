---
layout: page
title: Rcwl
description: R interface and toolchain for the Common Workflow Language
#img: assets/img/3.jpg
importance: 2
category: work
#giscus_comments: true
---

The `Rcwl/RcwlPipelines` package can be a simple and user-friendly way to manage command line tools and build data analysis pipelines in R using Common Workflow Language (CWL).

## Hello world!
```r
library(Rcwl)
inputs <- InputParamList(
    InputParam(id = "sth")
)
echo <- cwlProcess(baseCommand = "echo", inputs)
echo$sth <- "Hello World!"
res <- runCWL(echo)
```

## RcwlPipelines
```r
cwlUpdate()
cwlSearch("STAR")
STAR <- cwlLoad("tl_STAR")
```

## Project homepage
[rcwl.org](https://rcwl.org)

## Reference
<https://doi.org/10.1093/bioinformatics/btab208>