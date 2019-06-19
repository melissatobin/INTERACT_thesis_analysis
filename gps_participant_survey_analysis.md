---
title: "gps_participant_survey_analysis"
author: "Melissa Tobin"
date: "31/05/2019"
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

## Loading in Data

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


## Age descriptive Statistics for Victoria

```r
summary(victoria_small_merged_1$age_calculated, na.rm = TRUE)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   22.00   34.00   44.00   45.66   57.00   79.00
```

```r
sd(victoria_small_merged_1$age_calculated)
```

```
## [1] 13.7599
```

## Age descriptive Statistics for Victoria

```r
age_categories_plot <- ggplot (data = victoria_small_merged_1, aes(age_categories)) + 
    geom_bar() + 
        labs(title = "Age ", 
           x = "Age Categories", 
            y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(age_categories_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## Gender

```r
tabyl(victoria_small_merged_1$gender)
```

```
##  victoria_small_merged_1$gender  n   percent
##                             Men 71 0.4765101
##                           Women 78 0.5234899
```

```r
gender_plot <- ggplot(data = victoria_small_merged_1, aes(gender)) +
  geom_bar(aes(fill = gender)) +
  labs(title = "Gender",
       x = "Gender Type",
       y = "Number of Participants (n)")
plot(gender_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


## Health Status

```r
victoria_small_merged_1$health_status <- factor(victoria_small_merged_1$health_status, c("Poor", "Fair", "Good", "Very Good", "Excellent"))
tabyl(victoria_small_merged_1$health_status)
```

```
##  victoria_small_merged_1$health_status  n     percent
##                                   Poor  1 0.006711409
##                                   Fair  5 0.033557047
##                                   Good 29 0.194630872
##                              Very Good 71 0.476510067
##                              Excellent 43 0.288590604
```

```r
health_plot <- ggplot(data = victoria_small_merged_1, aes(health_status)) +
  geom_bar() +
  labs(title = "Health Status",
       x = "Health Status",
       y = "Number of Participants (n)") 
plot(health_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Marital Status

```r
tabyl(victoria_small_merged_1$marital)
```

```
##  victoria_small_merged_1$marital   n    percent
##          Married (or common law) 113 0.75838926
##            Separated or divorced  12 0.08053691
##           Single (never married)  22 0.14765101
##                          Widowed   2 0.01342282
```

```r
marital_plot <- ggplot(data = victoria_small_merged_1, aes(marital)) +
  geom_bar() +
  labs(title = "Marital Status",
       x = "Marital Status",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(marital_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## Ethnicity 

```r
tabyl(victoria_small_merged_1$ethnicity)
```

```
##  victoria_small_merged_1$ethnicity   n     percent
##                         Aboriginal   2 0.013422819
##                              Asian  11 0.073825503
##                          Caucasian 132 0.885906040
##                     Latin American   1 0.006711409
##                            Unknown   3 0.020134228
```

```r
ethnicity_plot <- ggplot(data = victoria_small_merged_1, aes(ethnicity)) +
  geom_bar() +
  labs(title = "Ethnicity",
       x = "Ethnic Group",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(ethnicity_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Income

```r
victoria_small_merged_1$income_1 <- factor(victoria_small_merged_1$income_1, c("No income", "$1 to $9,999", "$10,000 to $14,999", "$15,000 to $19,999", "$20,000 to $29,999", "$30,000 to $39,999", "$40,000 to $49,999","$50,000 to $99,999","$100,000 to $149,999","$150,000 to $199,999", "$200,000 or more", "I don't know/Prefer not to answer"))

tabyl(victoria_small_merged_1$income_1)
```

```
##   victoria_small_merged_1$income_1  n    percent
##                          No income  0 0.00000000
##                       $1 to $9,999  2 0.01342282
##                 $10,000 to $14,999  0 0.00000000
##                 $15,000 to $19,999  3 0.02013423
##                 $20,000 to $29,999  5 0.03355705
##                 $30,000 to $39,999  6 0.04026846
##                 $40,000 to $49,999  9 0.06040268
##                 $50,000 to $99,999 52 0.34899329
##               $100,000 to $149,999 37 0.24832215
##               $150,000 to $199,999 20 0.13422819
##                   $200,000 or more  3 0.02013423
##  I don't know/Prefer not to answer 12 0.08053691
```

```r
income_plot <- ggplot(data = victoria_small_merged_1, aes(income_1)) +
  geom_bar() +
  labs(title = "Income",
       x = "Income Level",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(income_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
tabyl(victoria_small_merged_1$income_2)
```

```
##   victoria_small_merged_1$income_2  n    percent
##               $100,000 to $149,999 37 0.24832215
##                   $150,000 or more 23 0.15436242
##                    $49,000 or less 25 0.16778523
##                 $50,000 to $99,999 52 0.34899329
##  I don't know/Prefer not to answer 12 0.08053691
```

## Children 

```r
tabyl(victoria_small_merged_1$children_1)
```

```
##  victoria_small_merged_1$children_1  n   percent
##                                  No 70 0.4697987
##                                 Yes 79 0.5302013
```

## Born in Canada

```r
tabyl(victoria_small_merged_1$born_canada)
```

```
##  victoria_small_merged_1$born_canada   n  percent
##                                   No  44 0.295302
##                                  Yes 105 0.704698
```

## Car Access

```r
tabyl(victoria_small_merged_1$car_access)
```

```
##  victoria_small_merged_1$car_access   n    percent
##                                  -7   2 0.01342282
##                                   1 136 0.91275168
##                                   2  11 0.07382550
```

```r
victoria_small_merged_1 <- victoria_small_merged_1 %>% mutate(car_access_1 = case_when(
  car_access == 1 ~ "Yes",
  car_access == 2 ~ "No"
))
tabyl(victoria_small_merged_1$car_access_1)
```

```
##  victoria_small_merged_1$car_access_1   n    percent valid_percent
##                                    No  11 0.07382550    0.07482993
##                                   Yes 136 0.91275168    0.92517007
##                                  <NA>   2 0.01342282            NA
```

## Support for the AAA

```r
tabyl(victoria_small_merged_1$aaa_familiarity_1)
```

```
##  victoria_small_merged_1$aaa_familiarity_1   n   percent
##                                         No  43 0.2885906
##                                        Yes 106 0.7114094
```

```r
victoria_small_merged_1$aaa_idea_1 <- factor(victoria_small_merged_1$aaa_idea_1, c("I don't know", "Very bad idea", "Somewhat bad idea", "Somewhat good idea", "Very good idea"))
tabyl(victoria_small_merged_1$aaa_idea_1)
```

```
##  victoria_small_merged_1$aaa_idea_1   n     percent
##                        I don't know   0 0.000000000
##                       Very bad idea   1 0.006711409
##                   Somewhat bad idea   2 0.013422819
##                  Somewhat good idea  14 0.093959732
##                      Very good idea 132 0.885906040
```

```r
tabyl(victoria_small_merged_1$aaa_bike_more_1)
```

```
##  victoria_small_merged_1$aaa_bike_more_1   n   percent
##                                       No  35 0.2348993
##                                      Yes 114 0.7651007
```

##Preference for Separated Bike Lane

```r
victoria_small_merged_1$major_street_separated_bike_lane <- factor(victoria_small_merged_1$major_street_separated_bike_lane, c( "Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable", "I don't know/Prefer not to answer"))
tabyl(victoria_small_merged_1$major_street_separated_bike_lane)
```

```
##  victoria_small_merged_1$major_street_separated_bike_lane   n    percent
##                                        Very uncomfortable  12 0.08053691
##                                    Somewhat uncomfortable   6 0.04026846
##                                      Somewhat comfortable  29 0.19463087
##                                          Very comfortable 100 0.67114094
##                         I don't know/Prefer not to answer   2 0.01342282
```

## Percieved Cycling Safety in Victoria

```r
victoria_small_merged_1 <- victoria_small_merged_1 %>% mutate(bike_safety_1 = case_when(
  bike_safety == 1 ~ "Very safe",
  bike_safety == 2 ~ "Somewhat safe",
  bike_safety == 3 ~ "Neither safe nor unsafe",
  bike_safety == 4 ~ "Somewhat dangerous",
  bike_safety == 5 ~ "Very dangerous",
))
tabyl(victoria_small_merged_1$bike_safety_1)
```

```
##  victoria_small_merged_1$bike_safety_1  n     percent
##                Neither safe nor unsafe 20 0.134228188
##                     Somewhat dangerous 26 0.174496644
##                          Somewhat safe 89 0.597315436
##                         Very dangerous  1 0.006711409
##                              Very safe 13 0.087248322
```

```r
victoria_small_merged_1$bike_safety_1 <- factor(victoria_small_merged_1$bike_safety_1, c( "Very safe", "Somewhat safe", "Neither safe nor unsafe", "Somewhat dangerous", "Very dangerous"))
tabyl(victoria_small_merged_1$bike_safety_1)
```

```
##  victoria_small_merged_1$bike_safety_1  n     percent
##                              Very safe 13 0.087248322
##                          Somewhat safe 89 0.597315436
##                Neither safe nor unsafe 20 0.134228188
##                     Somewhat dangerous 26 0.174496644
##                         Very dangerous  1 0.006711409
```

## Bike frequency by season

```r
summary(victoria_small_merged_1$bike_freq_a)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00   52.00   65.00   63.89   78.00   91.00
```

```r
sd(victoria_small_merged_1$bike_freq_a)
```

```
## [1] 22.87514
```

```r
summary(victoria_small_merged_1$bike_freq_b)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    0.00   39.00   65.00   53.51   65.00   91.00       1
```

```r
sd(victoria_small_merged_1$bike_freq_b, na.rm = TRUE)
```

```
## [1] 26.19329
```

```r
summary(victoria_small_merged_1$bike_freq_c)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0    52.0    66.0    66.1    78.0    91.0
```

```r
sd(victoria_small_merged_1$bike_freq_c)
```

```
## [1] 22.12579
```

```r
summary(victoria_small_merged_1$bike_freq_d)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   12.00   65.00   78.00   70.53   91.00   91.00       1
```

```r
sd(victoria_small_merged_1$bike_freq_d, na.rm = TRUE)
```

```
## [1] 19.73272
```

## Cycing infrastructure preference

```r
tabyl(victoria_small_merged_1$path_comf)
```

```
##  victoria_small_merged_1$path_comf   n    percent
##               Somewhat comfortable   9 0.06040268
##                   Very comfortable 131 0.87919463
##                 Very uncomfortable   9 0.06040268
```

```r
tabyl(victoria_small_merged_1$residential_street_comf)
```

```
##  victoria_small_merged_1$residential_street_comf  n    percent
##                             Somewhat comfortable 48 0.32214765
##                           Somewhat uncomfortable  8 0.05369128
##                                 Very comfortable 82 0.55033557
##                               Very uncomfortable 11 0.07382550
```

```r
tabyl(victoria_small_merged_1$res_street_traffic_calming_comf)
```

```
##  victoria_small_merged_1$res_street_traffic_calming_comf   n     percent
##                        I don't know/Prefer not to answer   1 0.006711409
##                                     Somewhat comfortable  13 0.087248322
##                                   Somewhat uncomfortable   2 0.013422819
##                                         Very comfortable 122 0.818791946
##                                       Very uncomfortable  11 0.073825503
```

```r
tabyl(victoria_small_merged_1$major_street_no_bike_lane)
```

```
##  victoria_small_merged_1$major_street_no_bike_lane  n    percent
##                               Somewhat comfortable 23 0.15436242
##                             Somewhat uncomfortable 54 0.36241611
##                                   Very comfortable  5 0.03355705
##                                 Very uncomfortable 67 0.44966443
```

```r
tabyl(victoria_small_merged_1$major_street_bike_lane)
```

```
##  victoria_small_merged_1$major_street_bike_lane  n   percent
##                            Somewhat comfortable 70 0.4697987
##                          Somewhat uncomfortable 43 0.2885906
##                                Very comfortable 20 0.1342282
##                              Very uncomfortable 16 0.1073826
```

