---
layout: post
date: 2019/12/20
tag:
  - rstats
  - flowchart
---

# Packages to draw flowchart (christmas tree) in R
## [Graphviz](https://rich-iannone.github.io/DiagrammeR/graphviz_and_mermaid.html)

```r
library(DiagrammeR)
f1 <- grViz("
digraph xmas {
  # graph satement
  graph [overlap = true, fontsize = 10]

  # node statement
  node [shape = triangle, style = filled, fillcolor = darkgreen]
  B; C; D; E; F; G
  A [shape = star, fillcolor = gold, fontsize = 5]
  H [shape = box, fillcolor = brown]

  # edge
  edge [arrowhead = dot, color = gold]
  A -> B
  B -> {C D}
  C -> {E F}
  D -> {F G}
  F -> H
}
")
```

<iframeComp ihtml="/widgets/xmas_grViz.html"></iframeComp>

## [mermaid](https://rich-iannone.github.io/DiagrammeR/graphviz_and_mermaid.html)
Could anyone tell me how to use star/triangle, change font size, define class?
```r
f2 <- mermaid("
graph TB
  A((A)) --> B{B}
  B{B} --> |O|C{C}
  B{B} --> |O|D{D}
  C{C} --> |O|E{E}
  C{C} --> |O|F{F}
  D{D} --> |O|F{F}
  D{D} --> |O|G{G}
  F{F} --- H[H]

  style A fill:gold
  style H fill:brown
  style B fill:green
  style C fill:green
  style D fill:green
  style E fill:green
  style F fill:green
  style G fill:green
")
```

<iframeComp ihtml="/widgets/xmas_mermaid.html"></iframeComp>

## [visNetwork](https://datastorm-open.github.io/visNetwork/)
Most R-friendly way to draw a flowchart.
```r
library(visNetwork)
nodes <- data.frame(id = LETTERS[1:8],
                    label = LETTERS[1:8],
                    color = c("gold", rep("darkgreen", 6), "brown"),
                    shape = c("star", rep("triangle", 6), "box"))
edges <- data.frame(from = c("A", "B", "B", "C", "C", "D", "D", "F", "E", "G"),
                    to = c("B", "C", "D", "E", "F", "F", "G", "H", "F", "F"))
f3 <- visNetwork(nodes, edges) %>% visLayout(7)
```

<iframeComp ihtml="/widgets/xmas_vis.html"></iframeComp>



