---
layout: post
date: 2019/12/4
tag:
  - rstats
---

# R function [`force`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/force)

When to use `force` function?

R function arguments are Lazy Evaluated. It causes confuse when function return a function in `for` a loop.

```r
f <- function(x) function() x
```

The function `f` will evaluate arguments in the end of the loop.

```r
lf <- vector("list", 5)
for (i in seq(lf)) lf[[i]] <- f(i)
lf[[1]]()
```

```
## [1] 5
```

The `force` function make sure the argument evaluated in each loop.

```r
f1 <- function(x) {
    force(x)
    function()x
}
for (i in seq(lf)) lf[[i]] <- f1(i)
lf[[1]]()
```

```
## [1] 1
```

The `lapply` function seems not lazy evaluated.

```r
lr <- lapply(seq(5), f)
lr[[1]]()
```

```
## [1] 1
```

Ref: <https://gist.github.com/peterhurford/d86331d8acdc99910de9>
