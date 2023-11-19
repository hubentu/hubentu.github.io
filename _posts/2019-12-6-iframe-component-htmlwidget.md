---
layout: post
date: 2019/12/6
tag:
  - markdown
  - vuepress
---

# How to include a htmlWidget to vuepress markdown?

First, write a `iframeComp` component to insert iframe in `.vuepress/components/iframeComp.vue`.
```vue
<template>
    <iframe width="600" height="600" :src="ihtml"></iframe>
</template>

<script>
  export default {
      props: {
          ihtml: {
              type: String
          }
      }
  }
</script>
```

Then use it. A htmlWidget from [leaflet heatmap](/2019/12/05/tidytuesday-leaflet-heatmap) was save to html
using `htmlwidgets::saveWidget` and copy to `/widgets`.

Here is the input code in the markdown.
```md
<iframeComp ihtml="/widgets/leaft_heatmap.html"></iframeComp>
```

Here is the output.
<iframeComp ihtml="/widgets/leaft_heatmap.html"></iframeComp>
