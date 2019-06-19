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
## ✔ ggplot2 3.1.1     ✔ purrr   0.3.2
## ✔ tibble  2.1.1     ✔ dplyr   0.8.1
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
```

## Reading in data

```r
victoria_small_merged_1 <- read_csv("victoria_small_merged_1.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   interact_id = col_integer(),
##   gps_id.x = col_integer(),
##   age_calculated = col_integer(),
##   car_access = col_integer(),
##   transp_bikes_adults = col_integer(),
##   bike_safety = col_integer(),
##   bike_freq_a = col_integer(),
##   bike_freq_b = col_integer(),
##   bike_freq_c = col_integer(),
##   bike_freq_d = col_integer(),
##   Cycling_formula = col_integer(),
##   total_pa_met_formula = col_double(),
##   proportion_PA = col_double(),
##   total_points = col_integer(),
##   exposure_points = col_integer(),
##   amount = col_double(),
##   percent = col_double(),
##   doy_start = col_integer(),
##   doy_end = col_integer(),
##   total_hours = col_double()
##   # ... with 5 more columns
## )
```

```
## See spec(...) for full column specifications.
```

```r
power_victoria_merged <- read_csv("power_victoria_merged.csv") #big file with all summary counts
```

```
## Parsed with column specification:
## cols(
##   .default = col_integer(),
##   utc_date = col_datetime(format = ""),
##   x = col_double(),
##   y = col_double(),
##   z = col_double(),
##   lon = col_double(),
##   lat = col_double(),
##   speed = col_double(),
##   alt = col_double(),
##   northing = col_double(),
##   easting = col_double(),
##   zone = col_character(),
##   summary_count = col_double(),
##   activity_levels = col_character(),
##   gender.x = col_character(),
##   sensedoc_ID = col_character(),
##   residence_cp = col_character(),
##   age_categories = col_character(),
##   gender.y = col_character(),
##   health_status = col_character(),
##   marital = col_character()
##   # ... with 24 more columns
## )
## See spec(...) for full column specifications.
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 499877 parsing failures.
## row # A tibble: 5 x 5 col     row col       expected   actual file                        expected   <int> <chr>     <chr>      <chr>  <chr>                       actual 1  7528 ethica_ID an integer NA - 3 'power_victoria_merged.csv' file 2  7529 ethica_ID an integer NA - 3 'power_victoria_merged.csv' row 3  7530 ethica_ID an integer NA - 3 'power_victoria_merged.csv' col 4  7531 ethica_ID an integer NA - 3 'power_victoria_merged.csv' expected 5  7532 ethica_ID an integer NA - 3 'power_victoria_merged.csv'
## ... ................. ... ............................................................... ........ ............................................................... ...... ............................................................... .... ............................................................... ... ............................................................... ... ............................................................... ........ ...............................................................
## See problems(...) for more details.
```

```r
summary_power_victoria_merged <- read_csv("summary_power_victoria_merged.csv") #summary table with summarized PA
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   interact_id = col_integer(),
##   total_sedentary = col_integer(),
##   total_light = col_integer(),
##   total_moderate = col_integer(),
##   total_vigorous = col_integer(),
##   total_pa = col_integer(),
##   gps_id.x = col_integer(),
##   age_calculated = col_integer(),
##   car_access = col_integer(),
##   transp_bikes_adults = col_integer(),
##   bike_safety = col_integer(),
##   bike_freq_a = col_integer(),
##   bike_freq_b = col_integer(),
##   bike_freq_c = col_integer(),
##   bike_freq_d = col_integer(),
##   Cycling_formula = col_integer(),
##   total_pa_met_formula = col_double(),
##   proportion_PA = col_double(),
##   total_points = col_integer(),
##   exposure_points = col_integer()
##   # ... with 10 more columns
## )
## See spec(...) for full column specifications.
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
#plot(regression_1)
```


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
                                  Other                                4944.6 (4872.6)    143.23 (-2149.00 to 2435.46, p=0.902)    201.37 (-2081.94 to 2484.67, p=0.862)
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
## ethnicity_updatedOther                      201.368   1154.529   0.174
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
## ethnicity_updatedOther                     0.86180   
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



Dependent: summary_count                                             Mean (sd)            Coefficient (univariable)          Coefficient (multivariable)
-------------------------  ----------------------------------  ---------------  -----------------------------------  -----------------------------------
percent                    [0,62.6]                             815.5 (1367.6)      -0.31 (-0.60 to -0.02, p=0.036)      -0.38 (-0.68 to -0.07, p=0.015)
doy_1                      [151,336]                            822.3 (1369.5)         0.06 (0.01 to 0.11, p=0.029)         0.56 (0.49 to 0.64, p<0.001)
gender_updated             Men                                  803.4 (1416.2)                                    -                                    -
                           Women                                826.8 (1321.0)      23.35 (18.10 to 28.60, p<0.001)      26.60 (21.19 to 32.01, p<0.001)
age_categories             20-29                                849.4 (1459.8)                                    -                                    -
                           30-39                                772.4 (1314.1)   -76.97 (-85.96 to -67.98, p<0.001)   -41.14 (-50.91 to -31.38, p<0.001)
                           40-49                                796.6 (1343.0)   -52.80 (-62.03 to -43.57, p<0.001)   -23.87 (-34.16 to -13.57, p<0.001)
                           50-59                                930.6 (1594.2)      81.25 (71.46 to 91.04, p<0.001)   119.36 (108.79 to 129.92, p<0.001)
                           60+                                  780.0 (1200.0)   -69.32 (-78.58 to -60.07, p<0.001)   -79.90 (-89.55 to -70.25, p<0.001)
ethnicity_updated          Caucasian                            821.1 (1386.1)                                    -                                    -
                           Other                                770.5 (1205.7)   -50.61 (-59.01 to -42.21, p<0.001)   -59.27 (-68.02 to -50.51, p<0.001)
income_2                   $100,000 to $149,999                 789.5 (1364.6)                                    -                                    -
                           $150,000 or more                     814.2 (1316.6)      24.73 (16.37 to 33.10, p<0.001)    -18.34 (-27.20 to -9.47, p<0.001)
                           $49,000 or less                      896.9 (1378.8)    107.40 (99.16 to 115.64, p<0.001)      70.80 (61.58 to 80.01, p<0.001)
                           $50,000 to $99,999                   769.4 (1348.5)   -20.06 (-26.93 to -13.19, p<0.001)   -46.31 (-53.59 to -39.04, p<0.001)
                           I don't know/Prefer not to answer    929.8 (1518.2)   140.30 (129.52 to 151.07, p<0.001)   138.97 (127.66 to 150.28, p<0.001)
mean_temp_date             [4.03,19.2]                          815.5 (1367.6)         5.48 (4.74 to 6.21, p<0.001)      15.84 (14.52 to 17.17, p<0.001)
total_precip_date          [0,124]                              815.5 (1367.6)      -0.85 (-0.95 to -0.76, p<0.001)         0.25 (0.10 to 0.39, p=0.001)

