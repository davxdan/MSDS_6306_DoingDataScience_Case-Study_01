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
## [1] RCurl_1.95-4.8 bitops_1.0-6   repmis_0.5     readr_1.1.1   
## [5] ggplot2_2.2.1 
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.14        knitr_1.17          magrittr_1.5       
##  [4] hms_0.4.0           munsell_0.4.3       R.cache_0.12.0     
##  [7] colorspace_1.3-2    R6_2.2.2            rlang_0.1.6        
## [10] httr_1.3.1          stringr_1.2.0       plyr_1.8.4         
## [13] tools_3.4.3         grid_3.4.3          data.table_1.10.4-3
## [16] gtable_0.2.0        R.oo_1.21.0         htmltools_0.3.6    
## [19] yaml_2.1.16         lazyeval_0.2.1      rprojroot_1.3-1    
## [22] digest_0.6.13       tibble_1.4.2        formatR_1.5        
## [25] R.utils_2.6.0       evaluate_0.10.1     rmarkdown_1.8      
## [28] stringi_1.1.6       compiler_3.4.3      pillar_1.1.0       
## [31] R.methodsS3_1.7.1   scales_0.5.0        backports_1.1.2    
## [34] pkgconfig_2.0.1
```
## Brewery Data Analysis

[Link to the Github Repository Associated with this Study](https://github.com/davxdan/MSDS_6306_DoingDataScience_Case-Study_01)

The purpose of this is to present findings from blah blah ....
The questions asked are listed below with data analysis methods and answers.  

### The Data Provided
Load Beers.csv

```r
UrlAddress <- "https://raw.githubusercontent.com/davxdan/MSDS_6306_DoingDataScience_Case-Study_01/master/Input/RawDataFiles/Beers.csv"
DataURL <- getURL(UrlAddress)
RawBeerData <- read.table(textConnection(DataURL), fill = TRUE, sep = ",", quote = "", 
    header = TRUE)
# I was getting errors due to null values in some columns. So I added fill =
# TRUE which populates the null with NA.
```
Load Breweries.csv

```r
UrlAddress <- "https://raw.githubusercontent.com/davxdan/MSDS_6306_DoingDataScience_Case-Study_01/master/Input/RawDataFiles/Breweries.csv"
DataURL <- getURL(UrlAddress)
RawBreweryData <- read.table(textConnection(DataURL), fill = TRUE, sep = ",", 
    quote = "", header = TRUE)
# again here I was getting errors due to null values. I added fill = TRUE
# which populates the null with NA.
```
###1. How many breweries are present in each state?
The record (110,"Woodstock Inn, Station & Brewery",North Woodstock, NH) was causing errors so I had to remove, and manually update it.

Identify the record:

```r
RawBreweryData[c(110, 111, 112), ]  #Identified erroneous record
```

```
##     Brew_ID                     Name                City           State
## 110     110           "Woodstock Inn  Station & Brewery" North Woodstock
## 111      NH                                                             
## 112     111 Renegade Brewing Company              Denver              CO
```
Create corrected records:

```r
FixedRecords <- rbind(c("110", "Woodstock Inn Station & Brewery", "North Woodstock", 
    "NH"), c("111", "Renegade Brewing Company", "Denver", "CO"))  #create records
colnames(FixedRecords) <- colnames(RawBreweryData)  #add names to the 'FixedRecords' matrix
```
Create a dataframe and call it Stage1BreweryData. I am assuming I will encounter additional issues the will require some tweaking. I want to avoid tampering with the original RawBreweryData dataframe loaded from the file.

```r
Stage1BreweryData <- data.frame(rbind(RawBreweryData[-c(110, 111, 112), ], FixedRecords))
```


```r
CountBreweriesByState <- data.frame(Stage1BreweryData$State)
summary(CountBreweriesByState, maxsum = 100)
```

```
##     Stage1BreweryData.State
##                 : 0        
##   AK            : 7        
##   AL            : 3        
##   AR            : 2        
##   AZ            :11        
##   CA            :39        
##   CO            :46        
##   CT            : 8        
##   DC            : 1        
##   DE            : 2        
##   FL            :15        
##   GA            : 7        
##   HI            : 4        
##   IA            : 5        
##   ID            : 5        
##   IL            :18        
##   IN            :22        
##   KS            : 3        
##   KY            : 4        
##   LA            : 5        
##   MA            :23        
##   MD            : 7        
##   ME            : 9        
##   MI            :32        
##   MN            :12        
##   MO            : 9        
##   MS            : 2        
##   MT            : 9        
##   NC            :19        
##   ND            : 1        
##   NE            : 5        
##   NH            : 2        
##   NJ            : 3        
##   NM            : 4        
##   NV            : 2        
##   NY            :16        
##   OH            :15        
##   OK            : 6        
##   OR            :29        
##   PA            :25        
##   RI            : 5        
##   SC            : 4        
##   SD            : 1        
##   TN            : 3        
##   TX            :28        
##   UT            : 4        
##   VA            :16        
##   VT            :10        
##   WA            :23        
##   WI            :20        
##   WV            : 1        
##   WY            : 4        
##  North Woodstock: 0        
##  CO             : 1        
##  NH             : 1
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