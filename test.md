---
title: "INTERACT Survey Data Victoria"
author: "Melissa Tobin"
date: '2019-05-14'
output: html_document
---

```{r setup, include=FALSE}
chooseCRANmirror(graphics = FALSE, ind = 1)
knitr::opts_chunk$set(echo = TRUE)
```

## Loading Packages
```{r}
install.packages("lmtest")
library(lmtest)
library(tidyverse)
library(ggplot2)
library(haven)
library(janitor)
library(pastecs)
library(psych)
library(car)
library(Hmisc)
library(ggm)
library(polycor)
library(tableone)
library(forcats)
library(gmodels)
library(QuantPsyc)
library(KernSmooth)
library(raster)
library(sp)
library(sf)
```

## Reading in Data
```{r}
getwd()
#setwd("/Volumes/hkr-storage/Research/dfuller/Walkabilly/people/Melissa Tobin/HKR 6000/Data Analysis") #only use on lab computer
#setwd("/Users/MelissaTobin/Documents/HKR 6000/Major Assignment/Data Analysis")
victoria_new <- read_csv("health_1vic_main.bf53654.csv")
victoria_eligibility <- read_csv("eligibility_1vic_main.f209866.csv")

victoria_eligibility[, "interact_id"] <- apply(victoria_eligibility[, "interact_id"], 1, function(x) as.integer(x))
victoria_new[, "interact_id"] <- apply(victoria_new[, "interact_id"], 1, function(x) as.integer(x))

victoria_merged <- full_join(victoria_eligibility, victoria_new, by = "interact_id") #merging eligibility and health survey together 

victoria_merged_filter <- filter(victoria_merged, transp_bikes_adults >= 0) #filtering for participants who did eligibility but didn't actually complete the health survey 
```

## Reading in ID relationship and GPS ID relationship data
```{r}
ID_relationship <- read_csv("Victoria participation_wave1_IDrelationship.csv")

gps_ID_relationship <- read_csv("pairings_with_sdid.csv")
tabyl(gps_ID_relationship$sensedoc_id)
```

## Changing column names and selecting important columns and filtering data
```{r}
ID_relationship <- mutate(ID_relationship, ID_data = 1)

colnames(ID_relationship)[colnames(ID_relationship) == "INTERACT ID"] <- "interact_id"
colnames(ID_relationship)[colnames(ID_relationship) == "Sensedoc ID"] <- "sensedoc_ID"
colnames(ID_relationship)[colnames(ID_relationship) == "Ethica ID"] <- "ethica_ID"

ID_relationship_1 <- dplyr::select(ID_relationship, "interact_id", "sensedoc_ID", "ethica_ID", "ID_data", "treksoft_id")

typeof(ID_relationship_1$treksoft_id)
as.numeric(ID_relationship_1$treksoft_id)

ID_relationship_2 <- ID_relationship_1 %>% mutate(NA_checker = case_when(
  treksoft_id < 908 ~ "1",
  treksoft_id == 908 ~ "2",
  treksoft_id > 908 ~ "1"
)) #participant 908 didn't complete the health survey and was giving alot of problems so we filtered 908 out. 

ID_relationship_3 <- ID_relationship_2 %>% filter(NA_checker == 1)
```

## GPS linking data - changing column names, selecting out important data and working with NA
```{r}
colnames(gps_ID_relationship)[colnames(gps_ID_relationship)== "interact_id"] <- "gps_id"
colnames(gps_ID_relationship)[colnames(gps_ID_relationship)== "sensedoc_id"] <- "sensedoc_ID"

gps_ID_relationship <- dplyr::select(gps_ID_relationship, "gps_id", "sensedoc_ID")

gps_ID_relationship_1 <- mutate(gps_ID_relationship, gps_checker = 1)
```

## Joining Data
```{r}
merged_ID <- left_join(ID_relationship_3, gps_ID_relationship_1, by = "sensedoc_ID") 

merged_ID_gps <- filter(merged_ID, gps_checker == 1)
tabyl(merged_ID$gps_checker)

victoria_new_ID_gps <- left_join(victoria_merged_filter, merged_ID_gps, by = "interact_id")

tabyl(victoria_new_ID_gps$gps_checker)
write_csv(victoria_new_ID_gps, "victoria_new_ID_gps.csv")
```

## Change to updated data frame to run once it gets fixed. Then do another markdown file to run the same analysis for the subset of participants who wore a Sensedoc.

## Descriptive Statistics 
## Age descriptive Statistics for Victoria
```{r}
summary(victoria_merged_filter$age_calculated, na.rm = TRUE)
sd(victoria_merged_filter$age_calculated)

age_categories_plot <- ggplot (data = victoria_merged_filter, aes(age_categories)) + 
    geom_bar() + 
        labs(title = "Age ", 
           x = "Age Categories", 
            y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(age_categories_plot)
```

## Age histogram 
```{r}
age_histogram <- ggplot (data = victoria_merged_filter, aes(age_calculated)) + 
    geom_histogram(aes(y = ..density..), color = "black", binwidth = 0.05) + 
      labs(title = "Barriers to Exercise ", 
          x = "Barriers to Exercise Score", 
          y = "Density (% of Participants)") 
plot(age_histogram)
```


## Gender
```{r}
tabyl(victoria_merged_filter$gender)

gender_plot <- ggplot(data = victoria_merged_filter, aes(gender)) +
  geom_bar(aes(fill = gender)) +
  labs(title = "Gender",
       x = "Gender Type",
       y = "Number of Participants (n)")
plot(gender_plot)
```

## Creating Gender in numbers for correlation
```{r}
victoria_merged_filter <- victoria_merged_filter %>% mutate(gender_number = case_when(
  gender_vic_1.x == 1 & gender_vic_4.x == 1 ~ "1",
  gender_vic_1.x == 1 ~ "1",
  gender_vic_2.x == 1 ~ "2", 
  gender_vic_3.x == 1 ~ "3",
  gender_vic_4.x == 1 ~ "3"
)) 
#update this to what we decided at the trainee summit. 
victoria_merged_filter$gender_number <- as.numeric(victoria_merged_filter$gender_number)
tabyl(victoria_merged_filter$gender_number)
```

## Ethnicity 
```{r}
tabyl(victoria_merged_filter$ethnicity)

ethnicity_plot <- ggplot(data = victoria_merged_filter, aes(ethnicity)) +
  geom_bar() +
  labs(title = "Ethnicity",
       x = "Ethnic Group",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(ethnicity_plot)
```

## Ethnicity recoded as numbers for correlation
```{r}
victoria_merged_filter <- victoria_merged_filter %>% mutate(ethnicity_number = case_when(
  group_id_1 == 1 & group_id_2 == 1 & group_id_4 == 1 ~ "1",
  group_id_1 == 1 & group_id_4 == 1 ~ "1",
  group_id_2 == 1 & group_id_4 == 1 ~ "2", 
  group_id_4 == 1 & group_id_1 == 1 ~ "1",
  group_id_4 == 1 & group_id_6 == 1 ~ "3",
  group_id_2 == 1 ~ "2",
  group_id_4 == 1 ~ "5",
  group_id_5 == 1 ~ "4",
  group_id_77 == 1 ~ "6"
)) 
victoria_merged_filter$ethnicity_number <- as.numeric(victoria_merged_filter$ethnicity_number)
```


## Income
```{r}
victoria_merged_filter$income_1 <- factor(victoria_merged_filter$income_1, c("No income", "$1 to $9,999", "$10,000 to $14,999", "$15,000 to $19,999", "$20,000 to $29,999", "$30,000 to $39,999", "$40,000 to $49,999","$50,000 to $99,999","$100,000 to $149,999","$150,000 to $199,999", "$200,000 or more", "I don't know/Prefer not to answer"))
tabyl(victoria_merged_filter$income_1)
summary(victoria_merged_filter$income)

income_plot <- ggplot(data = victoria_merged_filter, aes(income_1)) +
  geom_bar() +
  labs(title = "Income",
       x = "Income Level",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(income_plot)
```

## Income Needs
```{r}
tabyl(victoria_merged_filter$income_satisfy)
```

## Marital Status
```{r}
tabyl(victoria_merged_filter$marital)

marital_plot <- ggplot(data = victoria_merged_filter, aes(marital)) +
  geom_bar() +
  labs(title = "Marital Status",
       x = "Marital Status",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(marital_plot)
```

## Health Status
```{r}
victoria_merged_filter$health_status <- factor(victoria_merged_filter$health_status, c("Poor", "Fair", "Good", "Very Good", "Excellent"))
tabyl(victoria_merged_filter$health_status)

health_plot <- ggplot(data = victoria_merged_filter, aes(health_status)) +
  geom_bar() +
  labs(title = "Health Status",
       x = "Health Status",
       y = "Number of Participants (n)") 
plot(health_plot)
```

## Support for AAA
## Familiarity AAA
```{r}
tabyl(victoria_merged_filter$aaa_familiarity_1)

AAA_familiarity_plot <- ggplot(data = victoria_merged_filter, aes(aaa_familiarity_1)) +
  geom_bar() +
  labs(title = "All Ages and Abilities Cycling Network Familiarity",
       x = "Yes/No",
       y = "Number of Participants (n)") 
plot(AAA_familiarity_plot)
```

## AAA Familiarity - mutate 
```{r}
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_familiarity_2 = case_when(
  aaa_familiarity_1 == "Yes" ~ "1",
  aaa_familiarity_1 == "No" ~ "0"
))
tabyl(victoria_merged_filter$aaa_familiarity_2)
```

## AAA Idea
```{r}
victoria_merged_filter$aaa_idea_1 <- factor(victoria_merged_filter$aaa_idea_1, c("I don't know", "Very bad idea", "Somewhat bad idea", "Somewhat good idea", "Very good idea"))
tabyl(victoria_merged_filter$aaa_idea_1)

AAA_idea_plot <- ggplot(data = victoria_merged_filter, aes(aaa_idea_1)) +
  geom_bar() +
  labs(title = "All Ages and Abilities Cycling Network Idea",
       x = "AAA Idea",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=60, hjust=1))
plot(AAA_idea_plot)
```

## AAA Idea - mutate 
```{r}
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_idea_2 = case_when(
  aaa_idea_1 == "Very bad idea" ~ "0",
  aaa_idea_1 == "Somewhat bad idea" ~ "0.5", 
  aaa_idea_1 == "Somewhat good idea" ~ "0.5",
  aaa_idea_1 == "Very good idea" ~ "1" 
))
tabyl(victoria_merged_filter$aaa_idea_2)
```

## AAA Bike More
```{r}
tabyl(victoria_merged_filter$aaa_bike_more_1)

AAA_bike_more_plot <- ggplot(data = victoria_merged_filter, aes(aaa_bike_more_1)) +
  geom_bar() +
  labs(title = "All Ages and Abilities Cycling Network Bike More",
       x = "AAA Bike More",
       y = "Number of Participants (n)")
plot(AAA_bike_more_plot)
```

## AAA Bike More - mutate 
```{r}
victoria_merged_filter <- victoria_merged_filter %>% mutate(aaa_bike_more_2 = case_when(
  aaa_bike_more_1 == "Yes" ~ "1",
  aaa_bike_more_1 == "No" ~ "0"
))
tabyl(victoria_merged_filter$aaa_bike_more_2)
```

## Preference for separated 
```{r}
tabyl(victoria_merged_filter$major_street_separated_bike_lane)

victoria_merged_filter <- victoria_merged_filter %>% mutate(separated_bike_lane = case_when(
  major_street_separated_bike_lane == "Somewhat comfortable" ~ "0.5",
  major_street_separated_bike_lane == "Very comfortable" ~ "1",
  major_street_separated_bike_lane == "Very uncomfortable" ~ "0",
   major_street_separated_bike_lane == "Somewhat uncomfortable" ~ "0.5"
))
tabyl(victoria_merged_filter$separated_bike_lane)
```

## Plotting separated 
```{r}
victoria_merged_filter$major_street_separated_bike_lane <- factor(victoria_merged_filter$major_street_separated_bike_lane, c( "Very uncomfortable", "Somewhat uncomfortable", "Somewhat comfortable", "Very comfortable", "I don't know/Prefer not to answer"))
separated_preference <- ggplot(data = victoria_merged_filter, aes(major_street_separated_bike_lane)) +
  geom_bar() +
  labs(title = "Preference for Major Street with Separated Bike Lane",
       x = "Level of Preference",
       y = "Number of Participants (n)") + theme(axis.text.x = element_text(angle=45, hjust=1))
plot(separated_preference)
```

## Creating index
```{r}
victoria_merged_filter$separated_bike_lane <- as.numeric(victoria_merged_filter$separated_bike_lane)
victoria_merged_filter$aaa_bike_more_2 <- as.numeric(victoria_merged_filter$aaa_bike_more_2)
victoria_merged_filter$aaa_familiarity_2 <- as.numeric(victoria_merged_filter$aaa_familiarity_2)
victoria_merged_filter$aaa_idea_2 <- as.numeric(victoria_merged_filter$aaa_idea_2)

victoria_merged_filter <- mutate(victoria_merged_filter, "support_index" = aaa_idea_2 + aaa_familiarity_2 + aaa_bike_more_2 + separated_bike_lane)
tabyl(victoria_merged_filter$support_index)
```

## Plotting support index
```{r}
support_index_plot <- ggplot(data = victoria_merged_filter, aes(support_index)) +
  geom_bar() +
  labs(title = "All Ages and Abilities Cycling Network Support Index",
       x = "Level of Support",
       y = "Number of Participants (n)")
plot(support_index_plot)
```

## Scatterplot 

## Scatterplot analyzing support index and age 
```{r}
scatter_support_age <- ggplot(victoria_merged_filter, aes(support_index, age_calculated))

scatter_support_age_1 <- scatter_support_age +
                           geom_point() +
                             labs(title = "Scatterplot of Age and Support Index for AAA",
                               x = "Support Index for AAA", 
                                y = "Age")
plot(scatter_support_age_1)
```

## Spearman's Correlation Coefficient - Gender
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$gender_number, alternative = "less", method = "spearman")
#not significant - no correlation (4%)
```


## Spearman's Correlation Coefficient - Age
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$age_calculated, alternative = "less", method = "spearman")
#not significant - no correlation (-3.6%)
```

## Spearman's Correlation Coefficient - Health Status
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$sf1, alternative = "less", method = "spearman")
# not significant - no correlation (-.6%)
```

## Spearman's Correlation Coefficient - Income 
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$income, alternative = "less", method = "spearman")
# not significant - no correlation (9.6%)
```

## Spearman's Correlation Coefficient - Marital status
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$marital_status, alternative = "less", method = "spearman")
# not significant - no correlation (5.7%)
```

## Spearman's Correlation Coefficient - Ethnicity
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$ethnicity_number, alternative = "less", method = "spearman")
# not significant - no correlation (.04%)
```

## Spearman's Correlation Coefficient - Cycling MET-minutes of PA
```{r}
cor.test(victoria_merged_filter$support_index, victoria_merged_filter$Cycling_formula, alternative = "less", method = "spearman")
# significant -12%
```

## Spearman's Correlation Coefficient - Cycling MET-minutes of PA and comfortable on separated
```{r}
cor.test(victoria_merged_filter$separated_bike_lane, victoria_merged_filter$Cycling_formula, alternative = "less", method = "spearman")
# significant -14%
```

## Attempting regression between support index and PA
```{r}
support_regression <- lm(Cycling_formula ~ factor(support_index), data = victoria_merged_filter)
summary(support_regression)
```

## Checking self-report exposure
```{r}
tabyl(victoria_merged_filter$vicroads_f)
#86% bike on pandora 
```

## Self-report Physcial Activity Data - Total PA - Outcome Variable 
## Work - Vigorous 
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "work_formula" = 8 * work_vigpa * work_vigpa_freq)
tabyl(victoria_merged_filter$work_formula)
```

## Travel Walking 
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "travel_walk_formula" = 3.3 * travel_walk_freq * travel_walk)
tabyl(victoria_merged_filter$travel_walk_formula)
```

## Leisure Walking
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "leisure_walk_formula" = 3.3 * leisure_walk_freq * leisure_walk)
tabyl(victoria_merged_filter$leisure_walk_formula)
```

## Total Walking MET-minutes/week
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "total_walk_formula" = leisure_walk_formula + travel_walk_formula)
tabyl(victoria_merged_filter$total_walk_formula)
```

## Moderate Leisure
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "mod_leisure_formula" = 4* modpa_leisure_freq + modpa_leisure)
tabyl(victoria_merged_filter$mod_leisure_formula)
```

Vigorous Leisure
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "vig_leisure_formula" = 8* leisure_vigpa_freq + leisure_vigpa)
tabyl(victoria_merged_filter$vig_leisure_formula)
```

## Total Leisure 
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "total_leisure_formula" = leisure_walk_formula + mod_leisure_formula + vig_leisure_formula)
tabyl(victoria_merged_filter$total_leisure_formula)
```

## Total Moderate MET-minutes/week
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "total_moderate_met_formula" = mod_leisure_formula + Cycling_formula)
tabyl(victoria_merged_filter$total_moderate_met_formula)
```

## Total Vigorous MET-minutes/week
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "total_vigorous_met_formula" = work_formula + vig_leisure_formula)
tabyl(victoria_merged_filter$total_vigorous_met_formula)
```

## Total physical activity MET-minutes/week
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "total_pa_met_formula" = total_leisure_formula + total_moderate_met_formula + total_vigorous_met_formula)
tabyl(victoria_merged_filter$total_pa_met_formula)
```

## Plot 
```{r}
PA_total_plot <- ggplot (data = victoria_merged_filter, aes(total_pa_met_formula)) + 
    geom_density() + theme_bw()
    
plot(PA_total_plot)
```

##Determining the proportion of cycling PA in total PA
```{r}
victoria_merged_filter <- mutate(victoria_merged_filter, "proportion_PA" = (Cycling_formula/total_pa_met_formula)*100)
tabyl(victoria_merged_filter$proportion_PA)

PA_proportion_plot <- ggplot (data = victoria_merged_filter, aes(proportion_PA)) + 
    geom_density() + theme_bw()
    
plot(PA_proportion_plot)
```

