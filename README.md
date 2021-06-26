
<!-- README.md is generated from README.Rmd. Please edit that file -->

# AgavePrediction

This repository contains the data and code for our paper:

> Authors, (YYYY). *Title of your paper goes here*. Name of journal/book
> <https://doi.org/xxx/xxx>

Our pre-print is online here:

> Sarah C. Davis1, John T. Abatzoglou, David S. LeBauer, (YYYY).
> “Expanded potential growing region and yield increase for Agave
> americana with future climate”. Journal Online at
> <https://doi.org/xxx/xxx>

## Data Sources

**Climate data used as model inputs**

Abatzoglou, J.T., S.Z. Dobrowski, S.A. Parks, K.C. Hegewisch, 2018,
Terraclimate, a high-resolution global dataset of monthly climate and
climatic water balance from 1958-2015, Scientific Data,

**Urban and protected areas excluded from biomass predictions**

The following data was used to exclude present day 1) Urban Areas (Kelso
and Patterson 2010; South 2017) and 2) Protected Areas (UNEP-WCMC and
IUCN, 2021)

**Urban Areas** Andy South (2017). rnaturalearth: World Map Data from
Natural Earth. R package version 0.1.0.
<https://CRAN.R-project.org/package=rnaturalearth>

Kelso, Nathaniel Vaughn, and Tom Patterson. “Introducing natural earth
data-naturalearthdata.com.” Geographia Technica 5.82-89 (2010): 25.

**Protected Areas**

World Database of Protected Areas

  - `analysis/data/raw_data/WDPD_WDOECM_Apr2021`
  - `analysis/data/derived_data/wdpa.gpkg`

UNEP-WCMC and IUCN (2021), Protected Planet: The World Database on
Protected Areas (WDPA) and World Database on Other Effective Area-based
Conservation Measures (WD-OECM) \[Online\], April 2021, Cambridge, UK:
UNEP-WCMC and IUCN. Available at: www.protectedplanet.net.

## Model Description

\[\alpha=\] \[\beta=\] \[\gamma=\] \[\textrm{EPI}=\]
\[\textrm{Biomass}=\]

### Simulating Rock Mulch

\[\beta_{\textrm{rock mulch}}=\beta * 1.83\] \#\#\# Simulating
Irrigation

Calculate monthly water demand: how much precipitation is required for
\(\beta\gte 1\)?

Given \(\beta = 0.0279*\textrm{ppt}-0.2851\)

\[1 = 0.0279*\textrm{ppt}-0.2851\]
\[\textrm{ppt_{\beta=1}}=\sfrac{1+0.2851}{0.0279}\simeq= 46.06093\]

  - for any location where absmintemp \> -10,
      - for each month if
          - alpha (light factor) \> 0 AND
          - gamma (temperature factor) \> 0 AND
          - ppt \< 46.061mm
      - calculate irrigation required = water demand (46.01 mm) - ppt
      - annual irrigation demand =
        \(\sum_{jan}^{dec}46.01 - \textrm{ppt}\) calculate sum over all
        months

### How to cite

TODO

## Contents

The **analysis** directory contains:

  - [:file\_folder: data](/analysis/data): Data used in the analysis.
      - [:file\_folder: raw\_data](/analysis/data/raw_data): Inputs
      - [:file\_folder: derived\_data](/analysis/data/derived_data):
        Outputs
  - [:file\_folder: figures](/analysis/figures): Plots and Panoply
    (plotting software) configuration files.

## How to run in your broswer or download and run locally

This research compendium has been developed using the statistical
programming language R. To work with the compendium, you will need
installed on your computer the [R
software](https://cloud.r-project.org/) itself and optionally [RStudio
Desktop](https://rstudio.com/products/rstudio/download/).

The simplest way to explore the text, code and data is to click on
[binder](https://mybinder.org/v2/gh/az-digitalag/agave-prediction/master?urlpath=rstudio)
to open an instance of RStudio in your browser, which will have the
compendium files ready to work with. Binder uses rocker-project.org
Docker images to ensure a consistent and reproducible computational
environment. These Docker images can also be used locally.

### Licenses

**Text and figures :**
[CC-BY-4.0](http://creativecommons.org/licenses/by/4.0/)

**Code :** See the [DESCRIPTION](DESCRIPTION) file

**Data :** [CC-0](http://creativecommons.org/publicdomain/zero/1.0/)
attribution requested in reuse

### Contributions

We welcome contributions from everyone. Before you get started, please
see our [contributor guidelines](CONTRIBUTING.md). Please note that this
project is released with a [Contributor Code of Conduct](CONDUCT.md). By
participating in this project you agree to abide by its terms.
