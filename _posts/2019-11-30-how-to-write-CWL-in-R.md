---
layout: post
date: 2019/11/30
tag:
  - CWL
  - rstats
  - Bioinformatics
author: Qiang
---

# How to write CWL in R?
Here is an example to wrap `echo` in Common Workflow Language (CWL) using the [`Rcwl`](https://bioconductor.org/packages/release/bioc/html/Rcwl.html) package.


```r
library(Rcwl)
input1 <- InputParam(id = "sth")
echo <- cwlParam(baseCommand = "echo", inputs = InputParamList(input1))
echo$sth <- "Hello World!"
echo
```

```
## class: cwlParam 
##  cwlClass: CommandLineTool 
##  cwlVersion: v1.0 
##  baseCommand: echo 
## inputs:
##   sth (string):  Hello World!
## outputs:
## output:
##   type: stdout
```

To write echo to CWL and YML.

```r
out <- tempfile()
writeCWL(echo, out)
cat(readLines(paste0(out, ".cwl")), sep = "\n")
```

```
## cwlVersion: v1.0
## class: CommandLineTool
## baseCommand: echo
## inputs:
##   sth:
##     type: string
##     inputBinding:
##       separate: true
## outputs:
##   output:
##     type: stdout
```

```r
cat(readLines(paste0(out, ".yml")), sep = "\n")
```

```
## sth: Hello World!
```

Run CWL directly in R.
```
runCWL(echo)
```

More tutorial: <https://hubentu.github.io/Rcwl>
