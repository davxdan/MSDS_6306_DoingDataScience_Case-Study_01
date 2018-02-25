---
title: "Case Study_01_BeerDataAnalysis"
authors: "Daniel Davieau, Lakeitra Webb, Emil Ramos"
date: "February 19, 2018"
output:
  html_document:
    keep_md: true
---

Load Libraries

```r
rm(list = ls())
library(ggplot2)
library(readr)
library(repmis)
library(RCurl)
```

```
## Loading required package: bitops
```

```r
library(bitops)
library(tidyverse)
```

```
## -- Attaching packages ------------------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v tibble  1.4.1     v dplyr   0.7.4
## v tidyr   0.7.2     v stringr 1.2.0
## v purrr   0.2.4     v forcats 0.2.0
```

```
## -- Conflicts ---------------------------------------------------------------------------------- tidyverse_conflicts() --
## x tidyr::complete() masks RCurl::complete()
## x dplyr::filter()   masks stats::filter()
## x dplyr::lag()      masks stats::lag()
```
## Environment Information

```r
sessionInfo()
```

```
## R version 3.4.3 (2017-11-30)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 16299)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] forcats_0.2.0   stringr_1.2.0   dplyr_0.7.4     purrr_0.2.4    
##  [5] tidyr_0.7.2     tibble_1.4.1    tidyverse_1.2.1 RCurl_1.95-4.10
##  [9] bitops_1.0-6    repmis_0.5      readr_1.1.1     ggplot2_2.2.1  
## 
## loaded via a namespace (and not attached):
##  [1] reshape2_1.4.3      haven_1.1.1         lattice_0.20-35    
##  [4] colorspace_1.3-2    htmltools_0.3.6     yaml_2.1.16        
##  [7] rlang_0.1.6         R.oo_1.21.0         pillar_1.0.1       
## [10] foreign_0.8-69      glue_1.2.0          R.utils_2.6.0      
## [13] modelr_0.1.1        readxl_1.0.0        bindrcpp_0.2       
## [16] R.cache_0.13.0      plyr_1.8.4          bindr_0.1          
## [19] munsell_0.4.3       gtable_0.2.0        cellranger_1.1.0   
## [22] rvest_0.3.2         R.methodsS3_1.7.1   psych_1.7.8        
## [25] evaluate_0.10.1     knitr_1.18          parallel_3.4.3     
## [28] broom_0.4.3         Rcpp_0.12.14        scales_0.5.0       
## [31] backports_1.1.2     formatR_1.5         jsonlite_1.5       
## [34] mnormt_1.5-5        hms_0.4.1           digest_0.6.13      
## [37] stringi_1.1.6       grid_3.4.3          rprojroot_1.3-2    
## [40] cli_1.0.0           tools_3.4.3         magrittr_1.5       
## [43] lazyeval_0.2.1      crayon_1.3.4        pkgconfig_2.0.1    
## [46] xml2_1.2.0          data.table_1.10.4-3 lubridate_1.7.2    
## [49] rstudioapi_0.7      assertthat_0.2.0    rmarkdown_1.8      
## [52] httr_1.3.1          R6_2.2.2            nlme_3.1-131       
## [55] compiler_3.4.3
```
## Brewery Data Analysis

[Link to the Github Repository Associated with this Study](https://github.com/davxdan/MSDS_6306_DoingDataScience_Case-Study_01)

The purpose of this is to present findings from blah blah ....
The questions asked are listed below with data analysis methods and answers.  

### The Data Provided
Load Beers.csv

```r
RawBeerData <- read_csv("Input/RawDataFiles/Beers.csv")
```

```
## Parsed with column specification:
## cols(
##   Name = col_character(),
##   Beer_ID = col_integer(),
##   ABV = col_double(),
##   IBU = col_integer(),
##   Brewery_id = col_integer(),
##   Style = col_character(),
##   Ounces = col_double()
## )
```
Load Breweries.csv

```r
RawBreweryData <- read_csv("Input/RawDataFiles/Breweries.csv")
```

```
## Parsed with column specification:
## cols(
##   Brew_ID = col_integer(),
##   Name = col_character(),
##   City = col_character(),
##   State = col_character()
## )
```
###1. How many breweries are present in each state?
The record (110,"Woodstock Inn, Station & Brewery",North Woodstock, NH) was causing errors but readr fixed this.
Identify the records:

```r
RawBreweryData[c(110, 111, 112), ]  #Identified erroneous records
```

```
## # A tibble: 3 x 4
##   Brew_ID Name                             City            State
##     <int> <chr>                            <chr>           <chr>
## 1     110 Woodstock Inn, Station & Brewery North Woodstock NH   
## 2     111 Renegade Brewing Company         Denver          CO   
## 3     112 Mother Earth Brew Company        Vista           CA
```

```r
Stage1BreweryData <- RawBreweryData
Stage1BreweryData <- transform(Stage1BreweryData, State = as.character(State))
```

```r
CountBreweriesByState <- data.frame(Stage1BreweryData$State)
summary(CountBreweriesByState, maxsum = 100)
```

```
##  Stage1BreweryData.State
##  AK: 7                  
##  AL: 3                  
##  AR: 2                  
##  AZ:11                  
##  CA:39                  
##  CO:47                  
##  CT: 8                  
##  DC: 1                  
##  DE: 2                  
##  FL:15                  
##  GA: 7                  
##  HI: 4                  
##  IA: 5                  
##  ID: 5                  
##  IL:18                  
##  IN:22                  
##  KS: 3                  
##  KY: 4                  
##  LA: 5                  
##  MA:23                  
##  MD: 7                  
##  ME: 9                  
##  MI:32                  
##  MN:12                  
##  MO: 9                  
##  MS: 2                  
##  MT: 9                  
##  NC:19                  
##  ND: 1                  
##  NE: 5                  
##  NH: 3                  
##  NJ: 3                  
##  NM: 4                  
##  NV: 2                  
##  NY:16                  
##  OH:15                  
##  OK: 6                  
##  OR:29                  
##  PA:25                  
##  RI: 5                  
##  SC: 4                  
##  SD: 1                  
##  TN: 3                  
##  TX:28                  
##  UT: 4                  
##  VA:16                  
##  VT:10                  
##  WA:23                  
##  WI:20                  
##  WV: 1                  
##  WY: 4
```
###2. Merge beer data with the breweries data. Print the ﬁrst 6 observations and the last six observations to check the merged ﬁle.

###3. Report the number of NA’s in each column.

###4. Compute the median alcohol content and international bitterness unit for each state. Plot a bar chart to compare.

###5. Which state has the maximum alcoholic (ABV) beer? Which state has the most bitter (IBU) beer?

###6. Summary statistics for the ABV variable.

###7. Is there an apparent relationship between the bitterness of the beer and its alcoholic content? Draw a scatter plot. You are welcome to use the ggplot2 library for graphs. Please ignore missing values in your analysis. Make your best judgment of a relationship and EXPLAIN your answer.









>Formatting Samples  

# Header 1   
## Header 2   
### THis is a header  
#### Header 4   
##### Header 5   
###### Header 6  

--  

---  

...  

$A = \pi*r^{2}$  

![](SampleImage.png)  

***  

> block quote  

* unordered list  

* item 2  

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
