---
layout: post
date: 2019/12/14
tag:
  - rstats
---

# R fun package: cowsay 💃 📦

```r
library(cowsay)
say("fortune", by = "cow")
```

```
##  ----- 
## Can one be a good data analyst without being a half-good programmer? The short answer to that is, 'No.' The long answer to that is, 'No.'
##  Frank Harrell
##  1999 S-PLUS User Conference, New Orleans
##  October 1999 
##  ------ 
##     \   ^__^ 
##      \  (oo)\ ________ 
##         (__)\         )\ /\ 
##              ||------w|
##              ||      ||
```

Similar in bash
```sh
$fortune | cowsay
 ________________________________
/ question = ( to ) ? be : ! be; \
|                                |
\ -- Wm. Shakespeare             /
 --------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

Use [`howto`](/2019/12/13/rvest-index-blog-topics/) function.

```r
say(paste("Q: How to use javascript emoji?\nA: ",
          howto("javascript emoji", cutoff=0.1)[1,1]))
```

```
## 
##  -------------- 
```
Q: How to use javascript emoji?

A:  [Have some fun with JavaScript emoji library using the V8 R package.](/2019/12/10/r-package-v8/) 

```
##  --------------
##     \
##       \
##         \
##             |\___/|
##           ==) ^Y^ (==
##             \  ^  /
##              )=*=(
##             /     \
##             |     |
##            /| | | |\
##            \| | |_|/\
##       jgs  //_// ___/
##                \_)
## 

```
