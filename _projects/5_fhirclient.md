---
layout: page
title: fhiRclient
description: R interface to the FHIR standards for healthcare data
img:
importance: 3
category: fun
---

# [fhiRclient](https://github.com/hubentu/fhiRclient)

R FHIR client based on the python `fhirclient` library with more features.

Major feature:

* All CRUD function.
* Constructor for Resource data models.
* Search and bundle sets in tidy.

## Installation

Install the latest python library `fhirclient` first.

``` bash
pip install git+https://github.com/smart-on-fhir/client-py.git
```

You can install the released version of fhiRclient from [github](https://github.com/qhu75/fhiRclient) with:

``` r
## install.packages("fhiRclient")
devtools::install_github("qhu75/fhiRclient")
```

## Example

``` r
library(fhiRclient)
```

* Read
``` r
ct <- Client(app_id = "my_app",
             api_base = "https://r4.smarthealthit.org")
pt <- Patient(list(id = "326b4675-0bc8-4dbd-b406-a5564c282401"))
ct %>% Read(pt) %>% as_json %>% as_json_tbl
```

* Search
``` r
res <- Search(ct, "observation", "Observation",
              list(code = "http://loinc.org|2339-0"), page = "all")
```

* More examples
``` r
browseVignettes("fhiRclient")
```

* Shiny demo
``` r
ShinyDemo()
```
