---
title: "Midterm 1"
author: "Anthony Weidner"
date: "2021-01-29"
output:
  html_document: 
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library(janitor)
library(skimr)
options(dplyr.summarise.inform = FALSE)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

R is an open source, scripting language, while RStudio is the Graphical User Interface (GUI) that is used to ineract with R and serves as an integrated development environment for the R programming language. This explains why R must be installed in order to run RStudio. GitHub is a file storage and management site used commonly by programmers, serving as an online collection of repositories. This allows fellow coders to see and interact with other people's code to make data science transparent and repeatable. Individuals can check their code with others, use bits of others' code in their programs, and collaborate with others on a project through Github, thus enabling work flows in data science.  It serves as a means of sharing and accessing other programmers' code and files. Individuals using GitHub can upload their code to repositories to make them publicly available to other programmers. RMarkdown in this context allows code displayed in GitHub to appear neat, organized, and clean. This further adds on to the work flow because people viewing others' R code on GitHub can do so with ease and clarity, thanks to RMarkdown. R Markdown provides quick, reproducible reporting from R that is easy for others to view and incorporate within their own code and projects.

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**

The three types of data structures that we have discussed are vectors, matrices, and data frames. Data frames are extremely useful because they provide a neatly-formatted, clean, and organized way of storing data. Using a data frame also facilitates efficient manipulation and analysis of the data through functions such as filter, group_by, and select. Thus, data frames not only serve as an effective and easily-readable means of storing data, but also as a way of transforming and interacting with data. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**


```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Parsed with column specification:
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```


**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**


```r
elephants <- janitor::clean_names(elephants)
elephants$sex <- as.factor(elephants$sex)
elephants
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # … with 278 more rows
```


**5. (2 points) How many male and female elephants are represented in the data?**


```r
elephants %>%
  group_by(sex) %>%
  tabyl(sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```

As shown above, there are 138 male elephants and 150 female elephants.

**6. (2 points) What is the average age all elephants in the data?**


```r
average_age <- mean(elephants$age)
average_age
```

```
## [1] 10.97132
```


**7. (2 points) How does the average age and height of elephants compare by sex?**


```r
elephants %>%
  group_by(sex) %>%
  summarize(mean_age = mean(age), mean_height = mean(height))
```

```
## # A tibble: 2 x 3
##   sex   mean_age mean_height
##   <fct>    <dbl>       <dbl>
## 1 F        12.8         190.
## 2 M         8.95        185.
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**


```r
elephants %>%
  filter(age>25) %>%
  group_by(sex) %>%
  summarize(average_height = mean(height), min_height = min(height), max_height = max(height), total = n())
```

```
## # A tibble: 2 x 5
##   sex   average_height min_height max_height total
##   <fct>          <dbl>      <dbl>      <dbl> <int>
## 1 F               233.       206.       278.    25
## 2 M               273.       237.       304.     8
```

The average height of elephants is greater for males than for females when considering elephants over 25 years old. Additionally, the minimum height for elephants over 25 years old is 206.07 (206.07 minimum for female elephants over 25 years old and 237.19 minimum for male elephants over 25 years old). The maximum height for elephants over 25 years old is 304.06 (304.06 maximum for male elephants over 25 years old and 277.80 maximum for female elephants over 25 years old). The total number of individuals in the sample is 33, with 25 being female and 8 being male.


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
defaunation <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
```

```
## See spec(...) for full column specifications.
```

```r
str(defaunation)
```

```
## tibble [24 × 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ TransectID             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ Distance               : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ HuntCat                : chr [1:24] "Moderate" "None" "None" "None" ...
##  $ NumHouseholds          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ LandUse                : chr [1:24] "Park" "Park" "Park" "Logging" ...
##  $ Veg_Rich               : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ Veg_Stems              : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ Veg_liana              : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ Veg_DBH                : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ Veg_Canopy             : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ Veg_Understory         : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ RA_Apes                : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ RA_Birds               : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ RA_Elephant            : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ RA_Monkeys             : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ RA_Rodent              : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ RA_Ungulate            : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ Rich_AllSpecies        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ Evenness_AllSpecies    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ Diversity_AllSpecies   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ Rich_BirdSpecies       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ Evenness_BirdSpecies   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ Diversity_BirdSpecies  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ Rich_MammalSpecies     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ Evenness_MammalSpecies : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ Diversity_MammalSpecies: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
```

```r
skim(defaunation)
```


Table: Data summary

|                         |            |
|:------------------------|:-----------|
|Name                     |defaunation |
|Number of rows           |24          |
|Number of columns        |26          |
|_______________________  |            |
|Column type frequency:   |            |
|character                |2           |
|numeric                  |24          |
|________________________ |            |
|Group variables          |None        |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|HuntCat       |         0|             1|   4|   8|     0|        3|          0|
|LandUse       |         0|             1|   4|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|  mean|    sd|    p0|   p25|   p50|   p75|  p100|hist  |
|:-----------------------|---------:|-------------:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|:-----|
|TransectID              |         0|             1| 13.50|  8.51|  1.00|  5.75| 14.50| 20.25| 27.00|▇▃▅▆▆ |
|Distance                |         0|             1| 11.88|  7.28|  2.70|  5.67|  9.72| 17.68| 26.76|▇▂▂▅▂ |
|NumHouseholds           |         0|             1| 37.88| 17.80| 13.00| 24.75| 29.00| 54.00| 73.00|▇▇▂▇▂ |
|Veg_Rich                |         0|             1| 14.83|  2.07| 10.88| 13.10| 14.94| 16.54| 18.75|▃▂▃▇▁ |
|Veg_Stems               |         0|             1| 32.80|  5.96| 23.44| 28.69| 32.45| 37.08| 47.56|▆▇▆▃▁ |
|Veg_liana               |         0|             1| 11.04|  3.29|  4.75|  9.03| 11.94| 13.25| 16.38|▃▂▃▇▃ |
|Veg_DBH                 |         0|             1| 46.09| 10.67| 28.45| 40.65| 43.90| 50.57| 76.48|▂▇▃▁▁ |
|Veg_Canopy              |         0|             1|  3.47|  0.35|  2.50|  3.25|  3.43|  3.75|  4.00|▁▁▇▅▇ |
|Veg_Understory          |         0|             1|  3.02|  0.34|  2.38|  2.88|  3.00|  3.17|  3.88|▂▆▇▂▁ |
|RA_Apes                 |         0|             1|  2.04|  3.03|  0.00|  0.00|  0.48|  3.82| 12.93|▇▂▁▁▁ |
|RA_Birds                |         0|             1| 58.64| 14.71| 31.56| 52.51| 57.89| 68.18| 85.03|▅▅▇▇▃ |
|RA_Elephant             |         0|             1|  0.55|  0.67|  0.00|  0.00|  0.36|  0.89|  2.30|▇▂▂▁▁ |
|RA_Monkeys              |         0|             1| 31.30| 12.38|  5.84| 22.70| 31.74| 39.88| 54.12|▂▅▃▇▂ |
|RA_Rodent               |         0|             1|  3.28|  1.47|  1.06|  2.05|  3.23|  4.09|  6.31|▇▅▇▃▃ |
|RA_Ungulate             |         0|             1|  4.17|  4.31|  0.00|  1.23|  2.54|  5.16| 13.86|▇▂▁▁▂ |
|Rich_AllSpecies         |         0|             1| 20.21|  2.06| 15.00| 19.00| 20.00| 22.00| 24.00|▁▁▇▅▁ |
|Evenness_AllSpecies     |         0|             1|  0.77|  0.05|  0.67|  0.75|  0.78|  0.81|  0.83|▃▁▅▇▇ |
|Diversity_AllSpecies    |         0|             1|  2.31|  0.15|  1.97|  2.25|  2.32|  2.43|  2.57|▂▃▇▆▅ |
|Rich_BirdSpecies        |         0|             1| 10.33|  1.24|  8.00| 10.00| 11.00| 11.00| 13.00|▃▅▇▁▁ |
|Evenness_BirdSpecies    |         0|             1|  0.71|  0.08|  0.56|  0.68|  0.72|  0.77|  0.82|▅▁▇▆▇ |
|Diversity_BirdSpecies   |         0|             1|  1.66|  0.20|  1.16|  1.60|  1.68|  1.78|  2.01|▂▂▅▇▃ |
|Rich_MammalSpecies      |         0|             1|  9.88|  1.68|  6.00|  9.00| 10.00| 11.00| 12.00|▂▂▃▅▇ |
|Evenness_MammalSpecies  |         0|             1|  0.75|  0.06|  0.62|  0.71|  0.74|  0.78|  0.86|▂▃▇▂▅ |
|Diversity_MammalSpecies |         0|             1|  1.70|  0.17|  1.38|  1.57|  1.70|  1.81|  2.06|▅▇▇▇▃ |

```r
glimpse(defaunation)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16,…
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.0…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "…
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 5…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "P…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, …
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.0…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.7…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.4…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.2…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, …
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 2…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.8…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.5…
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, …
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.8…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.9…
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11,…
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.7…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.9…
```

```r
defaunation$HuntCat <- as.factor(defaunation$HuntCat)
defaunation$LandUse <- as.factor(defaunation$LandUse)
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**


```r
defaunation %>%
  group_by(HuntCat) %>%
  filter(HuntCat == "Moderate" | HuntCat == "High") %>%
  summarize(average_diversity_birds = mean(Diversity_BirdSpecies), average_diversity_mammals = mean(Diversity_MammalSpecies))
```

```
## # A tibble: 2 x 3
##   HuntCat  average_diversity_birds average_diversity_mammals
##   <fct>                      <dbl>                     <dbl>
## 1 High                        1.66                      1.74
## 2 Moderate                    1.62                      1.68
```

The average diversity of birds for high hunting intensity is higher than the average diversity of birds for moderate hunting intensity. The average diversity of mammals for high hunting intensity is higher than the average diversity of mammals for moderate hunting intensity. 

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  



```r
defaunation %>%
  filter(Distance<5 | Distance>20) %>%
  group_by(Distance<5, Distance>20) %>%
  summarize(across(starts_with("RA_"), mean, na.rm=T))
```

```
## # A tibble: 2 x 8
## # Groups:   Distance < 5 [2]
##   `Distance < 5` `Distance > 20` RA_Apes RA_Birds RA_Elephant RA_Monkeys
##   <lgl>          <lgl>             <dbl>    <dbl>       <dbl>      <dbl>
## 1 FALSE          TRUE               7.21     44.5      0.557        40.1
## 2 TRUE           FALSE              0.08     70.4      0.0967       24.1
## # … with 2 more variables: RA_Rodent <dbl>, RA_Ungulate <dbl>
```

The relative abundances for apes, elephants, monkeys, and ungulates are higher for sites that are greater than 20km from a village. The relative abundances for birds and rodents are higher for sites that are less than 5km from a village.

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**


```r
defaunation %>%
  select(Distance, HuntCat ,Diversity_AllSpecies) %>%
  filter(HuntCat != "None") %>%
  arrange(desc(Distance)) 
```

```
## # A tibble: 15 x 3
##    Distance HuntCat  Diversity_AllSpecies
##       <dbl> <fct>                   <dbl>
##  1    15.0  Moderate                 2.32
##  2    14.0  Moderate                 2.19
##  3    11.2  Moderate                 2.37
##  4     8.23 Moderate                 2.16
##  5     7.14 Moderate                 2.45
##  6     6.61 Moderate                 2.27
##  7     6.61 Moderate                 2.27
##  8     6.1  Moderate                 2.13
##  9     5.78 High                     2.28
## 10     5.33 High                     2.37
## 11     5.14 High                     2.45
## 12     5.13 High                     1.97
## 13     3.83 High                     2.45
## 14     2.92 High                     2.38
## 15     2.7  High                     2.25
```

As shown above, I selected for the variables distance, HuntCat, and Diversity_AllSpecies. I then filtered out HuntCat values of "None" and arranged the distances by descending order. We can see a correlation between the distance from the village and HuntCat - as we get closer to the village, the hunting intensity increases. 
