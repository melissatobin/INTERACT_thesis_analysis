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
install.packages("lmtest")
```

```
## Installing package into '/Users/shkr_dfuller_stu1/Library/R/3.5/library'
## (as 'lib' is unspecified)
```

```
## 
## The downloaded binary packages are in
## 	/var/folders/v7/t3tlmvn1459971hn6jvtwhmdcz8g_f/T//RtmpY2waR4/downloaded_packages
```

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
victoria_small_merged <- read_csv("victoria_small_merged.csv")
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
##   # ... with 4 more columns
## )
```

```
## See spec(...) for full column specifications.
```


## Age descriptive Statistics for Victoria

```r
summary(victoria_small_merged$age_calculated, na.rm = TRUE)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    22.0    34.0    44.0    45.6    57.0    79.0
```

```r
sd(victoria_small_merged$age_calculated)
```

```
## [1] 13.73714
```

## Age descriptive Statistics for Victoria

```r
age_categories_plot <- ggplot (data = victoria_small_merged, aes(age_categories)) + 
    geom_bar() + 
        labs(title = "Age ", 
           x = "Age Categories", 
            y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(age_categories_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## Gender

```r
tabyl(victoria_small_merged$gender)
```

```
##  victoria_small_merged$gender  n    percent
##                           Men 71 0.47019868
##    Trans or gender non-binary  2 0.01324503
##                         Women 78 0.51655629
```

```r
gender_plot <- ggplot(data = victoria_small_merged, aes(gender)) +
  geom_bar(aes(fill = gender)) +
  labs(title = "Gender",
       x = "Gender Type",
       y = "Number of Participants (n)")
plot(gender_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


## Health Status

```r
victoria_small_merged$health_status <- factor(victoria_small_merged$health_status, c("Poor", "Fair", "Good", "Very Good", "Excellent"))
tabyl(victoria_small_merged$health_status)
```

```
##  victoria_small_merged$health_status  n     percent
##                                 Poor  1 0.006622517
##                                 Fair  5 0.033112583
##                                 Good 30 0.198675497
##                            Very Good 72 0.476821192
##                            Excellent 43 0.284768212
```

```r
health_plot <- ggplot(data = victoria_small_merged, aes(health_status)) +
  geom_bar() +
  labs(title = "Health Status",
       x = "Health Status",
       y = "Number of Participants (n)") 
plot(health_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Marital Status

```r
tabyl(victoria_small_merged$marital)
```

```
##  victoria_small_merged$marital   n    percent
##        Married (or common law) 113 0.74834437
##          Separated or divorced  12 0.07947020
##         Single (never married)  24 0.15894040
##                        Widowed   2 0.01324503
```

```r
marital_plot <- ggplot(data = victoria_small_merged, aes(marital)) +
  geom_bar() +
  labs(title = "Marital Status",
       x = "Marital Status",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(marital_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
## Ethnicity 

```r
tabyl(victoria_small_merged$ethnicity)
```

```
##  victoria_small_merged$ethnicity   n     percent
##                       Aboriginal   2 0.013245033
##                            Asian  11 0.072847682
##                        Caucasian 134 0.887417219
##                   Latin American   1 0.006622517
##                          Unknown   3 0.019867550
```

```r
ethnicity_plot <- ggplot(data = victoria_small_merged, aes(ethnicity)) +
  geom_bar() +
  labs(title = "Ethnicity",
       x = "Ethnic Group",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(ethnicity_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Income

```r
victoria_small_merged$income_1 <- factor(victoria_small_merged$income_1, c("No income", "$1 to $9,999", "$10,000 to $14,999", "$15,000 to $19,999", "$20,000 to $29,999", "$30,000 to $39,999", "$40,000 to $49,999","$50,000 to $99,999","$100,000 to $149,999","$150,000 to $199,999", "$200,000 or more", "I don't know/Prefer not to answer"))

tabyl(victoria_small_merged$income_1)
```

```
##     victoria_small_merged$income_1  n    percent
##                          No income  0 0.00000000
##                       $1 to $9,999  2 0.01324503
##                 $10,000 to $14,999  0 0.00000000
##                 $15,000 to $19,999  3 0.01986755
##                 $20,000 to $29,999  5 0.03311258
##                 $30,000 to $39,999  6 0.03973510
##                 $40,000 to $49,999  9 0.05960265
##                 $50,000 to $99,999 53 0.35099338
##               $100,000 to $149,999 38 0.25165563
##               $150,000 to $199,999 20 0.13245033
##                   $200,000 or more  3 0.01986755
##  I don't know/Prefer not to answer 12 0.07947020
```

```r
income_plot <- ggplot(data = victoria_small_merged, aes(income_1)) +
  geom_bar() +
  labs(title = "Income",
       x = "Income Level",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(income_plot)
```

![](gps_participant_survey_analysis_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
tabyl(victoria_small_merged$income_2)
```

```
##     victoria_small_merged$income_2  n   percent
##               $100,000 to $149,999 38 0.2516556
##                   $150,000 or more 23 0.1523179
##                    $49,000 or less 25 0.1655629
##                 $50,000 to $99,999 53 0.3509934
##  I don't know/Prefer not to answer 12 0.0794702
```

## Children 

```r
tabyl(victoria_small_merged$children_1)
```

```
##  victoria_small_merged$children_1  n   percent
##                                No 72 0.4768212
##                               Yes 79 0.5231788
```

## Born in Canada

```r
tabyl(victoria_small_merged$born_canada)
```

```
##  victoria_small_merged$born_canada   n   percent
##                                 No  44 0.2913907
##                                Yes 107 0.7086093
```

## Car Access

```r
tabyl(victoria_small_merged$car_access)
```

```
##  victoria_small_merged$car_access   n    percent
##                                -7   3 0.01986755
##                                 1 137 0.90728477
##                                 2  11 0.07284768
```

```r
victoria_small_merged <- victoria_small_merged %>% mutate(car_access_1 = case_when(
  car_access == 1 ~ "Yes",
  car_access == 2 ~ "No"
))
tabyl(victoria_small_merged$car_access_1)
```

```
##  victoria_small_merged$car_access_1   n    percent valid_percent
##                                  No  11 0.07284768    0.07432432
##                                 Yes 137 0.90728477    0.92567568
##                                <NA>   3 0.01986755            NA
```

## Support for the AAA

```r
tabyl(victoria_small_merged$aaa_familiarity_1)
```

```
##  victoria_small_merged$aaa_familiarity_1   n   percent
##                                       No  43 0.2847682
##                                      Yes 108 0.7152318
```

```r
victoria_small_merged$aaa_idea_1 <- factor(victoria_small_merged$aaa_idea_1, c("I don't know", "Very bad idea", "Somewhat bad idea", "Somewhat good idea", "Very good idea"))
tabyl(victoria_small_merged$aaa_idea_1)
```

```
##  victoria_small_merged$aaa_idea_1   n     percent
##                      I don't know   0 0.000000000
##                     Very bad idea   1 0.006622517
##                 Somewhat bad idea   2 0.013245033
##                Somewhat good idea  14 0.092715232
##                    Very good idea 134 0.887417219
```

```r
tabyl(victoria_small_merged$aaa_bike_more_1)
```

```
##  victoria_small_merged$aaa_bike_more_1   n   percent
##                                     No  36 0.2384106
##                                    Yes 115 0.7615894
```

##Preference for Separated Bike Lane

```r
victoria_small_merged$major_street_separated_bike_lane <- factor(victoria_small_merged$major_street_separated_bike_lane, c( "Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable", "I don't know/Prefer not to answer"))
tabyl(victoria_small_merged$major_street_separated_bike_lane)
```

```
##  victoria_small_merged$major_street_separated_bike_lane   n    percent
##                                      Very uncomfortable  12 0.07947020
##                                  Somewhat uncomfortable   6 0.03973510
##                                    Somewhat comfortable  30 0.19867550
##                                        Very comfortable 101 0.66887417
##                       I don't know/Prefer not to answer   2 0.01324503
```

```r
tabyl(victoria_small_merged$major_street_separated_bike_lane)
```

```
##  victoria_small_merged$major_street_separated_bike_lane   n    percent
##                                      Very uncomfortable  12 0.07947020
##                                  Somewhat uncomfortable   6 0.03973510
##                                    Somewhat comfortable  30 0.19867550
##                                        Very comfortable 101 0.66887417
##                       I don't know/Prefer not to answer   2 0.01324503
```

## Percieved Cycling Safety in Victoria

```r
victoria_small_merged <- victoria_small_merged %>% mutate(bike_safety_1 = case_when(
  bike_safety == 1 ~ "Very safe",
  bike_safety == 2 ~ "Somewhat safe",
  bike_safety == 3 ~ "Neither safe nor unsafe",
  bike_safety == 4 ~ "Somewhat dangerous",
  bike_safety == 5 ~ "Very dangerous",
))
tabyl(victoria_small_merged$bike_safety_1)
```

```
##  victoria_small_merged$bike_safety_1  n     percent
##              Neither safe nor unsafe 22 0.145695364
##                   Somewhat dangerous 26 0.172185430
##                        Somewhat safe 89 0.589403974
##                       Very dangerous  1 0.006622517
##                            Very safe 13 0.086092715
```

```r
victoria_small_merged$bike_safety_1 <- factor(victoria_small_merged$bike_safety_1, c( "Very safe", "Somewhat safe", "Neither safe nor unsafe", "Somewhat dangerous", "Very dangerous"))
tabyl(victoria_small_merged$bike_safety_1)
```

```
##  victoria_small_merged$bike_safety_1  n     percent
##                            Very safe 13 0.086092715
##                        Somewhat safe 89 0.589403974
##              Neither safe nor unsafe 22 0.145695364
##                   Somewhat dangerous 26 0.172185430
##                       Very dangerous  1 0.006622517
```

## Bike frequency by season

```r
summary(victoria_small_merged$bike_freq_a)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00   52.00   65.00   63.82   78.00   91.00
```

```r
sd(victoria_small_merged$bike_freq_a)
```

```
## [1] 22.74294
```

```r
summary(victoria_small_merged$bike_freq_b)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    0.00   39.00   65.00   53.15   65.00   91.00       1
```

```r
sd(victoria_small_merged$bike_freq_b, na.rm = TRUE)
```

```
## [1] 26.2521
```

```r
summary(victoria_small_merged$bike_freq_c)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0      52      65      66      78      91
```

```r
sd(victoria_small_merged$bike_freq_c)
```

```
## [1] 22.00788
```

```r
summary(victoria_small_merged$bike_freq_d)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   12.00   65.00   78.00   70.54   91.00   91.00       1
```

```r
sd(victoria_small_merged$bike_freq_d, na.rm = TRUE)
```

```
## [1] 19.61462
```

## Cycing infrastructure preference

```r
tabyl(victoria_small_merged$path_comf)
```

```
##  victoria_small_merged$path_comf   n    percent
##             Somewhat comfortable   9 0.05960265
##                 Very comfortable 133 0.88079470
##               Very uncomfortable   9 0.05960265
```

```r
tabyl(victoria_small_merged$residential_street_comf)
```

```
##  victoria_small_merged$residential_street_comf  n    percent
##                           Somewhat comfortable 48 0.31788079
##                         Somewhat uncomfortable  8 0.05298013
##                               Very comfortable 84 0.55629139
##                             Very uncomfortable 11 0.07284768
```

```r
tabyl(victoria_small_merged$res_street_traffic_calming_comf)
```

```
##  victoria_small_merged$res_street_traffic_calming_comf   n     percent
##                      I don't know/Prefer not to answer   1 0.006622517
##                                   Somewhat comfortable  13 0.086092715
##                                 Somewhat uncomfortable   2 0.013245033
##                                       Very comfortable 124 0.821192053
##                                     Very uncomfortable  11 0.072847682
```

```r
tabyl(victoria_small_merged$major_street_no_bike_lane)
```

```
##  victoria_small_merged$major_street_no_bike_lane  n    percent
##                             Somewhat comfortable 24 0.15894040
##                           Somewhat uncomfortable 54 0.35761589
##                                 Very comfortable  5 0.03311258
##                               Very uncomfortable 68 0.45033113
```

```r
tabyl(victoria_small_merged$major_street_bike_lane)
```

```
##  victoria_small_merged$major_street_bike_lane  n   percent
##                          Somewhat comfortable 71 0.4701987
##                        Somewhat uncomfortable 44 0.2913907
##                              Very comfortable 20 0.1324503
##                            Very uncomfortable 16 0.1059603
```

