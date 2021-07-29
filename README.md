
# Future Agave

This repository contains the code, code documentation, and data
description for our paper:

> Sarah C. Davis, John T. Abatzoglou, David S. LeBauer, (2021).
> “Expanded potential growing region and yield increase for Agave
> americana with future climate”. *Submitted to Agronomy*

At time of submission, data are [available on
Box](https://arizona.box.com/s/kjecv66pe88gtwdw9h20u0yvbw5thzmu). They
will be moved to a repository at the time of publication.

## Data Sources

### Climate data used as model inputs

-   **TERRACLIMATE** Abatzoglou, J.T., S.Z. Dobrowski, S.A. Parks, K.C.
    Hegewisch, 2018, Terraclimate, a high-resolution global dataset of
    monthly climate and climatic water balance from 1958-2015,
    Scientific Data

### Urban and protected areas

The following data was used to exclude present day 1) Urban Areas (Kelso
and Patterson 2010; South 2017) and 2) Protected Areas (UNEP-WCMC and
IUCN, 2021) from regional predictions of suitable land area and
potential biomass:

-   **Natural Earth**
    -   Andy South (2017). rnaturalearth: World Map Data from Natural
        Earth. R package version 0.1.0.
        <https://CRAN.R-project.org/package=rnaturalearth>
    -   Kelso, Nathaniel Vaughn, and Tom Patterson. “Introducing natural
        earth data-naturalearthdata.com.” Geographia Technica 5.82-89
        (2010): 25.
-   **Protected Areas** UNEP-WCMC and IUCN (2021), Protected Planet: The
    World Database on Protected Areas (WDPA) and World Database on Other
    Effective Area-based Conservation Measures (WD-OECM) \[Online\],
    April 2021, Cambridge, UK: UNEP-WCMC and IUCN. Available at:
    www.protectedplanet.net.
    -   For convenience the aggregated, OGC-compliant geopackage format
        of this database is provided in the file
        `derived_data/wdpa.gpkg`

## Software

**NetCDF Operators (Zender 2014)** Zender, C. S. (2014), netCDF Operator
(NCO) User Guide, Version 4.4.3, <http://nco.sf.net/nco.pdf>.

**R** R Core Team (2018). R: A language and environment for statistical
computing. R Foundation for Statistical Computing, Vienna, Austria. URL
<https://www.R-project.org/>.

**fasterize** Noam Ross (2020). fasterize: Fast Polygon to Raster
Conversion. R package version 1.0.3.
<https://CRAN.R-project.org/package=fasterize>

**rnaturalearth** Andy South (2017). rnaturalearth: World Map Data from
Natural Earth. R package version 0.1.0.
<https://CRAN.R-project.org/package=rnaturalearth>

**sf** Pebesma, E., 2018. Simple Features for R: Standardized Support
for Spatial Vector Data. The R Journal 10 (1), 439-446,
<https://doi.org/10.32614/RJ-2018-009>

**exactextractr** Daniel Baston (2021). exactextractr: Fast Extraction
from Raster Datasets using Polygons. R package version 0.6.0.
<https://CRAN.R-project.org/package=exactextractr>

**Panoply** PanoplyCL v 4.12.8 b0071 downloaded from
<https://www.giss.nasa.gov/tools/panoply/download/beta1f/> can be used
to recreate figures, see `.pcl` files in figures directory

## Contents

The **analysis** directory contains:

-   **Scripts**
    -   `01-download_climate.R` downloads the TERRA-Climate data to the
        `raw_data` directory.
    -   `01.1-combine_masks.R` combines urban and protected area
        polygons and generates polygons stored in
        `/data/derived_data/not_suitable.gpkg`
    -   `0.2-append_terraclimate.sh` combines TERRA-CLIMATE data into a
        single file for each scenario `TerraClimate4C.nc` and
        `TerraClimate19812010.nc`
    -   `03-compute.biomass.sh` implements the model under different
        climate scenarios with rainfed, irrigation, and rock mulch
        scenarios (described below)
    -   `04-update_nc_metadata.sh` corrects metadata in output netcdf
        files
-   **Data**
-   [/analysis/data/raw\_data](/analysis/data/raw_data): Inputs
    -   TERRACLIMATE files must be downloaded to this folder using the
        `01-download_climate.R` script
    -   WPDA Shapefiles can be downloaded from www.protectedplanet.net
-   [/analysis/data/derived\_data](/analysis/data/derived_data): Outputs
    available - `coefs19812010.nc` calculated values of
    ![\\alpha](https://latex.codecogs.com/png.latex?%5Calpha "\alpha"),
    ![\\beta](https://latex.codecogs.com/png.latex?%5Cbeta "\beta"), and
    ![\\Gamma](https://latex.codecogs.com/png.latex?%5CGamma "\Gamma")
    values for each grid cell under 1981-2010 climate normals for three
    water availability scenarios - `coefs4C.nc` same as above but for
    ![+4^\\circ C](https://latex.codecogs.com/png.latex?%2B4%5E%5Ccirc%20C "+4^\circ C")
    warming - `allbiomass.nc` predicted annual biomass increments for
    *Agave americana* under all climates and scenarios -
    `biomassdiff.nc` differences between current and future biomass -
    `not_suitable.gpkg` polygons combining land classified as unsuitable
    for growing that is either protected (UNEP-WCMC and IUCN 2021) or
    urbanized (Kelso and Patterson 2010)
-   [analysis/figures](/analysis/figures): Plots and Panoply (plotting
    software) configuration files.
    -   contains all map figures generated using Panoply.
    -   `*.pcl` files provide information about Panoply software
        settings used to generate the figures. In addition, these files
        can regenerate the figures with the command
        `java -jar PanoplyCL.jar [filename].pcl` using a `PanoplyCL.jar`
        that is available from the Panoply software author [Dr. Robert
        Schmunk](https://science.gsfc.nasa.gov/sed/bio/robert.b.schmunk).

## Model Description

The EPI model is implemented using NetCDF Operators in the file
[analysis/03-compute.biomass.sh](analysis/03-compute.biomass.sh). The
key equations of the model are provided below for reference; a full
description is provided in Niechayev et al (2019) and the manuscript
associated with this archive Davis et al (*submitted*).

> Niechayev, N., Jones, A., Rosenthal, D., & Davis, S. (2019). A model
> of environmental limitations on production of Agave americana L. grown
> as a biofuel crop in semi-arid regions. Journal of Experimental
> Botany, 70, 6549-6559.

**light index
![\\alpha](https://latex.codecogs.com/png.latex?%5Calpha "\alpha"):**

![
\\alpha=
\\left\\{\\begin{matrix}
0,                                   & \\textrm{for } I&lt; 0.6252 \\\\ 
-7\\times10^{-7}I^2+0.0016\\;I-0.0001, & \\textrm{for } I&gt; 0.6252 
\\end{matrix}\\right.
](https://latex.codecogs.com/png.latex?%0A%5Calpha%3D%0A%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%0A0%2C%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%26%20%5Ctextrm%7Bfor%20%7D%20I%3C%200.6252%20%5C%5C%20%0A-7%5Ctimes10%5E%7B-7%7DI%5E2%2B0.0016%5C%3BI-0.0001%2C%20%26%20%5Ctextrm%7Bfor%20%7D%20I%3E%200.6252%20%0A%5Cend%7Bmatrix%7D%5Cright.%0A "
\alpha=
\left\{\begin{matrix}
0,                                   & \textrm{for } I< 0.6252 \\ 
-7\times10^{-7}I^2+0.0016\;I-0.0001, & \textrm{for } I> 0.6252 
\end{matrix}\right.
")

Where ![I](https://latex.codecogs.com/png.latex?I "I") is the daily
photosynthetically active radiation for the month.

**precipitation index
![\\beta](https://latex.codecogs.com/png.latex?%5Cbeta "\beta"):**

![
\\beta=
\\left\\{\\begin{matrix}
0,                                   & \\textrm{for } P\\leq 10.219 \\\\ 
0.0279\\;P-0.2851,                                   & \\textrm{for } 10.219&lt; P&lt; 46.061 \\\\ 
1, & \\textrm{for } P\\geq 46.061 
\\end{matrix}\\right.
](https://latex.codecogs.com/png.latex?%0A%5Cbeta%3D%0A%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%0A0%2C%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%26%20%5Ctextrm%7Bfor%20%7D%20P%5Cleq%2010.219%20%5C%5C%20%0A0.0279%5C%3BP-0.2851%2C%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%26%20%5Ctextrm%7Bfor%20%7D%2010.219%3C%20P%3C%2046.061%20%5C%5C%20%0A1%2C%20%26%20%5Ctextrm%7Bfor%20%7D%20P%5Cgeq%2046.061%20%0A%5Cend%7Bmatrix%7D%5Cright.%0A "
\beta=
\left\{\begin{matrix}
0,                                   & \textrm{for } P\leq 10.219 \\ 
0.0279\;P-0.2851,                                   & \textrm{for } 10.219< P< 46.061 \\ 
1, & \textrm{for } P\geq 46.061 
\end{matrix}\right.
")

Where ![P](https://latex.codecogs.com/png.latex?P "P") is the average
total monthly precipitation.

**temperature index
![\\Gamma](https://latex.codecogs.com/png.latex?%5CGamma "\Gamma"):**

![
\\Gamma=
\\left\\{\\begin{matrix}
0,&\\textrm{for}\\; -3&lt; T &lt; 36\\\\
-2\\times10^{-7}T^5+0.13\\times10^{-4}T^3-3.878\\times10^{-3}T^2+0.1052T+0.352 ,&\\textrm{otherwise}
\\end{matrix}\\right.
](https://latex.codecogs.com/png.latex?%0A%5CGamma%3D%0A%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%0A0%2C%26%5Ctextrm%7Bfor%7D%5C%3B%20-3%3C%20T%20%3C%2036%5C%5C%0A-2%5Ctimes10%5E%7B-7%7DT%5E5%2B0.13%5Ctimes10%5E%7B-4%7DT%5E3-3.878%5Ctimes10%5E%7B-3%7DT%5E2%2B0.1052T%2B0.352%20%2C%26%5Ctextrm%7Botherwise%7D%0A%5Cend%7Bmatrix%7D%5Cright.%0A "
\Gamma=
\left\{\begin{matrix}
0,&\textrm{for}\; -3< T < 36\\
-2\times10^{-7}T^5+0.13\times10^{-4}T^3-3.878\times10^{-3}T^2+0.1052T+0.352 ,&\textrm{otherwise}
\end{matrix}\right.
")

Where T is the average daily temperature for the month.

monthly EPI:

![\\textrm{EPI}\_\\textrm{month}=\\alpha\\times\\beta\\times\\Gamma](https://latex.codecogs.com/png.latex?%5Ctextrm%7BEPI%7D_%5Ctextrm%7Bmonth%7D%3D%5Calpha%5Ctimes%5Cbeta%5Ctimes%5CGamma "\textrm{EPI}_\textrm{month}=\alpha\times\beta\times\Gamma")

**Five year total EPI** is the sum of monthly EPIs multiplied by five:

![\\textrm{EPI}\_\\textrm{5 years}=5\\times\\sum\_{0}^{12}\\textrm{EPI}\_\\textrm{month}](https://latex.codecogs.com/png.latex?%5Ctextrm%7BEPI%7D_%5Ctextrm%7B5%20years%7D%3D5%5Ctimes%5Csum_%7B0%7D%5E%7B12%7D%5Ctextrm%7BEPI%7D_%5Ctextrm%7Bmonth%7D "\textrm{EPI}_\textrm{5 years}=5\times\sum_{0}^{12}\textrm{EPI}_\textrm{month}")

**Predicted Biomass over a 5 year cropping cycle** uses the parameters
from

![\\textrm{Biomass}=1.7607\\times\\textrm{EPI}-14.774](https://latex.codecogs.com/png.latex?%5Ctextrm%7BBiomass%7D%3D1.7607%5Ctimes%5Ctextrm%7BEPI%7D-14.774 "\textrm{Biomass}=1.7607\times\textrm{EPI}-14.774")

### Simulating Rock Mulch

![\\beta\_{\\textrm{rock mulch}}=\\beta \* 1.83](https://latex.codecogs.com/png.latex?%5Cbeta_%7B%5Ctextrm%7Brock%20mulch%7D%7D%3D%5Cbeta%20%2A%201.83 "\beta_{\textrm{rock mulch}}=\beta * 1.83")

### Simulating Irrigation

Calculate monthly water demand:

Given
![\\beta = 0.0279\\times\\textrm{P}-0.2851](https://latex.codecogs.com/png.latex?%5Cbeta%20%3D%200.0279%5Ctimes%5Ctextrm%7BP%7D-0.2851 "\beta = 0.0279\times\textrm{P}-0.2851"),
find the values of ![P](https://latex.codecogs.com/png.latex?P "P")
where
![\\beta\\geq 1](https://latex.codecogs.com/png.latex?%5Cbeta%5Cgeq%201 "\beta\geq 1")?

![\\textrm{P}\_{\\beta=1}=\\frac{1+0.2851}{0.0279}\\simeq= 46.06093](https://latex.codecogs.com/png.latex?%5Ctextrm%7BP%7D_%7B%5Cbeta%3D1%7D%3D%5Cfrac%7B1%2B0.2851%7D%7B0.0279%7D%5Csimeq%3D%2046.06093 "\textrm{P}_{\beta=1}=\frac{1+0.2851}{0.0279}\simeq= 46.06093")

-   for each month if
    -   ![\\alpha &gt; 0](https://latex.codecogs.com/png.latex?%5Calpha%20%3E%200 "\alpha > 0")
        AND
    -   ![\\gamma &gt; 0](https://latex.codecogs.com/png.latex?%5Cgamma%20%3E%200 "\gamma > 0")
        AND
    -   ![\\textrm{P} &lt; 46.061\\textrm{mm}](https://latex.codecogs.com/png.latex?%5Ctextrm%7BP%7D%20%3C%2046.061%5Ctextrm%7Bmm%7D "\textrm{P} < 46.061\textrm{mm}")
-   calculate irrigation required = water demand
    ![46.01 - P](https://latex.codecogs.com/png.latex?46.01%20-%20P "46.01 - P")
-   annual irrigation demand =
    ![\\sum\_{jan}^{dec}46.01 - \\textrm{P}](https://latex.codecogs.com/png.latex?%5Csum_%7Bjan%7D%5E%7Bdec%7D46.01%20-%20%5Ctextrm%7BP%7D "\sum_{jan}^{dec}46.01 - \textrm{P}")
    calculate sum over all months

### Simulating Freezing Mortality

For any grid cell with absolute minimum annual temperature &lt; -10, set
biomass to 0.

## Licenses

-   **Code :** [BSD
    3-Clause](https://opensource.org/licenses/BSD-3-Clause)
-   **Data :** are ‘public domain’
    [CC-0](http://creativecommons.org/publicdomain/zero/1.0/):
    attribution requested in reuse