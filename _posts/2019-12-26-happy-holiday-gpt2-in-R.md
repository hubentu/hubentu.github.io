---
layout: post
date: 2019/12/26
tag:
  - deeplearning
  - rstats
---

# Happy holiday from GTP-2 in R
[GTP-2](https://openai.com/blog/better-language-models/) is the state-of-the-art language model from OpenAI.

## Installation
Here we test the [`gpt2`](https://github.com/r-tensorflow/gpt2) package from rstudio.

```r
devtools::install_github("r-tensorflow/gpt2")
gpt2::install_gpt2(method = "conda", envname = "r-gpt2")
reticulate::use_condaenv(required = TRUE, condaenv = "r-gpt2")
library(gpt2)
```

## Text generation

```r
txt <- gpt2(prompt = "Iron Man: Happy holiday, Jarvis!",
            model = "355M",
            total_tokens = 100,
            top_k = 1)
cat(txt)
```

```
## 
## 
## Jarvis: I'm sorry, I'm just so excited to be here.
## 
## Tony Stark: I'm sure you're excited to be here.
## 
## Jarvis: I'm not.
## 
## Tony Stark: I'm sure you're excited to be here.
## 
## Jarvis: I'm not.
## 
## Tony Stark: I'm sure you're excited to be here.
## 
## Jarvis: I'm not.
## 
## Tony Stark: I'm sure you're
```


## Reference
https://blogs.rstudio.com/tensorflow/posts/2019-10-23-gpt-2/
