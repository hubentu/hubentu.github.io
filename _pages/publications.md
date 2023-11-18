---
layout: page
permalink: /publications/
title: publications
description: Selected publications per year since 2020.
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->

* [Google scholar](https://scholar.google.com/citations?user={{ site.data.scholar.id }})
* Citations: {{ site.data.scholar.citations }}
* h-index: {{ site.data.scholar.h_index }}
* i10-index: {{ site.data.scholar.i10_index }}

<div class="publications">

{% bibliography -f {{ site.scholar.bibliography }} %}

</div>
