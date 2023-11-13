---
layout: page
title: sedatasets
description: a project to bring Bioconductor datasets to AI area
#img: assets/img/12.jpg
importance: 1
category: fun
#related_publications: einstein1956investigations, einstein1950meaning
---

Python and R/Bioconductor packages `sedatasets` are developped to transfer `SummarizedExperiment` data structure to AI-friendly Huggingface `datasets` format.

## Usage
### Command line
```bash
python -m sedatasets.cli -h
```

### Python Module
```python
from sedatasets.se_convert import AD2Datasets, SE2Datasets

SE2Datasets(
    efiles={"exp": "tests/data/rse_counts.csv"},
    pfile="tests/data/rse_cdata.csv",
    ffile="tests/data/rse_rdata.csv",
    outdir='/tmp/rse',
)

AD2Datasets("tests/data/adata.h5ad", outdir='/tmp/anndata')
```
