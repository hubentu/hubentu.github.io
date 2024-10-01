---
layout: post
date: 2019/12/9
tag:
  - rstats
  - python
---

# How to read number column with comma as thousands separator in CSV file?

The `readr::read_csv` function with `locale` option is able to parse numbers in different format.


```r
library(readr)
read_csv("x,y\n123,\"4,567\"", locale = locale(grouping_mark = ","))
```

```
## # A tibble: 1 x 2
##       x     y
##   <dbl> <dbl>
## 1   123  4567
```

A `thousands` option is available in the python pandas package.
```python
import pandas as pd
pd.read_csv(file, thousands = ",")
```

Ref: <https://r4ds.had.co.nz/data-import.html>
