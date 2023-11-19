---
layout: post
date: 2020/01/07
tag:
  - rstats
---

# Virtual environment for R development by conda

Sometimes we need mutliple versions of R for different development
(R-devel) and data analysis (R-release) purposes. Here is one of the
solutions by python package manager conda. We can maintain mutliple
versions of R and specific packages in different conda environments.

## For released versions
```sh
conda config --add channels conda-forge
conda create -n r-3.6
conda activate r-3.6
conda search r-base
conda install r-base=3.6.2
```

```sh
Rscript -e "R.version.string"
## [1] "R version 3.6.2 (2019-12-12)"
conda deactivate
```

## For development version (R-4.0.0)
* Prepare recipe for R-4.0.0
```sh
git clone https://github.com/hubentu/conda-r-devel.git
cd conda-r-devel
conda create -n r-devel
conda activate r-devel
```

* Build and install from source
```sh
conda install conda-build
conda build .
conda install --use-local path/to/r-devel-gz
```

Or just install the compiled version.
```
conda install --use-local r-devel-4.0.0-0.tar.bz2
conda install readline
```

* Test
```sh
Rscript -e "R.version.string"
## [1] "R Under development (unstable) (2020-01-07 r77631)"
```

Via conda environments, we can switch different version of R by `conda
activate renv`.
