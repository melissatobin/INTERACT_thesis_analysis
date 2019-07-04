---
title: "HKR 6610 - Online Survey Analysis (Gender)"
author: "Melissa Tobin"
date: '2019-05-14'
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
## ── Attaching packages ─────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.1       ✔ purrr   0.3.2  
## ✔ tibble  2.1.1       ✔ dplyr   0.8.0.1
## ✔ tidyr   0.8.3       ✔ stringr 1.4.0  
## ✔ readr   1.1.1       ✔ forcats 0.4.0
```

```
## ── Conflicts ────────────────────────────────────────────────────── tidyverse_conflicts() ──
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

## Reading in Data

```r
getwd()
```

```
## [1] "/Users/MelissaTobin/Documents/Thesis Results/INTERACT Thesis Analysis"
```

```r
#setwd("/Volumes/hkr-storage/Research/dfuller/Walkabilly/studies/INTERACT/Data/Health") #only use on lab computer

eligibility <- read_csv("eligibility_1vic_main.90838bb.csv")
```

```
## Parsed with column specification:
## cols(
##   treksoft_id = col_integer(),
##   birth_date = col_date(format = ""),
##   gender_vic = col_character(),
##   gender_vic_1 = col_integer(),
##   gender_vic_2 = col_integer(),
##   gender_vic_3 = col_integer(),
##   gender_vic_4 = col_integer(),
##   residence_vic = col_integer(),
##   moving_plans_vic = col_integer(),
##   bike_weekly_vic = col_integer(),
##   language = col_integer()
## )
```

```r
health_data <- read_csv("health_1vic_main.90838bb.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_integer(),
##   preferred_mode_f_txt = col_character(),
##   car_share = col_character(),
##   car_share_txt = col_character(),
##   house_tenure_txt = col_character(),
##   dwelling_type_txt = col_character(),
##   living_arrange = col_character(),
##   living_arrange_txt = col_character(),
##   residence = col_date(format = ""),
##   group_id = col_character(),
##   gender_vic = col_character()
## )
```

```
## See spec(...) for full column specifications.
```

```r
van_eligibility <- read_csv("eligibility_1van_main.90838bb.csv")
```

```
## Parsed with column specification:
## cols(
##   treksoft_id = col_integer(),
##   birth_date = col_date(format = ""),
##   residence_van = col_character(),
##   moving_plans_van = col_integer(),
##   leave_home = col_integer(),
##   language = col_integer(),
##   hear = col_integer(),
##   hear_txt = col_character()
## )
```

```r
van_health <- read_csv("health_1van_main.90838bb.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_integer(),
##   transp_main_mode_txt = col_character(),
##   preferred_mode_f_txt = col_character(),
##   intercept_ag_mode_txt = col_character(),
##   intercept_ag_spring_txt = col_character(),
##   intercept_ag_future_txt = col_character(),
##   intercept_ag_not = col_character(),
##   intercept_ag_not_txt = col_character(),
##   weight = col_double(),
##   tracking1_txt = col_character(),
##   gender_txt = col_character(),
##   sex_txt = col_character(),
##   living_arrange = col_character(),
##   living_arrange_txt = col_character(),
##   house_tenure_txt = col_character(),
##   dwelling_type_txt = col_character(),
##   residence = col_date(format = ""),
##   group_id = col_character(),
##   group_id_txt = col_character(),
##   employment_txt = col_character(),
##   aid_type_txt = col_character()
## )
## See spec(...) for full column specifications.
```

## Merging Data together

```r
victoria_merged <- full_join(eligibility, health_data, by = "treksoft_id")
vancouver_merged <- full_join(van_eligibility, van_health, by = "treksoft_id")
```

## Merging round two 

```r
victoria_merged_filter <- filter(victoria_merged, transp_bikes_adults >= 0)
vancouver_merged_filter <- filter(vancouver_merged, transp_main_mode >= 0)
```

## Separating Birth Date into Year/Month/Day

```r
victoria_merged_filter <- separate(victoria_merged_filter, birth_date, c("year", "month", "day"), sep = "-")
vancouver_merged_filter <- separate(vancouver_merged_filter, birth_date, c("year", "month", "day"), sep = "-")
```

## Adding new column for age

```r
victoria_merged_filter$year <- as.numeric(victoria_merged_filter$year)
victoria_merged_filter <- mutate(victoria_merged_filter, "age_calculated" = 2017 - year)
vancouver_merged_filter$year <- as.numeric(vancouver_merged_filter$year)
vancouver_merged_filter <- mutate(vancouver_merged_filter, "age_calculated" = 2018 - year)
```

## Age descriptive Statistics for Vancouver

```r
summary(vancouver_merged_filter$age_calculated, na.rm = TRUE)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   18.00   48.00   58.00   56.45   68.00   89.00
```

```r
sd(vancouver_merged_filter$age_calculated)
```

```
## [1] 15.39683
```

## Age descriptive Statistics for Victoria

```r
summary(victoria_merged_filter$age_calculated, na.rm = TRUE)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   21.00   34.00   42.00   44.23   56.00   79.00
```

```r
sd(victoria_merged_filter$age_calculated)
```

```
## [1] 13.42192
```

## Gender and Sex Vancouver

```r
tabyl(vancouver_merged_filter$sex)
```

```
##  vancouver_merged_filter$sex   n     percent valid_percent
##                            1 110 0.328358209     0.3293413
##                            2 224 0.668656716     0.6706587
##                           NA   1 0.002985075            NA
```

```r
tabyl(vancouver_merged_filter$gender)
```

```
##  vancouver_merged_filter$gender   n     percent valid_percent
##                               1 110 0.328358209   0.329341317
##                               2 223 0.665671642   0.667664671
##                               5   1 0.002985075   0.002994012
##                              NA   1 0.002985075            NA
```

## Gender Victoria

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(gender = case_when(
  gender_vic_1.x == 1 & gender_vic_4.x == 1 ~ "LGBTQ+",
  gender_vic_1.x == 1 ~ "Men",
  gender_vic_2.x == 1 ~ "Women", 
  gender_vic_3.x == 1 ~ "LGBTQ+",
  gender_vic_4.x == 1 ~ "LGBTQ+"
)) 

victoria_merged_filter$gender <- factor(victoria_merged_filter$gender, c("Women", "Men", "LGBTQ+")) 
tabyl(victoria_merged_filter$gender)
```

```
##  victoria_merged_filter$gender   n    percent
##                          Women 146 0.51957295
##                            Men 131 0.46619217
##                         LGBTQ+   4 0.01423488
```

```r
tabyl(victoria_merged_filter$gender_vic.x)
```

```
##  victoria_merged_filter$gender_vic.x   n     percent
##                               [1, 4]   1 0.003558719
##                                  [1] 131 0.466192171
##                                  [2] 146 0.519572954
##                                  [3]   1 0.003558719
##                                  [4]   2 0.007117438
```

## Age statistics grouped by gender

```r
describeBy(victoria_merged_filter$age_calculated, victoria_merged_filter$gender)
```

```
## 
##  Descriptive statistics by group 
## group: Women
##    vars   n  mean    sd median trimmed   mad min max range skew kurtosis
## X1    1 146 42.86 12.91   40.5   42.37 14.08  21  71    50 0.36    -1.04
##      se
## X1 1.07
## -------------------------------------------------------- 
## group: Men
##    vars   n  mean    sd median trimmed   mad min max range skew kurtosis
## X1    1 131 45.75 13.98     44   45.15 16.31  21  79    58 0.31    -0.81
##      se
## X1 1.22
## -------------------------------------------------------- 
## group: LGBTQ+
##    vars n mean   sd median trimmed  mad min max range  skew kurtosis   se
## X1    1 4 44.5 9.98     48    44.5 4.45  30  52    22 -0.62    -1.78 4.99
```

## Trying a new table 

```r
table_age_gender <- table(victoria_merged_filter$age_calculated, victoria_merged_filter$gender)
table_age_gender
```

```
##     
##      Women Men LGBTQ+
##   21     2   1      0
##   22     1   0      0
##   23     2   2      0
##   24     2   3      0
##   25     1   1      0
##   26     3   1      0
##   27     3   3      0
##   28     3   4      0
##   29     5   4      0
##   30     5   1      1
##   31     5   3      0
##   32     3   1      0
##   33     4   3      0
##   34    11   4      0
##   35     2   5      0
##   36     5   5      0
##   37     8   4      0
##   38     3   5      0
##   39     3   2      0
##   40     2   3      0
##   41     3   2      0
##   42     6   2      0
##   43     3   4      0
##   44     4   4      0
##   45     2   2      0
##   46     1   2      1
##   47     4   3      0
##   48     3   4      0
##   49     3   0      0
##   50     1   4      1
##   51     1   5      0
##   52     1   2      1
##   53     2   0      0
##   54     0   3      0
##   55     4   2      0
##   56     3   2      0
##   57     5   4      0
##   58     2   4      0
##   59     2   3      0
##   60     4   3      0
##   61     4   2      0
##   62     3   3      0
##   63     1   2      0
##   64     4   0      0
##   65     1   2      0
##   66     2   0      0
##   67     1   2      0
##   68     1   1      0
##   69     1   1      0
##   70     0   2      0
##   71     1   1      0
##   73     0   2      0
##   75     0   1      0
##   78     0   1      0
##   79     0   1      0
```

```r
margin.table(table_age_gender, 1)
```

```
## 
## 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 
##  3  1  4  5  2  4  6  7  9  7  8  4  7 15  7 10 12  8  5  5  5  8  7  8  4 
## 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 
##  4  7  7  3  6  6  4  2  3  6  5  9  6  5  7  6  6  3  4  3  2  3  2  2  2 
## 71 73 75 78 79 
##  2  2  1  1  1
```

```r
prop.table(table_age_gender)
```

```
##     
##            Women         Men      LGBTQ+
##   21 0.007117438 0.003558719 0.000000000
##   22 0.003558719 0.000000000 0.000000000
##   23 0.007117438 0.007117438 0.000000000
##   24 0.007117438 0.010676157 0.000000000
##   25 0.003558719 0.003558719 0.000000000
##   26 0.010676157 0.003558719 0.000000000
##   27 0.010676157 0.010676157 0.000000000
##   28 0.010676157 0.014234875 0.000000000
##   29 0.017793594 0.014234875 0.000000000
##   30 0.017793594 0.003558719 0.003558719
##   31 0.017793594 0.010676157 0.000000000
##   32 0.010676157 0.003558719 0.000000000
##   33 0.014234875 0.010676157 0.000000000
##   34 0.039145907 0.014234875 0.000000000
##   35 0.007117438 0.017793594 0.000000000
##   36 0.017793594 0.017793594 0.000000000
##   37 0.028469751 0.014234875 0.000000000
##   38 0.010676157 0.017793594 0.000000000
##   39 0.010676157 0.007117438 0.000000000
##   40 0.007117438 0.010676157 0.000000000
##   41 0.010676157 0.007117438 0.000000000
##   42 0.021352313 0.007117438 0.000000000
##   43 0.010676157 0.014234875 0.000000000
##   44 0.014234875 0.014234875 0.000000000
##   45 0.007117438 0.007117438 0.000000000
##   46 0.003558719 0.007117438 0.003558719
##   47 0.014234875 0.010676157 0.000000000
##   48 0.010676157 0.014234875 0.000000000
##   49 0.010676157 0.000000000 0.000000000
##   50 0.003558719 0.014234875 0.003558719
##   51 0.003558719 0.017793594 0.000000000
##   52 0.003558719 0.007117438 0.003558719
##   53 0.007117438 0.000000000 0.000000000
##   54 0.000000000 0.010676157 0.000000000
##   55 0.014234875 0.007117438 0.000000000
##   56 0.010676157 0.007117438 0.000000000
##   57 0.017793594 0.014234875 0.000000000
##   58 0.007117438 0.014234875 0.000000000
##   59 0.007117438 0.010676157 0.000000000
##   60 0.014234875 0.010676157 0.000000000
##   61 0.014234875 0.007117438 0.000000000
##   62 0.010676157 0.010676157 0.000000000
##   63 0.003558719 0.007117438 0.000000000
##   64 0.014234875 0.000000000 0.000000000
##   65 0.003558719 0.007117438 0.000000000
##   66 0.007117438 0.000000000 0.000000000
##   67 0.003558719 0.007117438 0.000000000
##   68 0.003558719 0.003558719 0.000000000
##   69 0.003558719 0.003558719 0.000000000
##   70 0.000000000 0.007117438 0.000000000
##   71 0.003558719 0.003558719 0.000000000
##   73 0.000000000 0.007117438 0.000000000
##   75 0.000000000 0.003558719 0.000000000
##   78 0.000000000 0.003558719 0.000000000
##   79 0.000000000 0.003558719 0.000000000
```

## Plotting Gender


```r
gender_plot <- ggplot(data = victoria_merged_filter, aes(gender)) +
  geom_bar(aes(fill = gender)) +
  labs(title = "Gender",
       x = "Gender Type",
       y = "Number of Participants (n)")
plot(gender_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


## Plotting the gender and age groupings

```r
age_grouped_plot <- ggplot (data = victoria_merged_filter, aes(age_calculated)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Age ", 
          x = "Housing Type", 
          y = "Number of Participants (n)")
plot(age_grouped_plot)
```

```
## Warning: position_dodge requires non-overlapping x intervals
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

## Age Categories (using stats can age groups)

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(age_categories = case_when(
  age_calculated < 25 ~ "20-24",
  age_calculated >= 25 & age_calculated <= 29 ~ "25-29",
  age_calculated >= 30 & age_calculated <= 34 ~ "30-34", 
  age_calculated >= 35 & age_calculated <= 39 ~ "35-39",
  age_calculated >= 40 & age_calculated <= 44 ~ "40-44",
  age_calculated >= 45 & age_calculated <= 49 ~ "45-49",
  age_calculated >= 50 & age_calculated <= 54 ~ "50-54",
  age_calculated >= 55 & age_calculated <= 59 ~ "55-59", 
  age_calculated >= 60 & age_calculated <= 64 ~ "60-64",
  age_calculated >= 65 & age_calculated <= 69 ~ "65-69",
  age_calculated >= 70 & age_calculated <= 74 ~ "70-74", 
  age_calculated >= 75 & age_calculated <= 79 ~ "75-79",
  age_calculated >= 80 & age_calculated <= 84 ~ "80-84"
))
tabyl(victoria_merged_filter$age_categories)
```

```
##  victoria_merged_filter$age_categories  n    percent
##                                  20-24 13 0.04626335
##                                  25-29 28 0.09964413
##                                  30-34 41 0.14590747
##                                  35-39 42 0.14946619
##                                  40-44 33 0.11743772
##                                  45-49 25 0.08896797
##                                  50-54 21 0.07473310
##                                  55-59 31 0.11032028
##                                  60-64 26 0.09252669
##                                  65-69 12 0.04270463
##                                  70-74  6 0.02135231
##                                  75-79  3 0.01067616
```

## Tables for gender and age categories


```r
table(victoria_merged_filter$age_categories, victoria_merged_filter$gender)
```

```
##        
##         Women Men LGBTQ+
##   20-24     7   6      0
##   25-29    15  13      0
##   30-34    28  12      1
##   35-39    21  21      0
##   40-44    18  15      0
##   45-49    13  11      1
##   50-54     5  14      2
##   55-59    16  15      0
##   60-64    16  10      0
##   65-69     6   6      0
##   70-74     1   5      0
##   75-79     0   3      0
```

```r
CrossTable(victoria_merged_filter$age_categories, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                       | victoria_merged_filter$gender 
## victoria_merged_filter$age_categories |     Women |       Men |    LGBTQ+ | Row Total | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 20-24 |         7 |         6 |         0 |        13 | 
##                                       |     0.009 |     0.001 |     0.185 |           | 
##                                       |     0.538 |     0.462 |     0.000 |     0.046 | 
##                                       |     0.048 |     0.046 |     0.000 |           | 
##                                       |     0.025 |     0.021 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 25-29 |        15 |        13 |         0 |        28 | 
##                                       |     0.014 |     0.000 |     0.399 |           | 
##                                       |     0.536 |     0.464 |     0.000 |     0.100 | 
##                                       |     0.103 |     0.099 |     0.000 |           | 
##                                       |     0.053 |     0.046 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 30-34 |        28 |        12 |         1 |        41 | 
##                                       |     2.106 |     2.648 |     0.297 |           | 
##                                       |     0.683 |     0.293 |     0.024 |     0.146 | 
##                                       |     0.192 |     0.092 |     0.250 |           | 
##                                       |     0.100 |     0.043 |     0.004 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 35-39 |        21 |        21 |         0 |        42 | 
##                                       |     0.031 |     0.103 |     0.598 |           | 
##                                       |     0.500 |     0.500 |     0.000 |     0.149 | 
##                                       |     0.144 |     0.160 |     0.000 |           | 
##                                       |     0.075 |     0.075 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 40-44 |        18 |        15 |         0 |        33 | 
##                                       |     0.043 |     0.010 |     0.470 |           | 
##                                       |     0.545 |     0.455 |     0.000 |     0.117 | 
##                                       |     0.123 |     0.115 |     0.000 |           | 
##                                       |     0.064 |     0.053 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 45-49 |        13 |        11 |         1 |        25 | 
##                                       |     0.000 |     0.037 |     1.166 |           | 
##                                       |     0.520 |     0.440 |     0.040 |     0.089 | 
##                                       |     0.089 |     0.084 |     0.250 |           | 
##                                       |     0.046 |     0.039 |     0.004 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 50-54 |         5 |        14 |         2 |        21 | 
##                                       |     3.202 |     1.810 |     9.680 |           | 
##                                       |     0.238 |     0.667 |     0.095 |     0.075 | 
##                                       |     0.034 |     0.107 |     0.500 |           | 
##                                       |     0.018 |     0.050 |     0.007 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 55-59 |        16 |        15 |         0 |        31 | 
##                                       |     0.001 |     0.021 |     0.441 |           | 
##                                       |     0.516 |     0.484 |     0.000 |     0.110 | 
##                                       |     0.110 |     0.115 |     0.000 |           | 
##                                       |     0.057 |     0.053 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 60-64 |        16 |        10 |         0 |        26 | 
##                                       |     0.459 |     0.371 |     0.370 |           | 
##                                       |     0.615 |     0.385 |     0.000 |     0.093 | 
##                                       |     0.110 |     0.076 |     0.000 |           | 
##                                       |     0.057 |     0.036 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 65-69 |         6 |         6 |         0 |        12 | 
##                                       |     0.009 |     0.029 |     0.171 |           | 
##                                       |     0.500 |     0.500 |     0.000 |     0.043 | 
##                                       |     0.041 |     0.046 |     0.000 |           | 
##                                       |     0.021 |     0.021 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 70-74 |         1 |         5 |         0 |         6 | 
##                                       |     1.438 |     1.735 |     0.085 |           | 
##                                       |     0.167 |     0.833 |     0.000 |     0.021 | 
##                                       |     0.007 |     0.038 |     0.000 |           | 
##                                       |     0.004 |     0.018 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                                 75-79 |         0 |         3 |         0 |         3 | 
##                                       |     1.559 |     1.834 |     0.043 |           | 
##                                       |     0.000 |     1.000 |     0.000 |     0.011 | 
##                                       |     0.000 |     0.023 |     0.000 |           | 
##                                       |     0.000 |     0.011 |     0.000 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
##                          Column Total |       146 |       131 |         4 |       281 | 
##                                       |     0.520 |     0.466 |     0.014 |           | 
## --------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Plotting the gender and age categories

```r
age_categories_plot <- ggplot (data = victoria_merged_filter, aes(age_categories)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Age ", 
          x = "Age Categories", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(age_categories_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


## Housing 

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(housing = case_when(
  house_tenure == 1 ~ "Owner",
  house_tenure == 2 ~ "Tenant", 
  house_tenure == 3 ~ "Resident with friends or relatives",
  house_tenure == 4 ~ "Resident not with friends or relatives", 
  house_tenure == 5 ~ "Other"
))

victoria_merged_filter$housing <- factor(victoria_merged_filter$housing, c("Other", "Resident not with friends or relatives", "Resident with friends or relatives", "Tenant", "Owner")) 
tabyl(victoria_merged_filter$housing)
```

```
##          victoria_merged_filter$housing   n     percent
##                                   Other   2 0.007117438
##  Resident not with friends or relatives   2 0.007117438
##      Resident with friends or relatives  11 0.039145907
##                                  Tenant  91 0.323843416
##                                   Owner 175 0.622775801
```

## Table for housing and gender

```r
table(victoria_merged_filter$housing, victoria_merged_filter$gender)
```

```
##                                         
##                                          Women Men LGBTQ+
##   Other                                      2   0      0
##   Resident not with friends or relatives     1   1      0
##   Resident with friends or relatives         6   4      1
##   Tenant                                    42  48      1
##   Owner                                     95  78      2
```

```r
CrossTable(victoria_merged_filter$housing, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                        | victoria_merged_filter$gender 
##         victoria_merged_filter$housing |     Women |       Men |    LGBTQ+ | Row Total | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
##                                  Other |         2 |         0 |         0 |         2 | 
##                                        |     0.888 |     0.932 |     0.028 |           | 
##                                        |     1.000 |     0.000 |     0.000 |     0.007 | 
##                                        |     0.014 |     0.000 |     0.000 |           | 
##                                        |     0.007 |     0.000 |     0.000 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
## Resident not with friends or relatives |         1 |         1 |         0 |         2 | 
##                                        |     0.001 |     0.005 |     0.028 |           | 
##                                        |     0.500 |     0.500 |     0.000 |     0.007 | 
##                                        |     0.007 |     0.008 |     0.000 |           | 
##                                        |     0.004 |     0.004 |     0.000 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
##     Resident with friends or relatives |         6 |         4 |         1 |        11 | 
##                                        |     0.014 |     0.248 |     4.543 |           | 
##                                        |     0.545 |     0.364 |     0.091 |     0.039 | 
##                                        |     0.041 |     0.031 |     0.250 |           | 
##                                        |     0.021 |     0.014 |     0.004 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
##                                 Tenant |        42 |        48 |         1 |        91 | 
##                                        |     0.590 |     0.733 |     0.067 |           | 
##                                        |     0.462 |     0.527 |     0.011 |     0.324 | 
##                                        |     0.288 |     0.366 |     0.250 |           | 
##                                        |     0.149 |     0.171 |     0.004 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
##                                  Owner |        95 |        78 |         2 |       175 | 
##                                        |     0.183 |     0.157 |     0.097 |           | 
##                                        |     0.543 |     0.446 |     0.011 |     0.623 | 
##                                        |     0.651 |     0.595 |     0.500 |           | 
##                                        |     0.338 |     0.278 |     0.007 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
##                           Column Total |       146 |       131 |         4 |       281 | 
##                                        |     0.520 |     0.466 |     0.014 |           | 
## ---------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Housing Graph

```r
housing_bar <- ggplot (data = victoria_merged_filter, aes(housing)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Living Situation ", 
          x = "Living Situation", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(housing_bar)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


## Dwelling Type 

```r
tabyl(victoria_merged_filter$dwelling_type)
```

```
##  victoria_merged_filter$dwelling_type   n     percent
##                                     1 165 0.587188612
##                                     2  15 0.053380783
##                                     3  16 0.056939502
##                                     4  15 0.053380783
##                                     5  44 0.156583630
##                                     6  19 0.067615658
##                                     7   1 0.003558719
##                                     9   6 0.021352313
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(housing_type = case_when(
  dwelling_type == 1 ~ "Single-detached house",
  dwelling_type == 2 ~ "Semi-detached house", 
  dwelling_type == 3 ~ "Row house",
  dwelling_type == 4 ~ "Apartment/Condo in a duplex or triplex", 
  dwelling_type == 5 ~ "Apartment/Condo with fewer than 5 storeys",
  dwelling_type == 6 ~ "Apartment/Condo with more than 5 storeys",
  dwelling_type == 7 ~ "Mobile Home",
  dwelling_type == 8 ~ "Seniors Home",
  dwelling_type == 9 ~ "Other"
))

victoria_merged_filter$housing_type <- factor(victoria_merged_filter$housing_type, c("Other", "Seniors Home", "Mobile Home", "Apartment/Condo with more than 5 storeys", "Apartment/Condo with fewer than 5 storeys", "Apartment/Condo in a duplex or triplex", "Row house", "Semi-detached house", "Single-detached house" )) 
tabyl(victoria_merged_filter$housing_type)
```

```
##        victoria_merged_filter$housing_type   n     percent
##                                      Other   6 0.021352313
##                               Seniors Home   0 0.000000000
##                                Mobile Home   1 0.003558719
##   Apartment/Condo with more than 5 storeys  19 0.067615658
##  Apartment/Condo with fewer than 5 storeys  44 0.156583630
##     Apartment/Condo in a duplex or triplex  15 0.053380783
##                                  Row house  16 0.056939502
##                        Semi-detached house  15 0.053380783
##                      Single-detached house 165 0.587188612
```

## Table for housing type and gender

```r
table(victoria_merged_filter$housing_type, victoria_merged_filter$gender)
```

```
##                                            
##                                             Women Men LGBTQ+
##   Other                                         3   3      0
##   Seniors Home                                  0   0      0
##   Mobile Home                                   0   1      0
##   Apartment/Condo with more than 5 storeys      7  12      0
##   Apartment/Condo with fewer than 5 storeys    28  16      0
##   Apartment/Condo in a duplex or triplex       10   5      0
##   Row house                                     8   8      0
##   Semi-detached house                           7   8      0
##   Single-detached house                        83  78      4
```

```r
CrossTable(victoria_merged_filter$housing_type, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                           | victoria_merged_filter$gender 
##       victoria_merged_filter$housing_type |     Women |       Men |    LGBTQ+ | Row Total | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                                     Other |         3 |         3 |         0 |         6 | 
##                                           |     0.004 |     0.015 |     0.085 |           | 
##                                           |     0.500 |     0.500 |     0.000 |     0.021 | 
##                                           |     0.021 |     0.023 |     0.000 |           | 
##                                           |     0.011 |     0.011 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                               Mobile Home |         0 |         1 |         0 |         1 | 
##                                           |     0.520 |     0.611 |     0.014 |           | 
##                                           |     0.000 |     1.000 |     0.000 |     0.004 | 
##                                           |     0.000 |     0.008 |     0.000 |           | 
##                                           |     0.000 |     0.004 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##  Apartment/Condo with more than 5 storeys |         7 |        12 |         0 |        19 | 
##                                           |     0.835 |     1.115 |     0.270 |           | 
##                                           |     0.368 |     0.632 |     0.000 |     0.068 | 
##                                           |     0.048 |     0.092 |     0.000 |           | 
##                                           |     0.025 |     0.043 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
## Apartment/Condo with fewer than 5 storeys |        28 |        16 |         0 |        44 | 
##                                           |     1.155 |     0.993 |     0.626 |           | 
##                                           |     0.636 |     0.364 |     0.000 |     0.157 | 
##                                           |     0.192 |     0.122 |     0.000 |           | 
##                                           |     0.100 |     0.057 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##    Apartment/Condo in a duplex or triplex |        10 |         5 |         0 |        15 | 
##                                           |     0.625 |     0.568 |     0.214 |           | 
##                                           |     0.667 |     0.333 |     0.000 |     0.053 | 
##                                           |     0.068 |     0.038 |     0.000 |           | 
##                                           |     0.036 |     0.018 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                                 Row house |         8 |         8 |         0 |        16 | 
##                                           |     0.012 |     0.039 |     0.228 |           | 
##                                           |     0.500 |     0.500 |     0.000 |     0.057 | 
##                                           |     0.055 |     0.061 |     0.000 |           | 
##                                           |     0.028 |     0.028 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                       Semi-detached house |         7 |         8 |         0 |        15 | 
##                                           |     0.081 |     0.145 |     0.214 |           | 
##                                           |     0.467 |     0.533 |     0.000 |     0.053 | 
##                                           |     0.048 |     0.061 |     0.000 |           | 
##                                           |     0.025 |     0.028 |     0.000 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                     Single-detached house |        83 |        78 |         4 |       165 | 
##                                           |     0.087 |     0.015 |     1.161 |           | 
##                                           |     0.503 |     0.473 |     0.024 |     0.587 | 
##                                           |     0.568 |     0.595 |     1.000 |           | 
##                                           |     0.295 |     0.278 |     0.014 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
##                              Column Total |       146 |       131 |         4 |       281 | 
##                                           |     0.520 |     0.466 |     0.014 |           | 
## ------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Housing Type and Gender Graph

```r
housing_type_bar <- ggplot (data = victoria_merged_filter, aes(housing_type)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Housing ", 
          x = "Housing Type", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(housing_type_bar)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-23-1.png)<!-- -->


## Health Status

```r
tabyl(victoria_merged_filter$sf1)
```

```
##  victoria_merged_filter$sf1   n     percent
##                           1  69 0.245551601
##                           2 138 0.491103203
##                           3  60 0.213523132
##                           4  13 0.046263345
##                           5   1 0.003558719
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(health_status = case_when(
  sf1 == 1 ~ "Excellent",
  sf1 == 2 ~ "Very Good", 
  sf1 == 3 ~ "Good",
  sf1 == 4 ~ "Fair", 
  sf1 == 5 ~ "Poor"
))

victoria_merged_filter$health_status <- factor(victoria_merged_filter$health_status, c("Poor", "Fair", "Good", "Very Good", "Excellent")) #WORKED - puts the graph in order
```

## Trying a new table 

```r
table_health_status_gender <- table(victoria_merged_filter$health_status, victoria_merged_filter$gender)
table_health_status_gender
```

```
##            
##             Women Men LGBTQ+
##   Poor          0   1      0
##   Fair          6   7      0
##   Good         29  30      1
##   Very Good    76  59      3
##   Excellent    35  34      0
```

```r
margin.table(table_health_status_gender, 1)
```

```
## 
##      Poor      Fair      Good Very Good Excellent 
##         1        13        60       138        69
```

```r
margin.table(table_health_status_gender, 2)
```

```
## 
##  Women    Men LGBTQ+ 
##    146    131      4
```

```r
prop.table(table_health_status_gender)
```

```
##            
##                   Women         Men      LGBTQ+
##   Poor      0.000000000 0.003558719 0.000000000
##   Fair      0.021352313 0.024911032 0.000000000
##   Good      0.103202847 0.106761566 0.003558719
##   Very Good 0.270462633 0.209964413 0.010676157
##   Excellent 0.124555160 0.120996441 0.000000000
```

```r
prop.table(table_health_status_gender, 1)
```

```
##            
##                  Women        Men     LGBTQ+
##   Poor      0.00000000 1.00000000 0.00000000
##   Fair      0.46153846 0.53846154 0.00000000
##   Good      0.48333333 0.50000000 0.01666667
##   Very Good 0.55072464 0.42753623 0.02173913
##   Excellent 0.50724638 0.49275362 0.00000000
```

```r
prop.table(table_health_status_gender, 2)
```

```
##            
##                   Women         Men      LGBTQ+
##   Poor      0.000000000 0.007633588 0.000000000
##   Fair      0.041095890 0.053435115 0.000000000
##   Good      0.198630137 0.229007634 0.250000000
##   Very Good 0.520547945 0.450381679 0.750000000
##   Excellent 0.239726027 0.259541985 0.000000000
```

```r
CrossTable(victoria_merged_filter$health_status, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                      | victoria_merged_filter$gender 
## victoria_merged_filter$health_status |     Women |       Men |    LGBTQ+ | Row Total | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                                 Poor |         0 |         1 |         0 |         1 | 
##                                      |     0.520 |     0.611 |     0.014 |           | 
##                                      |     0.000 |     1.000 |     0.000 |     0.004 | 
##                                      |     0.000 |     0.008 |     0.000 |           | 
##                                      |     0.000 |     0.004 |     0.000 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                                 Fair |         6 |         7 |         0 |        13 | 
##                                      |     0.084 |     0.146 |     0.185 |           | 
##                                      |     0.462 |     0.538 |     0.000 |     0.046 | 
##                                      |     0.041 |     0.053 |     0.000 |           | 
##                                      |     0.021 |     0.025 |     0.000 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                                 Good |        29 |        30 |         1 |        60 | 
##                                      |     0.152 |     0.147 |     0.025 |           | 
##                                      |     0.483 |     0.500 |     0.017 |     0.214 | 
##                                      |     0.199 |     0.229 |     0.250 |           | 
##                                      |     0.103 |     0.107 |     0.004 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                            Very Good |        76 |        59 |         3 |       138 | 
##                                      |     0.258 |     0.442 |     0.546 |           | 
##                                      |     0.551 |     0.428 |     0.022 |     0.491 | 
##                                      |     0.521 |     0.450 |     0.750 |           | 
##                                      |     0.270 |     0.210 |     0.011 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                            Excellent |        35 |        34 |         0 |        69 | 
##                                      |     0.020 |     0.104 |     0.982 |           | 
##                                      |     0.507 |     0.493 |     0.000 |     0.246 | 
##                                      |     0.240 |     0.260 |     0.000 |           | 
##                                      |     0.125 |     0.121 |     0.000 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
##                         Column Total |       146 |       131 |         4 |       281 | 
##                                      |     0.520 |     0.466 |     0.014 |           | 
## -------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

```r
table(victoria_merged_filter$health_status, victoria_merged_filter$gender)
```

```
##            
##             Women Men LGBTQ+
##   Poor          0   1      0
##   Fair          6   7      0
##   Good         29  30      1
##   Very Good    76  59      3
##   Excellent    35  34      0
```

```r
## cross table and table are good. 
```

## Health Status by gender plot 

```r
health_status_plot <- ggplot(data = victoria_merged_filter, aes(health_status)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Health Status and Gender ", 
          x = "Health Status ", 
          y = "Number of Participants (n)") 

plot(health_status_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

## Marital Status

```r
tabyl(victoria_merged_filter$marital_status)
```

```
##  victoria_merged_filter$marital_status   n     percent
##                                      1  54 0.192170819
##                                      2 202 0.718861210
##                                      3  23 0.081850534
##                                      4   2 0.007117438
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(marital = case_when(
  marital_status == 1 ~ "Single (never married)",
  marital_status == 2 ~ "Married (or common law)", 
  marital_status == 3 ~ "Separated or divorced",
  marital_status == 4 ~ "Widowed"
))
tabyl(victoria_merged_filter$marital)
```

```
##  victoria_merged_filter$marital   n     percent
##         Married (or common law) 202 0.718861210
##           Separated or divorced  23 0.081850534
##          Single (never married)  54 0.192170819
##                         Widowed   2 0.007117438
```

## Table for Marital status and gender

```r
table(victoria_merged_filter$marital, victoria_merged_filter$gender)
```

```
##                          
##                           Women Men LGBTQ+
##   Married (or common law)   101  99      2
##   Separated or divorced      13  10      0
##   Single (never married)     30  22      2
##   Widowed                     2   0      0
```

```r
CrossTable(victoria_merged_filter$marital, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                | victoria_merged_filter$gender 
## victoria_merged_filter$marital |     Women |       Men |    LGBTQ+ | Row Total | 
## -------------------------------|-----------|-----------|-----------|-----------|
##        Married (or common law) |       101 |        99 |         2 |       202 | 
##                                |     0.149 |     0.248 |     0.267 |           | 
##                                |     0.500 |     0.490 |     0.010 |     0.719 | 
##                                |     0.692 |     0.756 |     0.500 |           | 
##                                |     0.359 |     0.352 |     0.007 |           | 
## -------------------------------|-----------|-----------|-----------|-----------|
##          Separated or divorced |        13 |        10 |         0 |        23 | 
##                                |     0.092 |     0.049 |     0.327 |           | 
##                                |     0.565 |     0.435 |     0.000 |     0.082 | 
##                                |     0.089 |     0.076 |     0.000 |           | 
##                                |     0.046 |     0.036 |     0.000 |           | 
## -------------------------------|-----------|-----------|-----------|-----------|
##         Single (never married) |        30 |        22 |         2 |        54 | 
##                                |     0.135 |     0.400 |     1.972 |           | 
##                                |     0.556 |     0.407 |     0.037 |     0.192 | 
##                                |     0.205 |     0.168 |     0.500 |           | 
##                                |     0.107 |     0.078 |     0.007 |           | 
## -------------------------------|-----------|-----------|-----------|-----------|
##                        Widowed |         2 |         0 |         0 |         2 | 
##                                |     0.888 |     0.932 |     0.028 |           | 
##                                |     1.000 |     0.000 |     0.000 |     0.007 | 
##                                |     0.014 |     0.000 |     0.000 |           | 
##                                |     0.007 |     0.000 |     0.000 |           | 
## -------------------------------|-----------|-----------|-----------|-----------|
##                   Column Total |       146 |       131 |         4 |       281 | 
##                                |     0.520 |     0.466 |     0.014 |           | 
## -------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Marital Status and Gender Plot

```r
marital_status_plot <- ggplot (data = victoria_merged_filter, aes(marital)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Marital Status and Gender ", 
          x = "Marital Status ", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(marital_status_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

## Children (Yes/No)

```r
tabyl(victoria_merged_filter$children)
```

```
##  victoria_merged_filter$children   n   percent
##                                1 151 0.5373665
##                                2 130 0.4626335
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(children_1 = case_when(
  children == 1 ~ "Yes",
  children == 2 ~ "No"
))
tabyl(victoria_merged_filter$children_1)
```

```
##  victoria_merged_filter$children_1   n   percent
##                                 No 130 0.4626335
##                                Yes 151 0.5373665
```

## Children living 

```r
tabyl(victoria_merged_filter$living_children)
```

```
##  victoria_merged_filter$living_children   n     percent
##                                      -7 130 0.462633452
##                                       1  39 0.138790036
##                                       2  84 0.298932384
##                                       3  23 0.081850534
##                                       4   2 0.007117438
##                                       5   2 0.007117438
##                                       6   1 0.003558719
```

##Children living and gender plot

```r
children_living_plot <- ggplot (data = victoria_merged_filter, aes(living_children)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Children living and Gender ", 
          x = "Children living ", 
          y = "Number of Participants (n)")
plot(children_living_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

## Children living in household

```r
tabyl(victoria_merged_filter$children_household)
```

```
##  victoria_merged_filter$children_household   n    percent
##                                          0 203 0.72241993
##                                          1  34 0.12099644
##                                          2  36 0.12811388
##                                          3   8 0.02846975
```

## Children living in household and Gender Plot

```r
children_household_plot <- ggplot (data = victoria_merged_filter, aes(children_household)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Children living in household and Gender ", 
          x = "Children living in household ", 
          y = "Number of Participants (n)")
plot(children_household_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-34-1.png)<!-- -->

## Born in Canada

```r
tabyl(victoria_merged_filter$born_can)
```

```
##  victoria_merged_filter$born_can   n   percent
##                                1 209 0.7437722
##                                2  72 0.2562278
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(born_canada = case_when(
  born_can == 1 ~ "Yes",
  born_can == 2 ~ "No"
))
tabyl(victoria_merged_filter$born_canada)
```

```
##  victoria_merged_filter$born_canada   n   percent
##                                  No  72 0.2562278
##                                 Yes 209 0.7437722
```

## Born in Canada and Gender Plot

```r
born_canada_plot <- ggplot (data = victoria_merged_filter, aes(born_canada)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Born in Canada and Gender ", 
          x = "Born in Canada ", 
          y = "Number of Participants (n)")
plot(born_canada_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-36-1.png)<!-- -->

## Ethinic/Cultural Groups

```r
tabyl(victoria_merged_filter$group_id)
```

```
##  victoria_merged_filter$group_id   n     percent
##                        [1, 2, 4]   1 0.003558719
##                           [1, 4]   1 0.003558719
##                           [2, 4]   2 0.007117438
##                              [2]  16 0.056939502
##                           [4, 1]   2 0.007117438
##                           [4, 6]   1 0.003558719
##                              [4] 244 0.868327402
##                              [5]   4 0.014234875
##                             [77]  10 0.035587189
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(ethnicity = case_when(
  group_id_1 == 1 & group_id_2 == 1 & group_id_4 == 1 ~ "Aboriginal",
  group_id_1 == 1 & group_id_4 == 1 ~ "Aboriginal",
  group_id_2 == 1 & group_id_4 == 1 ~ "Asian", 
  group_id_4 == 1 & group_id_1 == 1 ~ "Aboriginal",
  group_id_4 == 1 & group_id_6 == 1 ~ "Middle Eastern",
  group_id_2 == 1 ~ "Asian",
  group_id_4 == 1 ~ "Caucasian",
  group_id_5 == 1 ~ "Latin American",
  group_id_77 == 1 ~ "Unknown"
)) 

victoria_merged_filter$ethnicity <- factor(victoria_merged_filter$ethnicity, c("Middle Eastern", "Latin American", "Aboriginal", "Asian", "Caucasian", "Unknown"))
tabyl(victoria_merged_filter$ethnicity)
```

```
##  victoria_merged_filter$ethnicity   n     percent
##                    Middle Eastern   1 0.003558719
##                    Latin American   4 0.014234875
##                        Aboriginal   4 0.014234875
##                             Asian  18 0.064056940
##                         Caucasian 244 0.868327402
##                           Unknown  10 0.035587189
```

## Ethnicity and Gender Plot

```r
ethnicity_plot <- ggplot (data = victoria_merged_filter, aes(ethnicity)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Ethnicity and Gender ", 
          x = "Ethnicity ", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(ethnicity_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-38-1.png)<!-- -->

## Ethnicity grouped by gender

```r
CrossTable(victoria_merged_filter$ethnicity, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                  | victoria_merged_filter$gender 
## victoria_merged_filter$ethnicity |     Women |       Men |    LGBTQ+ | Row Total | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                   Middle Eastern |         1 |         0 |         0 |         1 | 
##                                  |     0.444 |     0.466 |     0.014 |           | 
##                                  |     1.000 |     0.000 |     0.000 |     0.004 | 
##                                  |     0.007 |     0.000 |     0.000 |           | 
##                                  |     0.004 |     0.000 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                   Latin American |         3 |         1 |         0 |         4 | 
##                                  |     0.409 |     0.401 |     0.057 |           | 
##                                  |     0.750 |     0.250 |     0.000 |     0.014 | 
##                                  |     0.021 |     0.008 |     0.000 |           | 
##                                  |     0.011 |     0.004 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                       Aboriginal |         1 |         3 |         0 |         4 | 
##                                  |     0.559 |     0.691 |     0.057 |           | 
##                                  |     0.250 |     0.750 |     0.000 |     0.014 | 
##                                  |     0.007 |     0.023 |     0.000 |           | 
##                                  |     0.004 |     0.011 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                            Asian |         8 |         9 |         1 |        18 | 
##                                  |     0.196 |     0.044 |     2.159 |           | 
##                                  |     0.444 |     0.500 |     0.056 |     0.064 | 
##                                  |     0.055 |     0.069 |     0.250 |           | 
##                                  |     0.028 |     0.032 |     0.004 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                        Caucasian |       127 |       114 |         3 |       244 | 
##                                  |     0.000 |     0.001 |     0.064 |           | 
##                                  |     0.520 |     0.467 |     0.012 |     0.868 | 
##                                  |     0.870 |     0.870 |     0.750 |           | 
##                                  |     0.452 |     0.406 |     0.011 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                          Unknown |         6 |         4 |         0 |        10 | 
##                                  |     0.124 |     0.094 |     0.142 |           | 
##                                  |     0.600 |     0.400 |     0.000 |     0.036 | 
##                                  |     0.041 |     0.031 |     0.000 |           | 
##                                  |     0.021 |     0.014 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                     Column Total |       146 |       131 |         4 |       281 | 
##                                  |     0.520 |     0.466 |     0.014 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```


## Income

```r
tabyl(victoria_merged_filter$income)
```

```
##  victoria_merged_filter$income   n     percent
##                              2   3 0.010676157
##                              3   2 0.007117438
##                              4   5 0.017793594
##                              5   9 0.032028470
##                              6  11 0.039145907
##                              7  16 0.056939502
##                              8 107 0.380782918
##                              9  65 0.231316726
##                             10  34 0.120996441
##                             11   9 0.032028470
##                             77  20 0.071174377
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(income_1 = case_when(
  income == 1 ~ "No income",
  income == 2 ~ "$1 to $9,999", 
  income == 3 ~ "$10,000 to $14,999",
  income == 4 ~ "$15,000 to $19,999", 
  income == 5 ~ "$20,000 to $29,999",
  income == 6 ~ "$30,000 to $39,999",
  income == 7 ~ "$40,000 to $49,999",
  income == 8 ~ "$50,000 to $99,999",
  income == 9 ~ "$100,000 to $149,999",
  income == 10 ~ "$150,000 to $199,999",
  income == 11 ~ "$200,000 or more",
  income == 77 ~ "I don't know/Prefer not to answer"
))

victoria_merged_filter$income_1 <- factor(victoria_merged_filter$income_1, c("No income", "$1 to $9,999", "$10,000 to $14,999", "$15,000 to $19,999", "$20,000 to $29,999", "$30,000 to $39,999", "$40,000 to $49,999","$50,000 to $99,999","$100,000 to $149,999","$150,000 to $199,999", "$200,000 or more", "I don't know/Prefer not to answer"))

tabyl(victoria_merged_filter$income_1)
```

```
##    victoria_merged_filter$income_1   n     percent
##                          No income   0 0.000000000
##                       $1 to $9,999   3 0.010676157
##                 $10,000 to $14,999   2 0.007117438
##                 $15,000 to $19,999   5 0.017793594
##                 $20,000 to $29,999   9 0.032028470
##                 $30,000 to $39,999  11 0.039145907
##                 $40,000 to $49,999  16 0.056939502
##                 $50,000 to $99,999 107 0.380782918
##               $100,000 to $149,999  65 0.231316726
##               $150,000 to $199,999  34 0.120996441
##                   $200,000 or more   9 0.032028470
##  I don't know/Prefer not to answer  20 0.071174377
```

```r
summary(victoria_merged_filter$income)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    2.00    8.00    8.00   13.08    9.00   77.00
```

## Income and Gender Plot

```r
income_plot <- ggplot (data = victoria_merged_filter, aes(income_1)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Income and Gender ", 
          x = "Income", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(income_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-41-1.png)<!-- -->

## Income and Gender Table

```r
CrossTable(victoria_merged_filter$income_1, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                   | victoria_merged_filter$gender 
##   victoria_merged_filter$income_1 |     Women |       Men |    LGBTQ+ | Row Total | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                      $1 to $9,999 |         1 |         2 |         0 |         3 | 
##                                   |     0.200 |     0.259 |     0.043 |           | 
##                                   |     0.333 |     0.667 |     0.000 |     0.011 | 
##                                   |     0.007 |     0.015 |     0.000 |           | 
##                                   |     0.004 |     0.007 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $10,000 to $14,999 |         1 |         1 |         0 |         2 | 
##                                   |     0.001 |     0.005 |     0.028 |           | 
##                                   |     0.500 |     0.500 |     0.000 |     0.007 | 
##                                   |     0.007 |     0.008 |     0.000 |           | 
##                                   |     0.004 |     0.004 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $15,000 to $19,999 |         2 |         3 |         0 |         5 | 
##                                   |     0.138 |     0.192 |     0.071 |           | 
##                                   |     0.400 |     0.600 |     0.000 |     0.018 | 
##                                   |     0.014 |     0.023 |     0.000 |           | 
##                                   |     0.007 |     0.011 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $20,000 to $29,999 |         6 |         3 |         0 |         9 | 
##                                   |     0.375 |     0.341 |     0.128 |           | 
##                                   |     0.667 |     0.333 |     0.000 |     0.032 | 
##                                   |     0.041 |     0.023 |     0.000 |           | 
##                                   |     0.021 |     0.011 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $30,000 to $39,999 |         6 |         5 |         0 |        11 | 
##                                   |     0.014 |     0.003 |     0.157 |           | 
##                                   |     0.545 |     0.455 |     0.000 |     0.039 | 
##                                   |     0.041 |     0.038 |     0.000 |           | 
##                                   |     0.021 |     0.018 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $40,000 to $49,999 |         9 |         7 |         0 |        16 | 
##                                   |     0.057 |     0.028 |     0.228 |           | 
##                                   |     0.562 |     0.438 |     0.000 |     0.057 | 
##                                   |     0.062 |     0.053 |     0.000 |           | 
##                                   |     0.032 |     0.025 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                $50,000 to $99,999 |        54 |        52 |         1 |       107 | 
##                                   |     0.046 |     0.090 |     0.180 |           | 
##                                   |     0.505 |     0.486 |     0.009 |     0.381 | 
##                                   |     0.370 |     0.397 |     0.250 |           | 
##                                   |     0.192 |     0.185 |     0.004 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##              $100,000 to $149,999 |        30 |        32 |         3 |        65 | 
##                                   |     0.421 |     0.095 |     4.652 |           | 
##                                   |     0.462 |     0.492 |     0.046 |     0.231 | 
##                                   |     0.205 |     0.244 |     0.750 |           | 
##                                   |     0.107 |     0.114 |     0.011 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##              $150,000 to $199,999 |        20 |        14 |         0 |        34 | 
##                                   |     0.309 |     0.216 |     0.484 |           | 
##                                   |     0.588 |     0.412 |     0.000 |     0.121 | 
##                                   |     0.137 |     0.107 |     0.000 |           | 
##                                   |     0.071 |     0.050 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                  $200,000 or more |         6 |         3 |         0 |         9 | 
##                                   |     0.375 |     0.341 |     0.128 |           | 
##                                   |     0.667 |     0.333 |     0.000 |     0.032 | 
##                                   |     0.041 |     0.023 |     0.000 |           | 
##                                   |     0.021 |     0.011 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
## I don't know/Prefer not to answer |        11 |         9 |         0 |        20 | 
##                                   |     0.036 |     0.011 |     0.285 |           | 
##                                   |     0.550 |     0.450 |     0.000 |     0.071 | 
##                                   |     0.075 |     0.069 |     0.000 |           | 
##                                   |     0.039 |     0.032 |     0.000 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
##                      Column Total |       146 |       131 |         4 |       281 | 
##                                   |     0.520 |     0.466 |     0.014 |           | 
## ----------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Income needs

```r
tabyl(victoria_merged_filter$income_needs)
```

```
##  victoria_merged_filter$income_needs   n    percent
##                                    1 108 0.38434164
##                                    2 124 0.44128114
##                                    3  33 0.11743772
##                                    4   5 0.01779359
##                                   77  11 0.03914591
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(income_satisfy = case_when(
  income_needs == 1 ~ "Very well",
  income_needs == 2 ~ "Well", 
  income_needs == 3 ~ "Not so well",
  income_needs == 4 ~ "Not at all", 
  income_needs == 77 ~ "I don't know/Prefer not to answer"
))

tabyl(victoria_merged_filter$income_satisfy)
```

```
##  victoria_merged_filter$income_satisfy   n    percent
##      I don't know/Prefer not to answer  11 0.03914591
##                             Not at all   5 0.01779359
##                            Not so well  33 0.11743772
##                              Very well 108 0.38434164
##                                   Well 124 0.44128114
```

## Perceived Income and Gender Plot

```r
perceived_income_plot <- ggplot (data = victoria_merged_filter, aes(income_satisfy)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Income Satisfy Needs and Gender ", 
          x = "Income Satisfaction", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(perceived_income_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-44-1.png)<!-- -->

## Bicycle Ownership Adults

```r
tabyl(victoria_merged_filter$transp_bikes_adults)
```

```
##  victoria_merged_filter$transp_bikes_adults  n     percent
##                                           0  1 0.003558719
##                                           1 33 0.117437722
##                                           2 93 0.330960854
##                                           3 58 0.206405694
##                                           4 35 0.124555160
##                                           5 29 0.103202847
##                                           6 13 0.046263345
##                                           7  7 0.024911032
##                                           8  5 0.017793594
##                                           9  3 0.010676157
##                                          10  2 0.007117438
##                                          14  1 0.003558719
##                                          20  1 0.003558719
```

```r
bicycle_ownership_adults_plot <- ggplot (data = victoria_merged_filter, aes(transp_bikes_adults)) + 
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Adult Bicycle Ownership and Gender ", 
          x = "Number of Bicycles", 
          y = "Number of Participants (n)") 
plot(bicycle_ownership_adults_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-45-1.png)<!-- -->

## Bicycle Ownership Kids

```r
tabyl(victoria_merged_filter$transp_bikes_kids)
```

```
##  victoria_merged_filter$transp_bikes_kids   n     percent
##                                         0 195 0.693950178
##                                         1  29 0.103202847
##                                         2  36 0.128113879
##                                         3   9 0.032028470
##                                         4   9 0.032028470
##                                         5   2 0.007117438
##                                        10   1 0.003558719
```

```r
bicycle_ownership_kids_plot <- ggplot (data = victoria_merged_filter, aes(transp_bikes_kids)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Kid Bicycle Ownership and Gender ", 
          x = "Number of Bicycles", 
          y = "Number of Participants (n)") 
plot(bicycle_ownership_kids_plot)
```

```
## Warning: position_dodge requires non-overlapping x intervals
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-46-1.png)<!-- -->

## Bicycle Facility Preference - Path 

```r
tabyl(victoria_merged_filter$bike_comf_a)
```

```
##  victoria_merged_filter$bike_comf_a   n    percent
##                                   1  21 0.07473310
##                                   3  18 0.06405694
##                                   4 242 0.86120996
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(path_comf = case_when(
  bike_comf_a == 1 ~ "Very uncomfortable",
  bike_comf_a == 2 ~ "Somewhat uncomfortable", 
  bike_comf_a == 3 ~ "Somewhat comfortable",
  bike_comf_a == 4 ~ "Very comfortable"
))

victoria_merged_filter$path_comf <- factor(victoria_merged_filter$path_comf, c("Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

tabyl(victoria_merged_filter$path_comf)
```

```
##  victoria_merged_filter$path_comf   n    percent
##                Very uncomfortable  21 0.07473310
##              Somewhat comfortable  18 0.06405694
##                  Very comfortable 242 0.86120996
```

## Bicycle Facility Preference - Path - Plot

```r
path_comfortable_plot <- ggplot (data = victoria_merged_filter, aes(path_comf)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Bicycle Paths and Gender ", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(path_comfortable_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-48-1.png)<!-- -->

```r
CrossTable(victoria_merged_filter$path_comf, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                  | victoria_merged_filter$gender 
## victoria_merged_filter$path_comf |     Women |       Men |    LGBTQ+ | Row Total | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##               Very uncomfortable |        11 |        10 |         0 |        21 | 
##                                  |     0.001 |     0.005 |     0.299 |           | 
##                                  |     0.524 |     0.476 |     0.000 |     0.075 | 
##                                  |     0.075 |     0.076 |     0.000 |           | 
##                                  |     0.039 |     0.036 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##             Somewhat comfortable |        10 |         8 |         0 |        18 | 
##                                  |     0.045 |     0.018 |     0.256 |           | 
##                                  |     0.556 |     0.444 |     0.000 |     0.064 | 
##                                  |     0.068 |     0.061 |     0.000 |           | 
##                                  |     0.036 |     0.028 |     0.000 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                 Very comfortable |       125 |       113 |         4 |       242 | 
##                                  |     0.004 |     0.000 |     0.089 |           | 
##                                  |     0.517 |     0.467 |     0.017 |     0.861 | 
##                                  |     0.856 |     0.863 |     1.000 |           | 
##                                  |     0.445 |     0.402 |     0.014 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
##                     Column Total |       146 |       131 |         4 |       281 | 
##                                  |     0.520 |     0.466 |     0.014 |           | 
## ---------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Residential street 

```r
tabyl(victoria_merged_filter$bike_comf_b)
```

```
##  victoria_merged_filter$bike_comf_b   n    percent
##                                   1  21 0.07473310
##                                   2  20 0.07117438
##                                   3  87 0.30960854
##                                   4 153 0.54448399
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(residential_street_comf = case_when(
  bike_comf_b == 1 ~ "Very uncomfortable",
  bike_comf_b == 2 ~ "Somewhat uncomfortable", 
  bike_comf_b == 3 ~ "Somewhat comfortable",
  bike_comf_b == 4 ~ "Very comfortable"
))

victoria_merged_filter$residential_street_comf <- factor(victoria_merged_filter$residential_street_comf, c("Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable"))
tabyl(victoria_merged_filter$residential_street_comf)
```

```
##  victoria_merged_filter$residential_street_comf   n    percent
##                              Very uncomfortable  21 0.07473310
##                          Somewhat uncomfortable  20 0.07117438
##                            Somewhat comfortable  87 0.30960854
##                                Very comfortable 153 0.54448399
```

```r
CrossTable(victoria_merged_filter$residential_street_comf, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                                | victoria_merged_filter$gender 
## victoria_merged_filter$residential_street_comf |     Women |       Men |    LGBTQ+ | Row Total | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
##                             Very uncomfortable |        13 |         8 |         0 |        21 | 
##                                                |     0.400 |     0.327 |     0.299 |           | 
##                                                |     0.619 |     0.381 |     0.000 |     0.075 | 
##                                                |     0.089 |     0.061 |     0.000 |           | 
##                                                |     0.046 |     0.028 |     0.000 |           | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
##                         Somewhat uncomfortable |        13 |         7 |         0 |        20 | 
##                                                |     0.655 |     0.579 |     0.285 |           | 
##                                                |     0.650 |     0.350 |     0.000 |     0.071 | 
##                                                |     0.089 |     0.053 |     0.000 |           | 
##                                                |     0.046 |     0.025 |     0.000 |           | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
##                           Somewhat comfortable |        55 |        32 |         0 |        87 | 
##                                                |     2.123 |     1.806 |     1.238 |           | 
##                                                |     0.632 |     0.368 |     0.000 |     0.310 | 
##                                                |     0.377 |     0.244 |     0.000 |           | 
##                                                |     0.196 |     0.114 |     0.000 |           | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
##                               Very comfortable |        65 |        84 |         4 |       153 | 
##                                                |     2.643 |     2.252 |     1.524 |           | 
##                                                |     0.425 |     0.549 |     0.026 |     0.544 | 
##                                                |     0.445 |     0.641 |     1.000 |           | 
##                                                |     0.231 |     0.299 |     0.014 |           | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
##                                   Column Total |       146 |       131 |         4 |       281 | 
##                                                |     0.520 |     0.466 |     0.014 |           | 
## -----------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Residental Street - Plot

```r
residential_street_comfortable_plot <- ggplot (data = victoria_merged_filter, aes(residential_street_comf)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Residential Street and Gender ", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(residential_street_comfortable_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-50-1.png)<!-- -->

## Bicycle Facility Preference - Residential street with traffic calming measures

```r
tabyl(victoria_merged_filter$bike_comf_c)
```

```
##  victoria_merged_filter$bike_comf_c   n     percent
##                                   1  22 0.078291815
##                                   2   4 0.014234875
##                                   3  39 0.138790036
##                                   4 215 0.765124555
##                                  77   1 0.003558719
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(res_street_traffic_calming_comf = case_when(
  bike_comf_c == 1 ~ "Very uncomfortable",
  bike_comf_c == 2 ~ "Somewhat uncomfortable", 
  bike_comf_c == 3 ~ "Somewhat comfortable",
  bike_comf_c == 4 ~ "Very comfortable",
  bike_comf_c == 77 ~ "I don't know/Prefer not to answer"
))

victoria_merged_filter$res_street_traffic_calming_comf <- factor(victoria_merged_filter$res_street_traffic_calming_comf, c( "Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable", "I don't know/Prefer not to answer"))
tabyl(victoria_merged_filter$res_street_traffic_calming_comf)
```

```
##  victoria_merged_filter$res_street_traffic_calming_comf   n     percent
##                                      Very uncomfortable  22 0.078291815
##                                  Somewhat uncomfortable   4 0.014234875
##                                    Somewhat comfortable  39 0.138790036
##                                        Very comfortable 215 0.765124555
##                       I don't know/Prefer not to answer   1 0.003558719
```

```r
CrossTable(victoria_merged_filter$res_street_traffic_calming_comf, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                                        | victoria_merged_filter$gender 
## victoria_merged_filter$res_street_traffic_calming_comf |     Women |       Men |    LGBTQ+ | Row Total | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                     Very uncomfortable |        13 |         9 |         0 |        22 | 
##                                                        |     0.215 |     0.154 |     0.313 |           | 
##                                                        |     0.591 |     0.409 |     0.000 |     0.078 | 
##                                                        |     0.089 |     0.069 |     0.000 |           | 
##                                                        |     0.046 |     0.032 |     0.000 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                 Somewhat uncomfortable |         3 |         1 |         0 |         4 | 
##                                                        |     0.409 |     0.401 |     0.057 |           | 
##                                                        |     0.750 |     0.250 |     0.000 |     0.014 | 
##                                                        |     0.021 |     0.008 |     0.000 |           | 
##                                                        |     0.011 |     0.004 |     0.000 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                   Somewhat comfortable |        17 |        22 |         0 |        39 | 
##                                                        |     0.526 |     0.802 |     0.555 |           | 
##                                                        |     0.436 |     0.564 |     0.000 |     0.139 | 
##                                                        |     0.116 |     0.168 |     0.000 |           | 
##                                                        |     0.060 |     0.078 |     0.000 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                       Very comfortable |       113 |        98 |         4 |       215 | 
##                                                        |     0.015 |     0.050 |     0.288 |           | 
##                                                        |     0.526 |     0.456 |     0.019 |     0.765 | 
##                                                        |     0.774 |     0.748 |     1.000 |           | 
##                                                        |     0.402 |     0.349 |     0.014 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                      I don't know/Prefer not to answer |         0 |         1 |         0 |         1 | 
##                                                        |     0.520 |     0.611 |     0.014 |           | 
##                                                        |     0.000 |     1.000 |     0.000 |     0.004 | 
##                                                        |     0.000 |     0.008 |     0.000 |           | 
##                                                        |     0.000 |     0.004 |     0.000 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                           Column Total |       146 |       131 |         4 |       281 | 
##                                                        |     0.520 |     0.466 |     0.014 |           | 
## -------------------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Residental Street with traffic calming - Plot

```r
res_street_traffic_calming_comfortable_plot <- ggplot (data = victoria_merged_filter, aes(res_street_traffic_calming_comf)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Residential Street with Traffic Calming and Gender", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(res_street_traffic_calming_comfortable_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-52-1.png)<!-- -->

## Bicycle Facility Preference - Major Street with no bike lane 

```r
tabyl(victoria_merged_filter$bike_comf_d)
```

```
##  victoria_merged_filter$bike_comf_d   n    percent
##                                   1 127 0.45195730
##                                   2 106 0.37722420
##                                   3  39 0.13879004
##                                   4   9 0.03202847
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(major_street_no_bike_lane = case_when(
  bike_comf_d == 1 ~ "Very uncomfortable",
  bike_comf_d == 2 ~ "Somewhat uncomfortable", 
  bike_comf_d == 3 ~ "Somewhat comfortable",
  bike_comf_d == 4 ~ "Very comfortable"
))

victoria_merged_filter$major_street_no_bike_lane <- factor(victoria_merged_filter$major_street_no_bike_lane, c("Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable"))
tabyl(victoria_merged_filter$major_street_no_bike_lane)
```

```
##  victoria_merged_filter$major_street_no_bike_lane   n    percent
##                                Very uncomfortable 127 0.45195730
##                            Somewhat uncomfortable 106 0.37722420
##                              Somewhat comfortable  39 0.13879004
##                                  Very comfortable   9 0.03202847
```

```r
CrossTable(victoria_merged_filter$major_street_no_bike_lane, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                                  | victoria_merged_filter$gender 
## victoria_merged_filter$major_street_no_bike_lane |     Women |       Men |    LGBTQ+ | Row Total | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
##                               Very uncomfortable |        84 |        42 |         1 |       127 | 
##                                                  |     4.918 |     5.000 |     0.361 |           | 
##                                                  |     0.661 |     0.331 |     0.008 |     0.452 | 
##                                                  |     0.575 |     0.321 |     0.250 |           | 
##                                                  |     0.299 |     0.149 |     0.004 |           | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
##                           Somewhat uncomfortable |        46 |        58 |         2 |       106 | 
##                                                  |     1.495 |     1.491 |     0.160 |           | 
##                                                  |     0.434 |     0.547 |     0.019 |     0.377 | 
##                                                  |     0.315 |     0.443 |     0.500 |           | 
##                                                  |     0.164 |     0.206 |     0.007 |           | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
##                             Somewhat comfortable |        12 |        26 |         1 |        39 | 
##                                                  |     3.370 |     3.362 |     0.356 |           | 
##                                                  |     0.308 |     0.667 |     0.026 |     0.139 | 
##                                                  |     0.082 |     0.198 |     0.250 |           | 
##                                                  |     0.043 |     0.093 |     0.004 |           | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
##                                 Very comfortable |         4 |         5 |         0 |         9 | 
##                                                  |     0.098 |     0.154 |     0.128 |           | 
##                                                  |     0.444 |     0.556 |     0.000 |     0.032 | 
##                                                  |     0.027 |     0.038 |     0.000 |           | 
##                                                  |     0.014 |     0.018 |     0.000 |           | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
##                                     Column Total |       146 |       131 |         4 |       281 | 
##                                                  |     0.520 |     0.466 |     0.014 |           | 
## -------------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Major street with no bike lane - Plot

```r
major_street_no_bike_lane_plot <- ggplot (data = victoria_merged_filter, aes(major_street_no_bike_lane)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Major Street with No Bike Lane and Gender ", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(major_street_no_bike_lane_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-54-1.png)<!-- -->

## Bicycle Facility Preference - Major Street with bike lane 

```r
tabyl(victoria_merged_filter$bike_comf_e)
```

```
##  victoria_merged_filter$bike_comf_e   n    percent
##                                   1  25 0.08896797
##                                   2  89 0.31672598
##                                   3 128 0.45551601
##                                   4  39 0.13879004
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(major_street_bike_lane = case_when(
  bike_comf_e == 1 ~ "Very uncomfortable",
  bike_comf_e == 2 ~ "Somewhat uncomfortable", 
  bike_comf_e == 3 ~ "Somewhat comfortable",
  bike_comf_e == 4 ~ "Very comfortable"
))

victoria_merged_filter$major_street_bike_lane <- factor(victoria_merged_filter$major_street_bike_lane, c("Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable"))
tabyl(victoria_merged_filter$major_street_bike_lane)
```

```
##  victoria_merged_filter$major_street_bike_lane   n    percent
##                             Very uncomfortable  25 0.08896797
##                         Somewhat uncomfortable  89 0.31672598
##                           Somewhat comfortable 128 0.45551601
##                               Very comfortable  39 0.13879004
```

```r
CrossTable(victoria_merged_filter$major_street_bike_lane, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                               | victoria_merged_filter$gender 
## victoria_merged_filter$major_street_bike_lane |     Women |       Men |    LGBTQ+ | Row Total | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
##                            Very uncomfortable |        16 |         9 |         0 |        25 | 
##                                               |     0.698 |     0.605 |     0.356 |           | 
##                                               |     0.640 |     0.360 |     0.000 |     0.089 | 
##                                               |     0.110 |     0.069 |     0.000 |           | 
##                                               |     0.057 |     0.032 |     0.000 |           | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
##                        Somewhat uncomfortable |        50 |        37 |         2 |        89 | 
##                                               |     0.305 |     0.486 |     0.424 |           | 
##                                               |     0.562 |     0.416 |     0.022 |     0.317 | 
##                                               |     0.342 |     0.282 |     0.500 |           | 
##                                               |     0.178 |     0.132 |     0.007 |           | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
##                          Somewhat comfortable |        65 |        61 |         2 |       128 | 
##                                               |     0.034 |     0.030 |     0.017 |           | 
##                                               |     0.508 |     0.477 |     0.016 |     0.456 | 
##                                               |     0.445 |     0.466 |     0.500 |           | 
##                                               |     0.231 |     0.217 |     0.007 |           | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
##                              Very comfortable |        15 |        24 |         0 |        39 | 
##                                               |     1.367 |     1.862 |     0.555 |           | 
##                                               |     0.385 |     0.615 |     0.000 |     0.139 | 
##                                               |     0.103 |     0.183 |     0.000 |           | 
##                                               |     0.053 |     0.085 |     0.000 |           | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
##                                  Column Total |       146 |       131 |         4 |       281 | 
##                                               |     0.520 |     0.466 |     0.014 |           | 
## ----------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Major street with bike lane - Plot

```r
major_street_bike_lane_plot <- ggplot (data = victoria_merged_filter, aes(major_street_bike_lane)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Major Street with Bike Lane and Gender ", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(major_street_bike_lane_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-56-1.png)<!-- -->

## Bicycle Facility Preference - Major Street with separated bike lane 

```r
tabyl(victoria_merged_filter$bike_comf_f)
```

```
##  victoria_merged_filter$bike_comf_f   n    percent
##                                   1  20 0.07117438
##                                   2   9 0.03202847
##                                   3  58 0.20640569
##                                   4 191 0.67971530
##                                  77   3 0.01067616
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(major_street_separated_bike_lane = case_when(
  bike_comf_f == 1 ~ "Very uncomfortable",
  bike_comf_f == 2 ~ "Somewhat uncomfortable", 
  bike_comf_f == 3 ~ "Somewhat comfortable",
  bike_comf_f == 4 ~ "Very comfortable",
  bike_comf_f == 77 ~ "I don't know/Prefer not to answer"
))

victoria_merged_filter$major_street_separated_bike_lane <- factor(victoria_merged_filter$major_street_separated_bike_lane, c( "Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable", "I don't know/Prefer not to answer"))
tabyl(victoria_merged_filter$major_street_separated_bike_lane)
```

```
##  victoria_merged_filter$major_street_separated_bike_lane   n    percent
##                                       Very uncomfortable  20 0.07117438
##                                   Somewhat uncomfortable   9 0.03202847
##                                     Somewhat comfortable  58 0.20640569
##                                         Very comfortable 191 0.67971530
##                        I don't know/Prefer not to answer   3 0.01067616
```

```r
CrossTable(victoria_merged_filter$major_street_separated_bike_lane, victoria_merged_filter$gender)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  281 
## 
##  
##                                                         | victoria_merged_filter$gender 
## victoria_merged_filter$major_street_separated_bike_lane |     Women |       Men |    LGBTQ+ | Row Total | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                      Very uncomfortable |         8 |        12 |         0 |        20 | 
##                                                         |     0.550 |     0.768 |     0.285 |           | 
##                                                         |     0.400 |     0.600 |     0.000 |     0.071 | 
##                                                         |     0.055 |     0.092 |     0.000 |           | 
##                                                         |     0.028 |     0.043 |     0.000 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                  Somewhat uncomfortable |         6 |         3 |         0 |         9 | 
##                                                         |     0.375 |     0.341 |     0.128 |           | 
##                                                         |     0.667 |     0.333 |     0.000 |     0.032 | 
##                                                         |     0.041 |     0.023 |     0.000 |           | 
##                                                         |     0.021 |     0.011 |     0.000 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                    Somewhat comfortable |        35 |        22 |         1 |        58 | 
##                                                         |     0.785 |     0.939 |     0.037 |           | 
##                                                         |     0.603 |     0.379 |     0.017 |     0.206 | 
##                                                         |     0.240 |     0.168 |     0.250 |           | 
##                                                         |     0.125 |     0.078 |     0.004 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                        Very comfortable |        96 |        92 |         3 |       191 | 
##                                                         |     0.106 |     0.098 |     0.029 |           | 
##                                                         |     0.503 |     0.482 |     0.016 |     0.680 | 
##                                                         |     0.658 |     0.702 |     0.750 |           | 
##                                                         |     0.342 |     0.327 |     0.011 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                       I don't know/Prefer not to answer |         1 |         2 |         0 |         3 | 
##                                                         |     0.200 |     0.259 |     0.043 |           | 
##                                                         |     0.333 |     0.667 |     0.000 |     0.011 | 
##                                                         |     0.007 |     0.015 |     0.000 |           | 
##                                                         |     0.004 |     0.007 |     0.000 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
##                                            Column Total |       146 |       131 |         4 |       281 | 
##                                                         |     0.520 |     0.466 |     0.014 |           | 
## --------------------------------------------------------|-----------|-----------|-----------|-----------|
## 
## 
```

## Bicycle Facility Preference - Major street with separated bike lane - Plot

```r
major_street_separated_bike_lane_plot <- ggplot (data = victoria_merged_filter, aes(major_street_separated_bike_lane)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "Preference for Major Street with Separated Bike Lane and Gender ", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(major_street_separated_bike_lane_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-58-1.png)<!-- -->

### AAA Familiarity 

```r
tabyl(victoria_merged_filter$aaa_familiarity)
```

```
##  victoria_merged_filter$aaa_familiarity   n   percent
##                                       1 189 0.6725979
##                                       2  92 0.3274021
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_familiarity_1 = case_when(
  aaa_familiarity == 1 ~ "Yes",
  aaa_familiarity == 2 ~ "No"
))

tabyl(victoria_merged_filter$aaa_familiarity_1)
```

```
##  victoria_merged_filter$aaa_familiarity_1   n   percent
##                                        No  92 0.3274021
##                                       Yes 189 0.6725979
```

## AAA Familiarity - Plot

```r
aaa_familiarity_plot <- ggplot (data = victoria_merged_filter, aes(aaa_familiarity_1)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "All Ages and Abilities Cycling Network Familiarity ", 
          x = "Familiar with AAA", 
          y = "Number of Participants (n)")
plot(aaa_familiarity_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-60-1.png)<!-- -->

## AAA Good idea


```r
tabyl(victoria_merged_filter$aaa_idea)
```

```
##  victoria_merged_filter$aaa_idea   n     percent
##                                1 243 0.864768683
##                                2  31 0.110320285
##                                3   3 0.010676157
##                                4   1 0.003558719
##                               77   3 0.010676157
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_idea_1 = case_when(
  aaa_idea == 1 ~ "Very good idea",
  aaa_idea == 2 ~ "Somewhat good idea",
  aaa_idea == 3 ~ "Somewhat bad idea",
  aaa_idea == 4 ~ "Very bad idea",
  aaa_idea == 77 ~ "I don't know"
))

victoria_merged_filter$aaa_idea_1 <- factor(victoria_merged_filter$aaa_idea_1, c("I don't know", "Very bad idea", "Somewhat bad idea", "Somewhat good idea", "Very good idea"))
tabyl(victoria_merged_filter$aaa_idea_1)
```

```
##  victoria_merged_filter$aaa_idea_1   n     percent
##                       I don't know   3 0.010676157
##                      Very bad idea   1 0.003558719
##                  Somewhat bad idea   3 0.010676157
##                 Somewhat good idea  31 0.110320285
##                     Very good idea 243 0.864768683
```

## AAA Familiarity - Plot

```r
aaa_idea_1_plot <- ggplot (data = victoria_merged_filter, aes(aaa_idea_1)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "All Ages and Abilities Cycling Network Idea ", 
          x = "Preference for AAA", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(aaa_idea_1_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-62-1.png)<!-- -->

## AAA Cycle More

```r
tabyl(victoria_merged_filter$aaa_bike_more)
```

```
##  victoria_merged_filter$aaa_bike_more   n   percent
##                                     1 221 0.7864769
##                                     2  60 0.2135231
```

```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_bike_more_1 = case_when(
  aaa_bike_more == 1 ~ "Yes",
  aaa_bike_more == 2 ~ "No"
))

tabyl(victoria_merged_filter$aaa_bike_more_1)
```

```
##  victoria_merged_filter$aaa_bike_more_1   n   percent
##                                      No  60 0.2135231
##                                     Yes 221 0.7864769
```

## AAA Bike More - Plot

```r
aaa_bike_more_1_plot <- ggplot (data = victoria_merged_filter, aes(aaa_bike_more_1)) +
    geom_bar(aes(fill = gender), position = "dodge") + 
      labs(title = "All Ages and Abilities Cycling Network - Bicycle more ", 
          x = "Bicycle more",
          y = "Number of Participants (n)")
plot(aaa_bike_more_1_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-64-1.png)<!-- -->

## Physical Activity - Cycling 

```r
tabyl(victoria_merged_filter$travel_bike)
```

```
##  victoria_merged_filter$travel_bike  n    percent
##                                   0  8 0.02846975
##                                   1 13 0.04626335
##                                   2 13 0.04626335
##                                   3 30 0.10676157
##                                   4 35 0.12455516
##                                   5 76 0.27046263
##                                   6 41 0.14590747
##                                   7 65 0.23131673
```

```r
victoria_merged_filter <- mutate(victoria_merged_filter, "Cycling_formula" = 6 * travel_bike_freq * travel_bike)
victoria_merged_filter <- mutate(victoria_merged_filter, "Walking_formula" = 3.3 * travel_walk_freq * travel_walk)
victoria_merged_filter <- mutate(victoria_merged_filter, "Total_transport_formula" = Cycling_formula + Walking_formula)
```

## Plotting PA

```r
PA_cycling_plot <- ggplot (data = victoria_merged_filter, aes(Cycling_formula)) + 
    geom_density(aes(fill = gender), position = "dodge", alpha = 0.5) + 
      labs(title = "Cycling for Transport Physical Activity", 
          x = "Cycling MET Minutes of Physical Activity", 
          y = "Percent of Participants")
plot(PA_cycling_plot)
```

```
## Warning: Removed 3 rows containing non-finite values (stat_density).
```

```
## Warning: Width not defined. Set with `position_dodge(width = ?)`
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-66-1.png)<!-- -->

## Adding normal distribution to PA plot 

```r
victoria_merged_filter$Cycling_formula <- as.numeric(victoria_merged_filter$Cycling_formula)
PA_cycling_normal_plot  <- PA_cycling_plot +
  stat_function(fun = dnorm, args = list(mean = mean(victoria_merged_filter$Cycling_formula, na.rm = TRUE), sd =sd(victoria_merged_filter$Cycling_formula, na.rm = TRUE)), color = "red", size = 1)
plot(PA_cycling_normal_plot)
```

```
## Warning: Removed 3 rows containing non-finite values (stat_density).
```

```
## Warning: Width not defined. Set with `position_dodge(width = ?)`
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-67-1.png)<!-- -->

## Descriptive Stats for Cycling PA

```r
summary(victoria_merged_filter$Cycling_formula)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##       0     750    1440    1727    2160   10080       3
```

```r
describeBy(victoria_merged_filter$Cycling_formula)
```

```
## Warning in describeBy(victoria_merged_filter$Cycling_formula): no grouping
## variable requested
```

```
##    vars   n    mean      sd median trimmed     mad min   max range skew
## X1    1 278 1727.09 1565.53   1440 1441.34 1022.99   0 10080 10080 2.48
##    kurtosis    se
## X1     7.84 93.89
```

```r
describeBy(victoria_merged_filter$Cycling_formula, victoria_merged_filter$gender)
```

```
## 
##  Descriptive statistics by group 
## group: Women
##    vars   n    mean      sd median trimmed   mad min   max range skew
## X1    1 143 1549.51 1494.71   1260 1298.61 800.6   0 10080 10080 3.02
##    kurtosis     se
## X1    11.34 124.99
## -------------------------------------------------------- 
## group: Men
##    vars   n    mean      sd median trimmed     mad min   max range skew
## X1    1 131 1938.41 1635.74   1620 1665.14 1067.47   0 10080 10080 2.01
##    kurtosis     se
## X1     5.28 142.91
## -------------------------------------------------------- 
## group: LGBTQ+
##    vars n mean     sd median trimmed    mad min  max range skew kurtosis
## X1    1 4 1155 844.81   1140    1155 934.04 180 2160  1980 0.03    -2.05
##       se
## X1 422.4
```

```r
tabyl(victoria_merged_filter$Cycling_formula)
```

```
##  victoria_merged_filter$Cycling_formula  n     percent valid_percent
##                                       0  8 0.028469751   0.028776978
##                                      42  1 0.003558719   0.003597122
##                                      90  1 0.003558719   0.003597122
##                                     120  2 0.007117438   0.007194245
##                                     150  1 0.003558719   0.003597122
##                                     180  4 0.014234875   0.014388489
##                                     270  1 0.003558719   0.003597122
##                                     300  2 0.007117438   0.007194245
##                                     360 13 0.046263345   0.046762590
##                                     420  2 0.007117438   0.007194245
##                                     450  2 0.007117438   0.007194245
##                                     480  3 0.010676157   0.010791367
##                                     540  4 0.014234875   0.014388489
##                                     600  7 0.024911032   0.025179856
##                                     630  1 0.003558719   0.003597122
##                                     720 13 0.046263345   0.046762590
##                                     750  6 0.021352313   0.021582734
##                                     810  3 0.010676157   0.010791367
##                                     840  3 0.010676157   0.010791367
##                                     900 13 0.046263345   0.046762590
##                                     960  4 0.014234875   0.014388489
##                                    1050  3 0.010676157   0.010791367
##                                    1080 12 0.042704626   0.043165468
##                                    1200  6 0.021352313   0.021582734
##                                    1260  6 0.021352313   0.021582734
##                                    1350 10 0.035587189   0.035971223
##                                    1440 23 0.081850534   0.082733813
##                                    1470  1 0.003558719   0.003597122
##                                    1500  4 0.014234875   0.014388489
##                                    1560  1 0.003558719   0.003597122
##                                    1620  4 0.014234875   0.014388489
##                                    1650  1 0.003558719   0.003597122
##                                    1680  9 0.032028470   0.032374101
##                                    1800 24 0.085409253   0.086330935
##                                    1890  7 0.024911032   0.025179856
##                                    1980  1 0.003558719   0.003597122
##                                    2160 19 0.067615658   0.068345324
##                                    2520 22 0.078291815   0.079136691
##                                    2880  4 0.014234875   0.014388489
##                                    3600  5 0.017793594   0.017985612
##                                    4320  3 0.010676157   0.010791367
##                                    5040  9 0.032028470   0.032374101
##                                    5760  1 0.003558719   0.003597122
##                                    6480  2 0.007117438   0.007194245
##                                    7200  1 0.003558719   0.003597122
##                                    7560  4 0.014234875   0.014388489
##                                   10080  2 0.007117438   0.007194245
##                                      NA  3 0.010676157            NA
```

## Filtering out other in gender category 


```r
victoria_merged_filter <- victoria_merged_filter %>% mutate(gender_new = case_when(
  gender == "Female" ~ "Female",
  gender == "Male" ~ "Male"
)) 
tabyl(victoria_merged_filter$gender_new)
```

```
##  victoria_merged_filter$gender_new   n percent valid_percent
##                               <NA> 281       1            NA
```

## Regression - Regression Model 1 with just PA and Gender 

```r
gender_regression_1 <- lm(Cycling_formula ~ gender,  data = victoria_merged_filter)

summary(gender_regression_1)
```

```
## 
## Call:
## lm(formula = Cycling_formula ~ gender, data = victoria_merged_filter)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -1938.4  -858.4  -302.3   250.5  8530.5 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    1549.5      130.3  11.896   <2e-16 ***
## genderMen       388.9      188.4   2.064   0.0399 *  
## genderLGBTQ+   -394.5      789.6  -0.500   0.6178    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1558 on 275 degrees of freedom
##   (3 observations deleted due to missingness)
## Multiple R-squared:  0.01719,	Adjusted R-squared:  0.01004 
## F-statistic: 2.405 on 2 and 275 DF,  p-value: 0.09219
```

```r
lm.beta(gender_regression_1) ##This isn't working. 
```

```
## Warning in var(if (is.vector(x) || is.factor(x)) x else as.double(x), na.rm = na.rm): Calling var(x) on a factor x is deprecated and will become an error.
##   Use something like 'all(duplicated(x)[-1L])' to test for a constant vector.
```

```
##    genderMen genderLGBTQ+ 
##    0.1313984   -0.1332935
```

```r
confint(gender_regression_1)
```

```
##                    2.5 %    97.5 %
## (Intercept)   1293.08271 1805.9383
## genderMen       18.04624  759.7572
## genderLGBTQ+ -1949.02130 1160.0003
```

```r
plot(gender_regression_1)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-70-1.png)<!-- -->![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-70-2.png)<!-- -->![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-70-3.png)<!-- -->![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-70-4.png)<!-- -->

## Write CSV

```r
write.csv(victoria_merged_filter, file = "victoria_merged_filter.csv")
```

## Making figure for preference for AT infrastructure

```r
victoria_merged_filter <- read_csv("victoria_merged_filter.csv")
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## Parsed with column specification:
## cols(
##   .default = col_integer(),
##   month = col_character(),
##   day = col_character(),
##   gender_vic.x = col_character(),
##   preferred_mode_f_txt = col_character(),
##   car_share = col_character(),
##   car_share_txt = col_character(),
##   house_tenure_txt = col_character(),
##   dwelling_type_txt = col_character(),
##   living_arrange = col_character(),
##   living_arrange_txt = col_character(),
##   residence = col_date(format = ""),
##   group_id = col_character(),
##   gender_vic.y = col_character(),
##   gender = col_character(),
##   age_categories = col_character(),
##   housing = col_character(),
##   housing_type = col_character(),
##   health_status = col_character(),
##   marital = col_character(),
##   children_1 = col_character()
##   # ... with 16 more columns
## )
```

```
## See spec(...) for full column specifications.
```

```r
victoria_merged_filter$path_comf <- factor(victoria_merged_filter$path_comf, c("Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

path_comfortable_plot <- ggplot (data = victoria_merged_filter, aes(path_comf)) +
    geom_bar(aes()) + 
      labs(title = "Preference for Bicycle Paths", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))

plot(path_comfortable_plot)
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-72-1.png)<!-- -->


```r
at_infrastructure <- victoria_merged_filter %>% dplyr::select(X1, path_comf, residential_street_comf, res_street_traffic_calming_comf, major_street_no_bike_lane, major_street_bike_lane, major_street_separated_bike_lane)

at_infrastructure$path_comf <- factor(at_infrastructure$path_comf, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

at_infrastructure$residential_street_comf <- factor(at_infrastructure$residential_street_comf, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

at_infrastructure$res_street_traffic_calming_comf <- factor(at_infrastructure$res_street_traffic_calming_comf, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

at_infrastructure$major_street_no_bike_lane <- factor(at_infrastructure$major_street_no_bike_lane, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

at_infrastructure$major_street_bike_lane <- factor(at_infrastructure$major_street_bike_lane, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))

at_infrastructure$major_street_separated_bike_lane <- factor(at_infrastructure$major_street_separated_bike_lane, c("I don't know/Prefer not to answer", "Very uncomfortable", "Somewhat comfortable", "Very comfortable"))
```


```r
typeof(at_infrastructure$path_comf)  
```

```
## [1] "integer"
```

```r
at_infrastructure$path_comf <- as.character(at_infrastructure$path_comf)

at_infrastructure %>%
  keep(is.character) %>% 
  gather() %>% 
  ggplot(aes(value)) +
    facet_wrap(~ key, scales = "free") +
    geom_bar() + theme(axis.text.x = element_text(angle=45, hjust=1)) + 
      labs(title = "Preference for Different Types of Active Transportation Infrastructure", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") 
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-74-1.png)<!-- -->


```r
at_infrastructure_1 <- victoria_merged_filter %>% dplyr::select(bike_comf_a, bike_comf_b, bike_comf_c, bike_comf_d, bike_comf_e, bike_comf_f)
at_infrastructure_2 <- at_infrastructure_1 %>% filter(bike_comf_c <= 4) %>% filter(bike_comf_f <= 4)

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_a"] <- "separated_path"

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_b"] <- "residential_street"

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_c"] <- "residential_street_traffic_calming"

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_d"] <- "major_street_no_bike_lane"

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_e"] <- "major_street_bike_lane"

colnames(at_infrastructure_2)[colnames(at_infrastructure_2) == "bike_comf_f"] <- "major_street_separated_bike_lane"

at_infrastructure_2 %>%
  keep(is.numeric) %>% 
  gather() %>% 
  ggplot(aes(value)) +
    facet_wrap(~ key, scales = "free") +
    geom_bar() +
      labs(title = "Preference for Different Types of Active Transportation Infrastructure", 
          x = "Level of Preference", 
          y = "Number of Participants (n)") 
```

![](HKR_6610_gender_analysis_files/figure-html/unnamed-chunk-75-1.png)<!-- -->

