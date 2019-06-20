---
title: "regression_models"
author: "Melissa Tobin"
date: "19/06/2019"
output:   
  html_document:
        keep_md: true
---



## Loading Packages

```r
library(lmtest)
```

```
## Loading required package: zoo
```

```
## 
## Attaching package: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```r
library(tidyverse)
```

```
## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.2.0     ✔ purrr   0.3.2
## ✔ tibble  2.1.3     ✔ dplyr   0.8.1
## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
## ✔ readr   1.1.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(ggplot2)
library(haven)
library(janitor)
library(pastecs)
```

```
## 
## Attaching package: 'pastecs'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     first, last
```

```
## The following object is masked from 'package:tidyr':
## 
##     extract
```

```r
library(psych)
```

```
## 
## Attaching package: 'psych'
```

```
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

```r
library(car)
```

```
## Loading required package: carData
```

```
## 
## Attaching package: 'car'
```

```
## The following object is masked from 'package:psych':
## 
##     logit
```

```
## The following object is masked from 'package:dplyr':
## 
##     recode
```

```
## The following object is masked from 'package:purrr':
## 
##     some
```

```r
library(Hmisc)
```

```
## Loading required package: lattice
```

```
## Loading required package: survival
```

```
## Loading required package: Formula
```

```
## 
## Attaching package: 'Hmisc'
```

```
## The following object is masked from 'package:psych':
## 
##     describe
```

```
## The following objects are masked from 'package:dplyr':
## 
##     src, summarize
```

```
## The following objects are masked from 'package:base':
## 
##     format.pval, units
```

```r
library(ggm)
```

```
## Loading required package: igraph
```

```
## 
## Attaching package: 'igraph'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     as_data_frame, groups, union
```

```
## The following objects are masked from 'package:purrr':
## 
##     compose, simplify
```

```
## The following object is masked from 'package:tidyr':
## 
##     crossing
```

```
## The following object is masked from 'package:tibble':
## 
##     as_data_frame
```

```
## The following objects are masked from 'package:stats':
## 
##     decompose, spectrum
```

```
## The following object is masked from 'package:base':
## 
##     union
```

```
## 
## Attaching package: 'ggm'
```

```
## The following object is masked from 'package:igraph':
## 
##     pa
```

```
## The following object is masked from 'package:Hmisc':
## 
##     rcorr
```

```r
library(polycor)
```

```
## 
## Attaching package: 'polycor'
```

```
## The following object is masked from 'package:psych':
## 
##     polyserial
```

```r
library(tableone)
library(forcats)
library(gmodels)
library(QuantPsyc)
```

```
## Loading required package: boot
```

```
## 
## Attaching package: 'boot'
```

```
## The following object is masked from 'package:survival':
## 
##     aml
```

```
## The following object is masked from 'package:lattice':
## 
##     melanoma
```

```
## The following object is masked from 'package:car':
## 
##     logit
```

```
## The following object is masked from 'package:psych':
## 
##     logit
```

```
## Loading required package: MASS
```

```
## 
## Attaching package: 'MASS'
```

```
## The following object is masked from 'package:dplyr':
## 
##     select
```

```
## 
## Attaching package: 'QuantPsyc'
```

```
## The following object is masked from 'package:base':
## 
##     norm
```

```r
library(KernSmooth)
```

```
## KernSmooth 2.23 loaded
## Copyright M. P. Wand 1997-2009
```

```r
library(raster)
```

```
## Loading required package: sp
```

```
## 
## Attaching package: 'raster'
```

```
## The following objects are masked from 'package:MASS':
## 
##     area, select
```

```
## The following objects are masked from 'package:Hmisc':
## 
##     mask, zoom
```

```
## The following object is masked from 'package:pastecs':
## 
##     extract
```

```
## The following object is masked from 'package:janitor':
## 
##     crosstab
```

```
## The following object is masked from 'package:dplyr':
## 
##     select
```

```
## The following object is masked from 'package:tidyr':
## 
##     extract
```

```r
library(sp)
library(sf)
```

```
## Linking to GEOS 3.6.1, GDAL 2.1.3, PROJ 4.9.3
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:igraph':
## 
##     %--%
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(anytime)
library(stringr)
library(stringi)
library(finalfit)
library(data.table)
```

```
## 
## Attaching package: 'data.table'
```

```
## The following objects are masked from 'package:lubridate':
## 
##     hour, isoweek, mday, minute, month, quarter, second, wday,
##     week, yday, year
```

```
## The following object is masked from 'package:raster':
## 
##     shift
```

```
## The following objects are masked from 'package:pastecs':
## 
##     first, last
```

```
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
```

```
## The following object is masked from 'package:purrr':
## 
##     transpose
```

```r
install.packages("devtools")
```

```
## Installing package into '/Users/shkr_dfuller_stu1/Library/R/3.5/library'
## (as 'lib' is unspecified)
```

```
## 
## The downloaded binary packages are in
## 	/var/folders/v7/t3tlmvn1459971hn6jvtwhmdcz8g_f/T//Rtmpzs7yS3/downloaded_packages
```

```r
devtools::install_github("cardiomoon/ggiraphExtra")
```

```
## Skipping install of 'ggiraphExtra' from a github remote, the SHA1 (6a47d4b5) has not changed since last install.
##   Use `force = TRUE` to force installation
```

```r
require(ggiraph)
```

```
## Loading required package: ggiraph
```

```r
require(ggiraphExtra)
```

```
## Loading required package: ggiraphExtra
```

```r
require(plyr)
```

```
## Loading required package: plyr
```

```
## -------------------------------------------------------------------------
```

```
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
```

```
## -------------------------------------------------------------------------
```

```
## 
## Attaching package: 'plyr'
```

```
## The following object is masked from 'package:lubridate':
## 
##     here
```

```
## The following objects are masked from 'package:Hmisc':
## 
##     is.discrete, summarize
```

```
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```

```
## The following object is masked from 'package:purrr':
## 
##     compact
```

## Reading in data

```r
victoria_small_merged_1 <- fread("victoria_small_merged_1.csv")

power_victoria_merged <- fread("power_victoria_merged.csv") #big file with all summary counts
table_of_power_merged_2 <- fread("table_of_power_merged_2.csv")
summary_power_victoria_merged <- fread("summary_power_victoria_merged.csv") #summary table with summarized PA
```

## Variables

```r
power_victoria_merged$income_2 <- factor(power_victoria_merged$income_2, c("$49,000 or less", "$50,000 to $99,999", "$100,000 to $149,999", "$150,000 or more", "I don't know/Prefer not to answer")) 
```

## Models

```r
regression_1 <- lm(total_pa_met_formula ~ percent, data = victoria_small_merged_1)

summary(regression_1)
```

```
## 
## Call:
## lm(formula = total_pa_met_formula ~ percent, data = victoria_small_merged_1)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
##  -4547  -2833  -1356   1073  17400 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  4955.13     417.13  11.879   <2e-16 ***
## percent       -27.45      39.19  -0.701    0.485    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4494 on 147 degrees of freedom
## Multiple R-squared:  0.003328,	Adjusted R-squared:  -0.003452 
## F-statistic: 0.4908 on 1 and 147 DF,  p-value: 0.4847
```

```r
lm.beta(regression_1) ##This isn't working. 
```

```
##     percent 
## -0.05768553
```

```r
confint(regression_1)
```

```
##                 2.5 %     97.5 %
## (Intercept) 4130.7878 5779.46818
## percent     -104.8936   49.98855
```

```r
#plot(regression_1

ggPredict(regression_1, se = TRUE, interactive = TRUE)
```

<!--html_preserve--><div id="htmlwidget-17d0467b798053eea82c" style="width:672px;height:480px;" class="girafe html-widget"></div>
<script type="application/json" data-for="htmlwidget-17d0467b798053eea82c">{"x":{"html":"<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<svg xmlns=\"http://www.w3.org/2000/svg\" xmlns:xlink=\"http://www.w3.org/1999/xlink\" id=\"svg_c5f3d1bca4020190620125941\" viewBox=\"0 0 432.00 360.00\">\n  <g>\n    <defs>\n      <clipPath id=\"cl_idc5f6c85a62120190620125942_4\">\n        <rect x=\"0.00\" y=\"360.00\" width=\"0.00\" height=\"72.00\"/>\n      <\/clipPath>\n    <\/defs>\n    <rect x=\"0.00\" y=\"0.00\" width=\"432.00\" height=\"360.00\" id=\"1\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_4)\" fill=\"#FFFFFF\" fill-opacity=\"1\" stroke-width=\"0.75\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <defs>\n      <clipPath id=\"cl_idc5f6c85a62120190620125942_5\">\n        <rect x=\"0.00\" y=\"0.00\" width=\"432.00\" height=\"360.00\"/>\n      <\/clipPath>\n    <\/defs>\n    <rect x=\"0.00\" y=\"0.00\" width=\"432.00\" height=\"360.00\" id=\"2\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_5)\" fill=\"#FFFFFF\" fill-opacity=\"1\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <defs>\n      <clipPath id=\"cl_idc5f6c85a62120190620125942_6\">\n        <rect x=\"48.37\" y=\"5.48\" width=\"378.16\" height=\"322.85\"/>\n      <\/clipPath>\n    <\/defs>\n    <rect x=\"48.37\" y=\"5.48\" width=\"378.16\" height=\"322.85\" id=\"3\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#EBEBEB\" fill-opacity=\"1\" stroke=\"none\"/>\n    <polyline points=\"48.37,285.62 426.52,285.62\" id=\"4\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,218.62 426.52,218.62\" id=\"5\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,151.61 426.52,151.61\" id=\"6\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,84.60 426.52,84.60\" id=\"7\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,17.59 426.52,17.59\" id=\"8\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"118.44,328.33 118.44,5.48\" id=\"9\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"224.22,328.33 224.22,5.48\" id=\"10\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"330.00,328.33 330.00,5.48\" id=\"11\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"0.533489\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,319.13 426.52,319.13\" id=\"12\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,252.12 426.52,252.12\" id=\"13\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,185.11 426.52,185.11\" id=\"14\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,118.11 426.52,118.11\" id=\"15\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"48.37,51.10 426.52,51.10\" id=\"16\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"65.55,328.33 65.55,5.48\" id=\"17\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"171.33,328.33 171.33,5.48\" id=\"18\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"277.11,328.33 277.11,5.48\" id=\"19\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"382.89,328.33 382.89,5.48\" id=\"20\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#FFFFFF\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polygon points=\"65.55,247.13 92.00,249.63 118.44,250.81 144.89,251.03 171.33,250.79 197.78,250.32 224.22,249.73 250.67,249.09 277.11,248.41 303.55,247.70 330.00,246.98 356.44,246.24 382.89,245.50 409.33,244.74 409.33,308.53 382.89,304.09 356.44,299.67 330.00,295.26 303.55,290.85 277.11,286.47 250.67,282.10 224.22,277.78 197.78,273.52 171.33,269.37 144.89,265.44 118.44,261.99 92.00,259.49 65.55,258.31\" id=\"21\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#333333\" fill-opacity=\"0.2\" stroke=\"none\"/>\n    <circle cx=\"82.07\" cy=\"300.82\" r=\"1.47pt\" id=\"22\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"77.71\" cy=\"276.16\" r=\"1.47pt\" id=\"23\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.65\" cy=\"293.49\" r=\"1.47pt\" id=\"24\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"72.15\" cy=\"261.30\" r=\"1.47pt\" id=\"25\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.21\" cy=\"305.78\" r=\"1.47pt\" id=\"26\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"118.95\" cy=\"299.49\" r=\"1.47pt\" id=\"27\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.48\" cy=\"253.39\" r=\"1.47pt\" id=\"28\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"174.91\" cy=\"278.50\" r=\"1.47pt\" id=\"29\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"165.36\" cy=\"281.76\" r=\"1.47pt\" id=\"30\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"74.73\" cy=\"281.63\" r=\"1.47pt\" id=\"31\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.53\" cy=\"239.63\" r=\"1.47pt\" id=\"32\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.05\" cy=\"302.92\" r=\"1.47pt\" id=\"33\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.23\" cy=\"265.55\" r=\"1.47pt\" id=\"34\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"92.36\" cy=\"283.33\" r=\"1.47pt\" id=\"35\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"75.91\" cy=\"190.59\" r=\"1.47pt\" id=\"36\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"177.13\" cy=\"281.21\" r=\"1.47pt\" id=\"37\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"120.31\" cy=\"242.15\" r=\"1.47pt\" id=\"38\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"76.22\" cy=\"304.95\" r=\"1.47pt\" id=\"39\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"74.22\" cy=\"309.36\" r=\"1.47pt\" id=\"40\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.00\" cy=\"291.45\" r=\"1.47pt\" id=\"41\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"79.14\" cy=\"279.97\" r=\"1.47pt\" id=\"42\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"84.83\" cy=\"309.88\" r=\"1.47pt\" id=\"43\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"75.22\" cy=\"83.82\" r=\"1.47pt\" id=\"44\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"127.90\" cy=\"213.71\" r=\"1.47pt\" id=\"45\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.27\" cy=\"299.67\" r=\"1.47pt\" id=\"46\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"65.89\" r=\"1.47pt\" id=\"47\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"74.23\" cy=\"158.74\" r=\"1.47pt\" id=\"48\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"74.03\" cy=\"279.12\" r=\"1.47pt\" id=\"49\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"133.07\" cy=\"228.96\" r=\"1.47pt\" id=\"50\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.20\" cy=\"292.50\" r=\"1.47pt\" id=\"51\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"81.95\" cy=\"296.77\" r=\"1.47pt\" id=\"52\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"116.42\" r=\"1.47pt\" id=\"53\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"141.08\" cy=\"307.01\" r=\"1.47pt\" id=\"54\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"79.84\" cy=\"298.81\" r=\"1.47pt\" id=\"55\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"267.93\" r=\"1.47pt\" id=\"56\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.72\" cy=\"278.76\" r=\"1.47pt\" id=\"57\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"72.91\" cy=\"170.40\" r=\"1.47pt\" id=\"58\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"341.90\" cy=\"284.79\" r=\"1.47pt\" id=\"59\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.94\" cy=\"271.56\" r=\"1.47pt\" id=\"60\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"74.54\" cy=\"20.15\" r=\"1.47pt\" id=\"61\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"85.24\" cy=\"270.67\" r=\"1.47pt\" id=\"62\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"314.25\" cy=\"287.55\" r=\"1.47pt\" id=\"63\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.89\" cy=\"277.93\" r=\"1.47pt\" id=\"64\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"154.47\" cy=\"271.84\" r=\"1.47pt\" id=\"65\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"78.46\" cy=\"161.10\" r=\"1.47pt\" id=\"66\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.19\" cy=\"281.74\" r=\"1.47pt\" id=\"67\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"76.56\" cy=\"264.58\" r=\"1.47pt\" id=\"68\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"140.15\" cy=\"308.41\" r=\"1.47pt\" id=\"69\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"133.01\" cy=\"272.23\" r=\"1.47pt\" id=\"70\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"72.07\" cy=\"290.05\" r=\"1.47pt\" id=\"71\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"72.48\" cy=\"218.67\" r=\"1.47pt\" id=\"72\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"305.78\" r=\"1.47pt\" id=\"73\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"274.72\" r=\"1.47pt\" id=\"74\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"264.07\" r=\"1.47pt\" id=\"75\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.82\" cy=\"229.12\" r=\"1.47pt\" id=\"76\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.53\" cy=\"215.67\" r=\"1.47pt\" id=\"77\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.56\" cy=\"270.98\" r=\"1.47pt\" id=\"78\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.78\" cy=\"222.42\" r=\"1.47pt\" id=\"79\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"73.03\" cy=\"203.18\" r=\"1.47pt\" id=\"80\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"73.57\" cy=\"166.99\" r=\"1.47pt\" id=\"81\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.70\" cy=\"120.30\" r=\"1.47pt\" id=\"82\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.29\" cy=\"286.82\" r=\"1.47pt\" id=\"83\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.67\" cy=\"262.97\" r=\"1.47pt\" id=\"84\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"78.85\" cy=\"168.98\" r=\"1.47pt\" id=\"85\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"70.87\" cy=\"237.37\" r=\"1.47pt\" id=\"86\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.99\" cy=\"282.81\" r=\"1.47pt\" id=\"87\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.77\" cy=\"277.82\" r=\"1.47pt\" id=\"88\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"99.21\" cy=\"292.98\" r=\"1.47pt\" id=\"89\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"94.69\" cy=\"267.70\" r=\"1.47pt\" id=\"90\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.46\" cy=\"285.19\" r=\"1.47pt\" id=\"91\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"121.13\" cy=\"293.53\" r=\"1.47pt\" id=\"92\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.64\" cy=\"262.72\" r=\"1.47pt\" id=\"93\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.99\" cy=\"274.55\" r=\"1.47pt\" id=\"94\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"70.41\" cy=\"269.45\" r=\"1.47pt\" id=\"95\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.10\" cy=\"259.01\" r=\"1.47pt\" id=\"96\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"220.87\" cy=\"249.47\" r=\"1.47pt\" id=\"97\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"95.93\" cy=\"234.46\" r=\"1.47pt\" id=\"98\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"67.17\" cy=\"266.54\" r=\"1.47pt\" id=\"99\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.75\" cy=\"289.74\" r=\"1.47pt\" id=\"100\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.96\" cy=\"269.47\" r=\"1.47pt\" id=\"101\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.20\" cy=\"275.12\" r=\"1.47pt\" id=\"102\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"68.35\" cy=\"289.88\" r=\"1.47pt\" id=\"103\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.50\" cy=\"311.51\" r=\"1.47pt\" id=\"104\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"130.14\" cy=\"304.25\" r=\"1.47pt\" id=\"105\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.89\" cy=\"299.77\" r=\"1.47pt\" id=\"106\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"71.40\" cy=\"289.00\" r=\"1.47pt\" id=\"107\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"73.15\" cy=\"278.25\" r=\"1.47pt\" id=\"108\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"70.28\" cy=\"234.67\" r=\"1.47pt\" id=\"109\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"159.62\" r=\"1.47pt\" id=\"110\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"101.75\" cy=\"282.35\" r=\"1.47pt\" id=\"111\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"75.80\" cy=\"242.92\" r=\"1.47pt\" id=\"112\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"301.57\" r=\"1.47pt\" id=\"113\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.23\" cy=\"296.48\" r=\"1.47pt\" id=\"114\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.10\" cy=\"187.07\" r=\"1.47pt\" id=\"115\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.58\" cy=\"282.03\" r=\"1.47pt\" id=\"116\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.85\" cy=\"184.68\" r=\"1.47pt\" id=\"117\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.69\" cy=\"273.65\" r=\"1.47pt\" id=\"118\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"71.19\" cy=\"268.01\" r=\"1.47pt\" id=\"119\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"90.07\" cy=\"260.08\" r=\"1.47pt\" id=\"120\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"111.46\" cy=\"107.10\" r=\"1.47pt\" id=\"121\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"169.07\" cy=\"306.90\" r=\"1.47pt\" id=\"122\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"143.50\" cy=\"225.26\" r=\"1.47pt\" id=\"123\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.37\" cy=\"293.85\" r=\"1.47pt\" id=\"124\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"75.63\" cy=\"272.89\" r=\"1.47pt\" id=\"125\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"92.65\" cy=\"275.14\" r=\"1.47pt\" id=\"126\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"140.96\" cy=\"297.26\" r=\"1.47pt\" id=\"127\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"100.71\" cy=\"237.34\" r=\"1.47pt\" id=\"128\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"80.88\" cy=\"272.81\" r=\"1.47pt\" id=\"129\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"91.95\" cy=\"271.02\" r=\"1.47pt\" id=\"130\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"105.73\" cy=\"290.13\" r=\"1.47pt\" id=\"131\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"79.68\" cy=\"206.47\" r=\"1.47pt\" id=\"132\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"20.17\" r=\"1.47pt\" id=\"133\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"99.67\" cy=\"267.45\" r=\"1.47pt\" id=\"134\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"76.80\" cy=\"221.23\" r=\"1.47pt\" id=\"135\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"257.27\" r=\"1.47pt\" id=\"136\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"264.90\" r=\"1.47pt\" id=\"137\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"92.95\" cy=\"294.49\" r=\"1.47pt\" id=\"138\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"296.93\" r=\"1.47pt\" id=\"139\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.74\" cy=\"279.86\" r=\"1.47pt\" id=\"140\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"82.86\" cy=\"249.67\" r=\"1.47pt\" id=\"141\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"84.23\" cy=\"211.49\" r=\"1.47pt\" id=\"142\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"212.02\" cy=\"308.43\" r=\"1.47pt\" id=\"143\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"70.10\" cy=\"304.64\" r=\"1.47pt\" id=\"144\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"71.24\" cy=\"297.26\" r=\"1.47pt\" id=\"145\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.79\" cy=\"310.15\" r=\"1.47pt\" id=\"146\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"81.98\" cy=\"302.56\" r=\"1.47pt\" id=\"147\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.45\" cy=\"312.09\" r=\"1.47pt\" id=\"148\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"98.22\" cy=\"292.43\" r=\"1.47pt\" id=\"149\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"79.55\" cy=\"187.83\" r=\"1.47pt\" id=\"150\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"214.41\" r=\"1.47pt\" id=\"151\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"96.73\" cy=\"309.18\" r=\"1.47pt\" id=\"152\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"313.66\" r=\"1.47pt\" id=\"153\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"262.05\" cy=\"116.77\" r=\"1.47pt\" id=\"154\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"95.96\" cy=\"40.56\" r=\"1.47pt\" id=\"155\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"78.38\" cy=\"303.29\" r=\"1.47pt\" id=\"156\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"72.69\" cy=\"208.35\" r=\"1.47pt\" id=\"157\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.43\" cy=\"302.88\" r=\"1.47pt\" id=\"158\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"73.78\" cy=\"251.82\" r=\"1.47pt\" id=\"159\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"78.27\" cy=\"291.57\" r=\"1.47pt\" id=\"160\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.37\" cy=\"262.52\" r=\"1.47pt\" id=\"161\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"396.89\" cy=\"276.67\" r=\"1.47pt\" id=\"162\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"92.27\" cy=\"267.79\" r=\"1.47pt\" id=\"163\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"170.55\" cy=\"300.35\" r=\"1.47pt\" id=\"164\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"84.90\" cy=\"269.33\" r=\"1.47pt\" id=\"165\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"69.63\" cy=\"257.45\" r=\"1.47pt\" id=\"166\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.55\" cy=\"275.33\" r=\"1.47pt\" id=\"167\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"107.79\" cy=\"91.97\" r=\"1.47pt\" id=\"168\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"66.25\" cy=\"272.78\" r=\"1.47pt\" id=\"169\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <circle cx=\"65.80\" cy=\"269.11\" r=\"1.47pt\" id=\"170\" clip-path=\"url(#cl_idc5f6c85a62120190620125942_6)\" fill=\"#000000\" fill-opacity=\"1\" stroke-width=\"0.708661\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"round\"/>\n    <polyline points=\"65.55,252.72 92.00,254.56 118.44,256.40 144.89,258.24 171.33,260.08 197.78,261.92 224.22,263.76 250.67,265.60 277.11,267.44 303.55,269.28 330.00,271.12 356.44,272.96 382.89,274.79 409.33,276.63\" id=\"171\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_6)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#000000\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <defs>\n      <clipPath id=\"cl_idc5f6c85a62120190620125942_7\">\n        <rect x=\"0.00\" y=\"0.00\" width=\"432.00\" height=\"360.00\"/>\n      <\/clipPath>\n    <\/defs>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"38.43\" y=\"322.35\" id=\"172\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">0<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"23.41\" y=\"255.34\" id=\"173\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">5000<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"18.41\" y=\"188.33\" id=\"174\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">10000<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"18.41\" y=\"121.32\" id=\"175\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">15000<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"18.41\" y=\"54.32\" id=\"176\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">20000<\/text>\n    <\/g>\n    <polyline points=\"45.63,319.13 48.37,319.13\" id=\"177\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"45.63,252.12 48.37,252.12\" id=\"178\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"45.63,185.11 48.37,185.11\" id=\"179\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"45.63,118.11 48.37,118.11\" id=\"180\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"45.63,51.10 48.37,51.10\" id=\"181\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"65.55,331.07 65.55,328.33\" id=\"182\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"171.33,331.07 171.33,328.33\" id=\"183\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"277.11,331.07 277.11,328.33\" id=\"184\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <polyline points=\"382.89,331.07 382.89,328.33\" id=\"185\" clip-path=\"url(#cl_svg_c5f3d1bca4020190620125941_7)\" fill=\"none\" stroke-width=\"1.06698\" stroke=\"#333333\" stroke-opacity=\"1\" stroke-linejoin=\"round\" stroke-linecap=\"butt\"/>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"63.05\" y=\"339.70\" id=\"186\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">0<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"166.33\" y=\"339.70\" id=\"187\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">20<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"272.10\" y=\"339.70\" id=\"188\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">40<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"377.88\" y=\"339.70\" id=\"189\" font-size=\"6.60pt\" fill=\"#4D4D4D\" fill-opacity=\"1\" font-family=\"Arial\">60<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text x=\"219.10\" y=\"352.21\" id=\"190\" font-size=\"8.25pt\" font-family=\"Arial\">percent<\/text>\n    <\/g>\n    <g clip-path=\"url(#cl_idc5f6c85a62120190620125942_7)\">\n      <text transform=\"translate(13.35,220.10) rotate(-90)\" id=\"191\" font-size=\"8.25pt\" font-family=\"Arial\">total_pa_met_formula<\/text>\n    <\/g>\n  <\/g>\n<\/svg>\n","js":"function zzz(){document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('22').setAttribute('title','1<br>percent=3.12259659848714<br>total_pa_met_formula=1366');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('23').setAttribute('title','2<br>percent=2.29789379151417<br>total_pa_met_formula=3206');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('24').setAttribute('title','3<br>percent=0.0179188871707405<br>total_pa_met_formula=1913');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('25').setAttribute('title','4<br>percent=1.24744486020519<br>total_pa_met_formula=4315');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('26').setAttribute('title','5<br>percent=0.690624735032589<br>total_pa_met_formula=996');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('27').setAttribute('title','6<br>percent=10.0964966093545<br>total_pa_met_formula=1465');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('28').setAttribute('title','7<br>percent=0.364823629570901<br>total_pa_met_formula=4905');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('29').setAttribute('title','8<br>percent=20.6759836183131<br>total_pa_met_formula=3031.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('30').setAttribute('title','9<br>percent=18.8701722120047<br>total_pa_met_formula=2788');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('31').setAttribute('title','10<br>percent=1.73517685978454<br>total_pa_met_formula=2798');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('32').setAttribute('title','11<br>percent=0.751212895821378<br>total_pa_met_formula=5932');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('33').setAttribute('title','12<br>percent=0.660436235513458<br>total_pa_met_formula=1209');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('34').setAttribute('title','13<br>percent=0.316775645617725<br>total_pa_met_formula=3998');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('35').setAttribute('title','14<br>percent=5.06797930226697<br>total_pa_met_formula=2671');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('36').setAttribute('title','15<br>percent=1.95739430930984<br>total_pa_met_formula=9591');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('37').setAttribute('title','16<br>percent=21.0968048669261<br>total_pa_met_formula=2829.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('38').setAttribute('title','17<br>percent=10.3524347454439<br>total_pa_met_formula=5744');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('39').setAttribute('title','18<br>percent=2.01582927059674<br>total_pa_met_formula=1058');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('40').setAttribute('title','19<br>percent=1.63889899030713<br>total_pa_met_formula=729');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('41').setAttribute('title','20<br>percent=0.651215046604527<br>total_pa_met_formula=2065');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('42').setAttribute('title','21<br>percent=2.56931218788671<br>total_pa_met_formula=2922');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('43').setAttribute('title','22<br>percent=3.64543630892678<br>total_pa_met_formula=690');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('44').setAttribute('title','23<br>percent=1.82680249206977<br>total_pa_met_formula=17558');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('45').setAttribute('title','24<br>percent=11.7873118603833<br>total_pa_met_formula=7866');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('46').setAttribute('title','25<br>percent=2.78156632430577<br>total_pa_met_formula=1452');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('47').setAttribute('title','26<br>percent=0<br>total_pa_met_formula=18896');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('48').setAttribute('title','27<br>percent=1.64084602904224<br>total_pa_met_formula=11968');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('49').setAttribute('title','28<br>percent=1.60277236300628<br>total_pa_met_formula=2985');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('50').setAttribute('title','29<br>percent=12.7652226800989<br>total_pa_met_formula=6728');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('51').setAttribute('title','30<br>percent=2.76925138638061<br>total_pa_met_formula=1986.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('52').setAttribute('title','31<br>percent=3.09985973379762<br>total_pa_met_formula=1668');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('53').setAttribute('title','32<br>percent=0<br>total_pa_met_formula=15126');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('54').setAttribute('title','33<br>percent=14.2792940872945<br>total_pa_met_formula=904');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('55').setAttribute('title','34<br>percent=2.70089595777191<br>total_pa_met_formula=1516');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('56').setAttribute('title','35<br>percent=0<br>total_pa_met_formula=3820');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('57').setAttribute('title','36<br>percent=0.41048170582884<br>total_pa_met_formula=3012');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('58').setAttribute('title','37<br>percent=1.39173610926227<br>total_pa_met_formula=11098');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('59').setAttribute('title','38<br>percent=52.250885179565<br>total_pa_met_formula=2562');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('60').setAttribute('title','39<br>percent=0.0733555124689152<br>total_pa_met_formula=3549');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('61').setAttribute('title','40<br>percent=1.69931795095051<br>total_pa_met_formula=22309');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('62').setAttribute('title','41<br>percent=3.72282409670989<br>total_pa_met_formula=3616');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('63').setAttribute('title','42<br>percent=47.0232497026362<br>total_pa_met_formula=2356');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('64').setAttribute('title','43<br>percent=0.063638277061744<br>total_pa_met_formula=3074');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('65').setAttribute('title','44<br>percent=16.8109679571703<br>total_pa_met_formula=3528.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('66').setAttribute('title','45<br>percent=2.44037416278534<br>total_pa_met_formula=11792');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('67').setAttribute('title','46<br>percent=0.498555231824175<br>total_pa_met_formula=2790');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('68').setAttribute('title','47<br>percent=2.08190875235311<br>total_pa_met_formula=4070');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('69').setAttribute('title','48<br>percent=14.1043674900826<br>total_pa_met_formula=800');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('70').setAttribute('title','49<br>percent=12.7547691932697<br>total_pa_met_formula=3499');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('71').setAttribute('title','50<br>percent=1.23282416512748<br>total_pa_met_formula=2170');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('72').setAttribute('title','51<br>percent=1.3103913863837<br>total_pa_met_formula=7496');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('73').setAttribute('title','52<br>percent=0<br>total_pa_met_formula=996');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('74').setAttribute('title','53<br>percent=0<br>total_pa_met_formula=3313.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('75').setAttribute('title','54<br>percent=0<br>total_pa_met_formula=4108');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('76').setAttribute('title','55<br>percent=0.61724686871114<br>total_pa_met_formula=6716');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('77').setAttribute('title','56<br>percent=0.563029397110115<br>total_pa_met_formula=7720');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('78').setAttribute('title','57<br>percent=0.75766100296313<br>total_pa_met_formula=3593');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('79').setAttribute('title','58<br>percent=0.420946287253746<br>total_pa_met_formula=7216');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('80').setAttribute('title','59<br>percent=1.41435200207671<br>total_pa_met_formula=8652');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('81').setAttribute('title','60<br>percent=1.51511668007817<br>total_pa_met_formula=11352');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('82').setAttribute('title','61<br>percent=0.216890169836444<br>total_pa_met_formula=14836');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('83').setAttribute('title','62<br>percent=0.328974974403733<br>total_pa_met_formula=2410.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('84').setAttribute('title','63<br>percent=0.399947925024145<br>total_pa_met_formula=4190');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('85').setAttribute('title','64<br>percent=2.51310202238957<br>total_pa_met_formula=11204');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('86').setAttribute('title','65<br>percent=1.00467855259973<br>total_pa_met_formula=6100.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('87').setAttribute('title','66<br>percent=0.271398061442418<br>total_pa_met_formula=2710');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('88').setAttribute('title','67<br>percent=0.229677674955229<br>total_pa_met_formula=3082');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('89').setAttribute('title','68<br>percent=6.36399464740711<br>total_pa_met_formula=1951');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('90').setAttribute('title','69<br>percent=5.50830271147245<br>total_pa_met_formula=3837.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('91').setAttribute('title','70<br>percent=0.171335042652347<br>total_pa_met_formula=2532');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('92').setAttribute('title','71<br>percent=10.5082453311138<br>total_pa_met_formula=1910');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('93').setAttribute('title','72<br>percent=0.204639175257732<br>total_pa_met_formula=4209');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('94').setAttribute('title','73<br>percent=0.648853126452626<br>total_pa_met_formula=3326');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('95').setAttribute('title','74<br>percent=0.919046262976674<br>total_pa_met_formula=3707');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('96').setAttribute('title','75<br>percent=0.670219444750627<br>total_pa_met_formula=4486');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('97').setAttribute('title','76<br>percent=29.3658530307856<br>total_pa_met_formula=5198');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('98').setAttribute('title','77<br>percent=5.74360864862609<br>total_pa_met_formula=6318');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('99').setAttribute('title','78<br>percent=0.305745226808159<br>total_pa_met_formula=3924');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('100').setAttribute('title','79<br>percent=2.87312651449427<br>total_pa_met_formula=2192.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('101').setAttribute('title','80<br>percent=0.832816618717509<br>total_pa_met_formula=3705');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('102').setAttribute('title','81<br>percent=0.499819799313341<br>total_pa_met_formula=3284');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('103').setAttribute('title','82<br>percent=0.527876340130048<br>total_pa_met_formula=2182');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('104').setAttribute('title','83<br>percent=0.179619839891805<br>total_pa_met_formula=568');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('105').setAttribute('title','84<br>percent=12.211161561839<br>total_pa_met_formula=1110');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('106').setAttribute('title','85<br>percent=0.25265968926887<br>total_pa_met_formula=1444');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('107').setAttribute('title','86<br>percent=1.10587798998702<br>total_pa_met_formula=2248');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('108').setAttribute('title','87<br>percent=1.4366315574971<br>total_pa_met_formula=3050');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('109').setAttribute('title','88<br>percent=0.893547104412217<br>total_pa_met_formula=6302');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('110').setAttribute('title','89<br>percent=0<br>total_pa_met_formula=11902');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('111').setAttribute('title','90<br>percent=6.84349317698866<br>total_pa_met_formula=2744');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('112').setAttribute('title','91<br>percent=1.93802325264297<br>total_pa_met_formula=5686.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('113').setAttribute('title','92<br>percent=0<br>total_pa_met_formula=1310');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('114').setAttribute('title','93<br>percent=2.77477028619609<br>total_pa_met_formula=1690');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('115').setAttribute('title','94<br>percent=0.103145260676204<br>total_pa_met_formula=9854');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('116').setAttribute('title','95<br>percent=0.760295670538543<br>total_pa_met_formula=2768');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('117').setAttribute('title','96<br>percent=2.89145693406571<br>total_pa_met_formula=10032');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('118').setAttribute('title','97<br>percent=0.214846694718952<br>total_pa_met_formula=3393.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('119').setAttribute('title','98<br>percent=1.06617004478035<br>total_pa_met_formula=3814');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('120').setAttribute('title','99<br>percent=4.63458896772486<br>total_pa_met_formula=4406');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('121').setAttribute('title','100<br>percent=8.67950622342878<br>total_pa_met_formula=15821');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('122').setAttribute('title','101<br>percent=19.571958143089<br>total_pa_met_formula=912');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('123').setAttribute('title','102<br>percent=14.7368540627733<br>total_pa_met_formula=7004');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('124').setAttribute('title','103<br>percent=0.721414841682819<br>total_pa_met_formula=1886');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('125').setAttribute('title','104<br>percent=1.90460617521808<br>total_pa_met_formula=3450');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('126').setAttribute('title','105<br>percent=5.12350836600467<br>total_pa_met_formula=3282');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('127').setAttribute('title','106<br>percent=14.2582511829015<br>total_pa_met_formula=1632');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('128').setAttribute('title','107<br>percent=6.64630206928563<br>total_pa_met_formula=6103');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('129').setAttribute('title','108<br>percent=2.89788526897593<br>total_pa_met_formula=3456');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('130').setAttribute('title','109<br>percent=4.99059918177288<br>total_pa_met_formula=3590');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('131').setAttribute('title','110<br>percent=7.59713308292173<br>total_pa_met_formula=2164');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('132').setAttribute('title','111<br>percent=2.67096145288097<br>total_pa_met_formula=8406');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('133').setAttribute('title','112<br>percent=0<br>total_pa_met_formula=22308');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('134').setAttribute('title','113<br>percent=6.45082789514984<br>total_pa_met_formula=3856');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('135').setAttribute('title','114<br>percent=2.12599812726965<br>total_pa_met_formula=7305');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('136').setAttribute('title','115<br>percent=0<br>total_pa_met_formula=4616');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('137').setAttribute('title','116<br>percent=0<br>total_pa_met_formula=4046');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('138').setAttribute('title','117<br>percent=5.17971578350357<br>total_pa_met_formula=1838');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('139').setAttribute('title','118<br>percent=0<br>total_pa_met_formula=1656');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('140').setAttribute('title','119<br>percent=0.223560867184228<br>total_pa_met_formula=2930');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('141').setAttribute('title','120<br>percent=3.27280501610467<br>total_pa_met_formula=5183');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('142').setAttribute('title','121<br>percent=3.5317428865816<br>total_pa_met_formula=8032');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('143').setAttribute('title','122<br>percent=27.6923216868456<br>total_pa_met_formula=798');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('144').setAttribute('title','123<br>percent=0.860401673469803<br>total_pa_met_formula=1081');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('145').setAttribute('title','124<br>percent=1.07426597582038<br>total_pa_met_formula=1632');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('146').setAttribute('title','125<br>percent=0.0436905609153585<br>total_pa_met_formula=670');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('147').setAttribute('title','126<br>percent=3.10561993863661<br>total_pa_met_formula=1236.5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('148').setAttribute('title','127<br>percent=0.735953349936221<br>total_pa_met_formula=525');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('149').setAttribute('title','128<br>percent=6.17718539742639<br>total_pa_met_formula=1992');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('150').setAttribute('title','129<br>percent=2.64624906918437<br>total_pa_met_formula=9797');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('151').setAttribute('title','130<br>percent=0<br>total_pa_met_formula=7814');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('152').setAttribute('title','131<br>percent=5.89447875045405<br>total_pa_met_formula=742');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('153').setAttribute('title','132<br>percent=0<br>total_pa_met_formula=408');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('154').setAttribute('title','133<br>percent=37.1519783821751<br>total_pa_met_formula=15100');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('155').setAttribute('title','134<br>percent=5.74853339861636<br>total_pa_met_formula=20786');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('156').setAttribute('title','135<br>percent=2.42569650190828<br>total_pa_met_formula=1182');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('157').setAttribute('title','136<br>percent=1.34996793826147<br>total_pa_met_formula=8266');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('158').setAttribute('title','137<br>percent=0.165221233046667<br>total_pa_met_formula=1212');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('159').setAttribute('title','138<br>percent=1.5545305619779<br>total_pa_met_formula=5022');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('160').setAttribute('title','139<br>percent=2.40383433697923<br>total_pa_met_formula=2056');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('161').setAttribute('title','140<br>percent=0.154137935529283<br>total_pa_met_formula=4224');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('162').setAttribute('title','141<br>percent=62.647686542016<br>total_pa_met_formula=3168');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('163').setAttribute('title','142<br>percent=5.05070259608943<br>total_pa_met_formula=3831');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('164').setAttribute('title','143<br>percent=19.8523344191097<br>total_pa_met_formula=1401');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('165').setAttribute('title','144<br>percent=3.65746596235399<br>total_pa_met_formula=3716');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('166').setAttribute('title','145<br>percent=0.77020034212072<br>total_pa_met_formula=4602');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('167').setAttribute('title','146<br>percent=0<br>total_pa_met_formula=3268');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('168').setAttribute('title','147<br>percent=7.98576821852305<br>total_pa_met_formula=16950');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('169').setAttribute('title','148<br>percent=0.132549706139802<br>total_pa_met_formula=3458');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('170').setAttribute('title','149<br>percent=0.0465436816309317<br>total_pa_met_formula=3732');;document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('22').setAttribute('data-id','1');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('23').setAttribute('data-id','2');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('24').setAttribute('data-id','3');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('25').setAttribute('data-id','4');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('26').setAttribute('data-id','5');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('27').setAttribute('data-id','6');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('28').setAttribute('data-id','7');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('29').setAttribute('data-id','8');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('30').setAttribute('data-id','9');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('31').setAttribute('data-id','10');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('32').setAttribute('data-id','11');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('33').setAttribute('data-id','12');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('34').setAttribute('data-id','13');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('35').setAttribute('data-id','14');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('36').setAttribute('data-id','15');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('37').setAttribute('data-id','16');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('38').setAttribute('data-id','17');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('39').setAttribute('data-id','18');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('40').setAttribute('data-id','19');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('41').setAttribute('data-id','20');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('42').setAttribute('data-id','21');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('43').setAttribute('data-id','22');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('44').setAttribute('data-id','23');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('45').setAttribute('data-id','24');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('46').setAttribute('data-id','25');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('47').setAttribute('data-id','26');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('48').setAttribute('data-id','27');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('49').setAttribute('data-id','28');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('50').setAttribute('data-id','29');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('51').setAttribute('data-id','30');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('52').setAttribute('data-id','31');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('53').setAttribute('data-id','32');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('54').setAttribute('data-id','33');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('55').setAttribute('data-id','34');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('56').setAttribute('data-id','35');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('57').setAttribute('data-id','36');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('58').setAttribute('data-id','37');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('59').setAttribute('data-id','38');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('60').setAttribute('data-id','39');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('61').setAttribute('data-id','40');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('62').setAttribute('data-id','41');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('63').setAttribute('data-id','42');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('64').setAttribute('data-id','43');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('65').setAttribute('data-id','44');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('66').setAttribute('data-id','45');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('67').setAttribute('data-id','46');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('68').setAttribute('data-id','47');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('69').setAttribute('data-id','48');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('70').setAttribute('data-id','49');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('71').setAttribute('data-id','50');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('72').setAttribute('data-id','51');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('73').setAttribute('data-id','52');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('74').setAttribute('data-id','53');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('75').setAttribute('data-id','54');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('76').setAttribute('data-id','55');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('77').setAttribute('data-id','56');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('78').setAttribute('data-id','57');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('79').setAttribute('data-id','58');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('80').setAttribute('data-id','59');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('81').setAttribute('data-id','60');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('82').setAttribute('data-id','61');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('83').setAttribute('data-id','62');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('84').setAttribute('data-id','63');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('85').setAttribute('data-id','64');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('86').setAttribute('data-id','65');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('87').setAttribute('data-id','66');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('88').setAttribute('data-id','67');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('89').setAttribute('data-id','68');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('90').setAttribute('data-id','69');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('91').setAttribute('data-id','70');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('92').setAttribute('data-id','71');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('93').setAttribute('data-id','72');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('94').setAttribute('data-id','73');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('95').setAttribute('data-id','74');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('96').setAttribute('data-id','75');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('97').setAttribute('data-id','76');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('98').setAttribute('data-id','77');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('99').setAttribute('data-id','78');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('100').setAttribute('data-id','79');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('101').setAttribute('data-id','80');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('102').setAttribute('data-id','81');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('103').setAttribute('data-id','82');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('104').setAttribute('data-id','83');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('105').setAttribute('data-id','84');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('106').setAttribute('data-id','85');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('107').setAttribute('data-id','86');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('108').setAttribute('data-id','87');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('109').setAttribute('data-id','88');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('110').setAttribute('data-id','89');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('111').setAttribute('data-id','90');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('112').setAttribute('data-id','91');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('113').setAttribute('data-id','92');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('114').setAttribute('data-id','93');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('115').setAttribute('data-id','94');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('116').setAttribute('data-id','95');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('117').setAttribute('data-id','96');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('118').setAttribute('data-id','97');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('119').setAttribute('data-id','98');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('120').setAttribute('data-id','99');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('121').setAttribute('data-id','100');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('122').setAttribute('data-id','101');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('123').setAttribute('data-id','102');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('124').setAttribute('data-id','103');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('125').setAttribute('data-id','104');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('126').setAttribute('data-id','105');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('127').setAttribute('data-id','106');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('128').setAttribute('data-id','107');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('129').setAttribute('data-id','108');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('130').setAttribute('data-id','109');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('131').setAttribute('data-id','110');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('132').setAttribute('data-id','111');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('133').setAttribute('data-id','112');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('134').setAttribute('data-id','113');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('135').setAttribute('data-id','114');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('136').setAttribute('data-id','115');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('137').setAttribute('data-id','116');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('138').setAttribute('data-id','117');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('139').setAttribute('data-id','118');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('140').setAttribute('data-id','119');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('141').setAttribute('data-id','120');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('142').setAttribute('data-id','121');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('143').setAttribute('data-id','122');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('144').setAttribute('data-id','123');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('145').setAttribute('data-id','124');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('146').setAttribute('data-id','125');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('147').setAttribute('data-id','126');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('148').setAttribute('data-id','127');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('149').setAttribute('data-id','128');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('150').setAttribute('data-id','129');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('151').setAttribute('data-id','130');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('152').setAttribute('data-id','131');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('153').setAttribute('data-id','132');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('154').setAttribute('data-id','133');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('155').setAttribute('data-id','134');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('156').setAttribute('data-id','135');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('157').setAttribute('data-id','136');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('158').setAttribute('data-id','137');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('159').setAttribute('data-id','138');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('160').setAttribute('data-id','139');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('161').setAttribute('data-id','140');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('162').setAttribute('data-id','141');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('163').setAttribute('data-id','142');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('164').setAttribute('data-id','143');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('165').setAttribute('data-id','144');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('166').setAttribute('data-id','145');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('167').setAttribute('data-id','146');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('168').setAttribute('data-id','147');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('169').setAttribute('data-id','148');document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('170').setAttribute('data-id','149');;document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('171').setAttribute('title','y = -27.45*x +4955.13');;document.querySelectorAll('#svg_c5f3d1bca4020190620125941')[0].getElementById('171').setAttribute('data-id','1');};","uid":"svg_c5f3d1bca4020190620125941","ratio":1.2,"settings":{"tooltip":{"css":"{position:absolute;pointer-events:none;z-index:999;background-color:white;font-style:italic;padding:10px;border-radius:10px 20px 10px 20px;}","offx":10,"offy":0,"use_cursor_pos":true,"opacity":0.75,"usefill":false,"usestroke":false,"delay":{"over":200,"out":500}},"hover":{"css":"{r:4px;cursor:pointer;stroke:red;stroke-width:2px;}"},"zoom":{"min":1,"max":10},"capture":{"css":"{fill:#FF3333;stroke:black;}","type":"multiple","only_shiny":true},"toolbar":{"position":"top","saveaspng":false},"sizing":{"rescale":true,"width":0.75}}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
ggplot(victoria_small_merged_1, aes(y = total_pa_met_formula, x = percent)) + 
  geom_point() +
  geom_smooth(method = "lm")
```

![](regression_models_files/figure-html/unnamed-chunk-4-2.png)<!-- -->


## Regression Model with Survey Data

```r
victoria_small_merged_1 <- victoria_small_merged_1 %>% 
                              mutate_at(vars(income_2, age_categories, ethnicity_updated), as.factor)

explanatory = c("percent", "gender_updated", "age_categories", "ethnicity_updated", "income_2", "mean_temp_date", "total_precip_date")
dependent = "total_pa_met_formula"
victoria_small_merged_1 %>% finalfit(dependent, explanatory) -> t1
```

```
## Dependent is not a factor and will be treated as a continuous variable
```

```r
knitr::kable(t1, row.names = FALSE, align = c("l", "l", "r", "r", "r")) 
```



Dependent: total_pa_met_formula                                              Mean (sd)                Coefficient (univariable)              Coefficient (multivariable)
--------------------------------  ----------------------------------  ----------------  ---------------------------------------  ---------------------------------------
percent                           [0,62.6]                             4817.8 (4486.3)       -27.45 (-104.89 to 49.99, p=0.485)        -12.84 (-90.71 to 65.03, p=0.745)
gender_updated                    Men                                  5434.5 (4747.2)                                        -                                        -
                                  Women                                4256.4 (4186.7)   -1178.04 (-2624.55 to 268.47, p=0.110)   -1041.12 (-2489.15 to 406.90, p=0.157)
age_categories                    20-29                                5469.7 (4258.0)                                        -                                        -
                                  30-39                                3793.1 (4522.3)   -1676.60 (-4073.22 to 720.02, p=0.169)    -97.09 (-2688.26 to 2494.08, p=0.941)
                                  40-49                                3302.1 (2045.3)   -2167.63 (-4618.75 to 283.50, p=0.083)   -396.94 (-3134.47 to 2340.58, p=0.775)
                                  50-59                                6324.2 (5761.1)    854.50 (-1763.44 to 3472.43, p=0.520)    1926.94 (-930.83 to 4784.72, p=0.185)
                                  60+                                  6284.1 (4786.1)    814.38 (-1707.59 to 3336.36, p=0.524)    1675.65 (-952.88 to 4304.18, p=0.210)
ethnicity_updated                 Caucasian                            4801.4 (4453.8)                                        -                                        -
                                  Racialized Group                     4944.6 (4872.6)    143.23 (-2149.00 to 2435.46, p=0.902)    201.37 (-2081.94 to 2484.67, p=0.862)
income_2                          $100,000 to $149,999                 3453.4 (2930.8)                                        -                                        -
                                  $150,000 or more                     4299.2 (4424.3)    845.79 (-1441.10 to 3132.69, p=0.466)    702.12 (-1676.66 to 3080.89, p=0.560)
                                  $49,000 or less                      7421.6 (5408.0)    3968.13 (1738.37 to 6197.90, p=0.001)    3500.24 (1023.14 to 5977.34, p=0.006)
                                  $50,000 to $99,999                   4791.6 (4571.5)    1338.18 (-514.19 to 3190.55, p=0.155)    868.01 (-1049.09 to 2785.12, p=0.372)
                                  I don't know/Prefer not to answer    4706.8 (4537.1)   1253.39 (-1607.77 to 4114.54, p=0.388)    314.77 (-2639.94 to 3269.48, p=0.833)
mean_temp_date                    [4.03,19.2]                          4817.8 (4486.3)       107.31 (-92.43 to 307.05, p=0.290)       21.66 (-253.34 to 296.65, p=0.876)
total_precip_date                 [0,124]                              4817.8 (4486.3)         -23.50 (-51.62 to 4.62, p=0.101)         -6.61 (-44.79 to 31.56, p=0.732)

```r
lm_1 <- lm(total_pa_met_formula ~ percent + gender_updated + age_categories + ethnicity_updated + income_2 + mean_temp_date + total_precip_date, data = victoria_small_merged_1)
summary(lm_1)
```

```
## 
## Call:
## lm(formula = total_pa_met_formula ~ percent + gender_updated + 
##     age_categories + ethnicity_updated + income_2 + mean_temp_date + 
##     total_precip_date, data = victoria_small_merged_1)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6453.1 -2631.7  -942.9  1147.4 18597.3 
## 
## Coefficients:
##                                            Estimate Std. Error t value
## (Intercept)                                3641.543   2512.332   1.449
## percent                                     -12.841     39.376  -0.326
## gender_updatedWomen                       -1041.122    732.179  -1.422
## age_categories30-39                         -97.089   1310.200  -0.074
## age_categories40-49                        -396.945   1384.201  -0.287
## age_categories50-59                        1926.944   1445.006   1.334
## age_categories60+                          1675.649   1329.088   1.261
## ethnicity_updatedRacialized Group           201.368   1154.529   0.174
## income_2$150,000 or more                    702.116   1202.803   0.584
## income_2$49,000 or less                    3500.241   1252.519   2.795
## income_2$50,000 to $99,999                  868.011    969.365   0.895
## income_2I don't know/Prefer not to answer   314.766   1494.020   0.211
## mean_temp_date                               21.655    139.046   0.156
## total_precip_date                            -6.613     19.302  -0.343
##                                           Pr(>|t|)   
## (Intercept)                                0.14953   
## percent                                    0.74485   
## gender_updatedWomen                        0.15735   
## age_categories30-39                        0.94104   
## age_categories40-49                        0.77473   
## age_categories50-59                        0.18461   
## age_categories60+                          0.20957   
## ethnicity_updatedRacialized Group          0.86180   
## income_2$150,000 or more                   0.56037   
## income_2$49,000 or less                    0.00595 **
## income_2$50,000 to $99,999                 0.37214   
## income_2I don't know/Prefer not to answer  0.83345   
## mean_temp_date                             0.87647   
## total_precip_date                          0.73242   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4320 on 135 degrees of freedom
## Multiple R-squared:  0.1543,	Adjusted R-squared:  0.07289 
## F-statistic: 1.895 on 13 and 135 DF,  p-value: 0.03575
```

```r
victoria_small_merged_1$lm_fitted <- fitted(lm_1)

#ggplot(data = lm_1, aes(fitted, model$total_pa_met_formula)) + 
         #geom_point()
```

## Regression Model with summarry accelerometer data

```r
summary_power_victoria_merged <- summary_power_victoria_merged %>% 
                              mutate_at(vars(income_2, age_categories, ethnicity_updated), as.factor)
explanatory = c("percent", "gender_updated", "age_categories", "ethnicity_updated", "income_2", "mean_temp_date", "total_precip_date")
dependent = "total_pa"
summary_power_victoria_merged %>% finalfit(dependent, explanatory) -> t2
```

```
## Dependent is not a factor and will be treated as a continuous variable
```

```r
knitr::kable(t2, row.names = FALSE, align = c("l", "l", "r", "r", "r")) 
```



Dependent: total_pa                                              Mean (sd)              Coefficient (univariable)             Coefficient (multivariable)
--------------------  ----------------------------------  ----------------  -------------------------------------  --------------------------------------
percent               [0,62.6]                             3894.8 (1162.4)       -5.29 (-26.93 to 16.35, p=0.630)        -1.50 (-24.28 to 21.29, p=0.897)
gender_updated        Men                                  3790.1 (1203.2)                                      -                                       -
                      Women                                3988.5 (1124.2)    198.47 (-185.02 to 581.95, p=0.308)     219.05 (-177.13 to 615.24, p=0.276)
age_categories        20-29                                3694.4 (1212.3)                                      -                                       -
                      30-39                                3613.6 (1057.4)    -80.83 (-718.07 to 556.41, p=0.802)      43.31 (-658.93 to 745.54, p=0.903)
                      40-49                                3871.8 (1148.1)    177.42 (-482.26 to 837.10, p=0.596)    296.35 (-456.58 to 1049.29, p=0.438)
                      50-59                                4084.3 (1471.7)   389.91 (-309.53 to 1089.36, p=0.272)    613.51 (-160.11 to 1387.13, p=0.119)
                      60+                                   4260.2 (925.8)   565.81 (-102.01 to 1233.64, p=0.096)     681.34 (-28.10 to 1390.78, p=0.060)
ethnicity_updated     Caucasian                            3890.7 (1174.6)                                      -                                       -
                      Other                                3927.8 (1094.0)     37.12 (-574.28 to 648.51, p=0.905)     117.97 (-516.00 to 751.94, p=0.713)
income_2              $100,000 to $149,999                 3729.3 (1017.3)                                      -                                       -
                      $150,000 or more                      4119.2 (861.1)   389.96 (-230.85 to 1010.77, p=0.216)     188.57 (-464.14 to 841.28, p=0.569)
                      $49,000 or less                      4120.0 (1234.9)    390.73 (-206.27 to 987.72, p=0.198)     301.45 (-367.46 to 970.36, p=0.374)
                      $50,000 to $99,999                   3862.5 (1351.5)    133.21 (-371.26 to 637.68, p=0.602)      30.86 (-497.87 to 559.58, p=0.908)
                      I don't know/Prefer not to answer    3654.1 (1119.9)    -75.19 (-841.23 to 690.85, p=0.846)   -329.69 (-1127.14 to 467.76, p=0.415)
mean_temp_date        [4.03,19.2]                          3894.8 (1162.4)       39.86 (-12.55 to 92.27, p=0.135)       43.07 (-34.03 to 120.17, p=0.271)
total_precip_date     [0,124]                              3894.8 (1162.4)        -2.65 (-10.01 to 4.72, p=0.478)          3.00 (-7.57 to 13.57, p=0.576)

## Simple regression plot

```r
ggplot(summary_power_victoria_merged, aes(y = total_pa, x = percent)) + 
  geom_point() +
  geom_smooth(method = "lm")
```

```
## Warning: Removed 7 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 7 rows containing missing values (geom_point).
```

![](regression_models_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## Regression Model with summary_counts as outcome variable - from accelerometer data

```r
power_victoria_merged <- power_victoria_merged %>% 
                              mutate_at(vars(income_2, age_categories, ethnicity_updated), as.factor)
explanatory = c("percent", "doy_1", "gender_updated", "age_categories", "ethnicity_updated", "income_2", "mean_temp_date", "total_precip_date")
dependent = "summary_count"
power_victoria_merged %>% finalfit(dependent, explanatory) -> t3
```

```
## Dependent is not a factor and will be treated as a continuous variable
```

```r
knitr::kable(t3, row.names = FALSE, align = c("l", "l", "r", "r", "r")) 
```



Dependent: summary_count                                             Mean (sd)               Coefficient (univariable)             Coefficient (multivariable)
-------------------------  ----------------------------------  ---------------  --------------------------------------  --------------------------------------
percent                    [0,62.6]                             815.5 (1367.6)         -0.31 (-0.60 to -0.02, p=0.036)         -0.38 (-0.68 to -0.07, p=0.015)
doy_1                      [151,336]                            822.3 (1369.5)            0.06 (0.01 to 0.11, p=0.029)            0.56 (0.49 to 0.64, p<0.001)
gender_updated             Men                                  803.4 (1416.2)                                       -                                       -
                           Women                                826.8 (1321.0)         23.35 (18.10 to 28.60, p<0.001)         26.60 (21.19 to 32.01, p<0.001)
age_categories             20-29                                849.4 (1459.8)                                       -                                       -
                           30-39                                772.4 (1314.1)      -76.97 (-85.96 to -67.98, p<0.001)      -41.14 (-50.91 to -31.38, p<0.001)
                           40-49                                796.6 (1343.0)      -52.80 (-62.03 to -43.57, p<0.001)      -23.87 (-34.16 to -13.57, p<0.001)
                           50-59                                930.6 (1594.2)         81.25 (71.46 to 91.04, p<0.001)      119.36 (108.79 to 129.92, p<0.001)
                           60+                                  780.0 (1200.0)      -69.32 (-78.58 to -60.07, p<0.001)      -79.90 (-89.55 to -70.25, p<0.001)
ethnicity_updated          Caucasian                            821.1 (1386.1)                                       -                                       -
                           Racialized Group                     770.5 (1205.7)      -50.61 (-59.01 to -42.21, p<0.001)      -59.27 (-68.02 to -50.51, p<0.001)
income_2                   $49,000 or less                      896.9 (1378.8)                                       -                                       -
                           $50,000 to $99,999                   769.4 (1348.5)   -127.46 (-135.26 to -119.66, p<0.001)   -117.11 (-125.32 to -108.90, p<0.001)
                           $100,000 to $149,999                 789.5 (1364.6)    -107.40 (-115.64 to -99.16, p<0.001)      -70.80 (-80.01 to -61.58, p<0.001)
                           $150,000 or more                     814.2 (1316.6)      -82.67 (-91.81 to -73.52, p<0.001)      -89.13 (-99.31 to -78.96, p<0.001)
                           I don't know/Prefer not to answer    929.8 (1518.2)         32.90 (21.50 to 44.29, p<0.001)         68.17 (56.05 to 80.29, p<0.001)
mean_temp_date             [4.03,19.2]                          815.5 (1367.6)            5.48 (4.74 to 6.21, p<0.001)         15.84 (14.52 to 17.17, p<0.001)
total_precip_date          [0,124]                              815.5 (1367.6)         -0.85 (-0.95 to -0.76, p<0.001)            0.25 (0.10 to 0.39, p=0.001)

ng on making plots for regression

```r
pa_regression_1 <- lm(summary_count ~ percent,  data = power_victoria_merged)

summary(pa_regression_1)
```

```
## 
## Call:
## lm(formula = summary_count ~ percent, data = power_victoria_merged)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
##  -817.0  -814.7  -661.0   348.3 29478.5 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 817.0198     1.5130 540.000   <2e-16 ***
## percent      -0.3077     0.1469  -2.096   0.0361 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1368 on 1044811 degrees of freedom
##   (15537 observations deleted due to missingness)
## Multiple R-squared:  4.203e-06,	Adjusted R-squared:  3.246e-06 
## F-statistic: 4.391 on 1 and 1044811 DF,  p-value: 0.03612
```

```r
lm.beta(pa_regression_1) ##This isn't working. 
```

```
##      percent 
## -0.002050113
```

```r
confint(pa_regression_1)
```

```
##                   2.5 %       97.5 %
## (Intercept) 814.0543722 819.98522383
## percent      -0.5955618  -0.01991049
```

```r
ggplot(power_victoria_merged, aes(y = summary_count, x = percent)) + 
  geom_point() +
  geom_smooth(method = "lm")
```

```
## Warning: Removed 15537 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 15537 rows containing missing values (geom_point).
```

![](regression_models_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

