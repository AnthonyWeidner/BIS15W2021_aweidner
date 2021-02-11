---
title: "Lab 10 Homework"
author: "Anthony Weidner"
date: "2021-02-11"
output:
  html_document: 
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Parsed with column specification:
## cols(
##   record_id = col_double(),
##   month = col_double(),
##   day = col_double(),
##   year = col_double(),
##   plot_id = col_double(),
##   species_id = col_character(),
##   sex = col_character(),
##   hindfoot_length = col_double(),
##   weight = col_double(),
##   genus = col_character(),
##   species = col_character(),
##   taxa = col_character(),
##   plot_type = col_character()
## )
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  


```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7,…
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 1…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977,…
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2,…
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM",…
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F"…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, N…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipo…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merr…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Ro…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Ro…
```

```r
str(deserts)
```

```
## tibble [34,786 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ record_id      : num [1:34786] 1 2 3 4 5 6 7 8 9 10 ...
##  $ month          : num [1:34786] 7 7 7 7 7 7 7 7 7 7 ...
##  $ day            : num [1:34786] 16 16 16 16 16 16 16 16 16 16 ...
##  $ year           : num [1:34786] 1977 1977 1977 1977 1977 ...
##  $ plot_id        : num [1:34786] 2 3 2 7 3 1 2 1 1 6 ...
##  $ species_id     : chr [1:34786] "NL" "NL" "DM" "DM" ...
##  $ sex            : chr [1:34786] "M" "M" "F" "M" ...
##  $ hindfoot_length: num [1:34786] 32 33 37 36 35 14 NA 37 34 20 ...
##  $ weight         : num [1:34786] NA NA NA NA NA NA NA NA NA NA ...
##  $ genus          : chr [1:34786] "Neotoma" "Neotoma" "Dipodomys" "Dipodomys" ...
##  $ species        : chr [1:34786] "albigula" "albigula" "merriami" "merriami" ...
##  $ taxa           : chr [1:34786] "Rodent" "Rodent" "Rodent" "Rodent" ...
##  $ plot_type      : chr [1:34786] "Control" "Long-term Krat Exclosure" "Control" "Rodent Exclosure" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   record_id = col_double(),
##   ..   month = col_double(),
##   ..   day = col_double(),
##   ..   year = col_double(),
##   ..   plot_id = col_double(),
##   ..   species_id = col_character(),
##   ..   sex = col_character(),
##   ..   hindfoot_length = col_double(),
##   ..   weight = col_double(),
##   ..   genus = col_character(),
##   ..   species = col_character(),
##   ..   taxa = col_character(),
##   ..   plot_type = col_character()
##   .. )
```


```r
naniar::miss_var_summary(deserts)
```

```
## # A tibble: 13 x 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```

NAs are inputted into the data as NAs (the "NA" data type). The data is tidy because it satisfies the following 3 conditions: (1) each variable has its own column, (2) each observation has its own row, (3) each value has its own cell.

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?


```r
deserts %>%
  summarize(different_genera = n_distinct(genus), different_species = n_distinct(species), observations = n())
```

```
## # A tibble: 1 x 3
##   different_genera different_species observations
##              <int>             <int>        <int>
## 1               26                40        34786
```


```r
deserts %>%
  tabyl(species) %>%
  arrange(desc(n))
```

```
##          species     n      percent
##         merriami 10596 3.046053e-01
##     penicillatus  3123 8.977750e-02
##            ordii  3027 8.701777e-02
##          baileyi  2891 8.310815e-02
##        megalotis  2609 7.500144e-02
##      spectabilis  2504 7.198298e-02
##         torridus  2249 6.465245e-02
##           flavus  1597 4.590927e-02
##         eremicus  1299 3.734261e-02
##         albigula  1252 3.599149e-02
##      leucogaster  1006 2.891968e-02
##      maniculatus   899 2.584373e-02
##          harrisi   437 1.256253e-02
##        bilineata   303 8.710401e-03
##        spilosoma   248 7.129305e-03
##         hispidus   179 5.145748e-03
##              sp.    86 2.472259e-03
##        audubonii    75 2.156040e-03
##       fulvescens    75 2.156040e-03
##  brunneicapillus    50 1.437360e-03
##          taylori    46 1.322371e-03
##      fulviventer    43 1.236129e-03
##     ochrognathus    43 1.236129e-03
##        chlorurus    39 1.121141e-03
##         leucopus    36 1.034899e-03
##         squamata    16 4.599552e-04
##      melanocorys    13 3.737136e-04
##      intermedius     9 2.587248e-04
##        gramineus     8 2.299776e-04
##         montanus     8 2.299776e-04
##           fuscus     5 1.437360e-04
##        undulatus     5 1.437360e-04
##       leucophrys     2 5.749439e-05
##       savannarum     2 5.749439e-05
##           clarki     1 2.874720e-05
##       scutalatus     1 2.874720e-05
##     tereticaudus     1 2.874720e-05
##           tigris     1 2.874720e-05
##        uniparens     1 2.874720e-05
##          viridis     1 2.874720e-05
```

Merriami is the most frequently samples. Viridis, uniparens, tigris, tereticaudus, scutalatus, and clarki are the least frequently sampled.

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.


```r
deserts %>%
  tabyl(taxa)
```

```
##     taxa     n      percent
##     Bird   450 0.0129362387
##   Rabbit    75 0.0021560398
##  Reptile    14 0.0004024608
##   Rodent 34247 0.9845052607
```


```r
deserts %>%
  ggplot(aes(x = taxa)) + geom_bar()+
  scale_y_log10() +
  labs(title = "Distribution of taxa",
       x = "Taxa",
       y= "Count (log10)")
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`


```r
deserts %>%
  ggplot(aes(x = taxa, fill = plot_type)) + geom_bar()+
  scale_y_log10() +
  labs(title = "Distribution of taxa",
       x = "Taxa",
       y= "Count (log10)")
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.


```r
deserts %>% 
  filter(!is.na(weight)) %>%
  ggplot(aes(x = species, y = weight, color=species)) +
  labs(title = "Range of weight for each species",
       x = "Species",
       y = "Weight")+
  geom_boxplot()+coord_flip()+
  theme(plot.title = element_text(size = rel(0.95), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.


```r
deserts %>% 
  filter(weight != "NA") %>% 
  ggplot(aes(x = reorder(species, weight), y = weight)) +
  geom_boxplot() +
  geom_point(size=0.2) +
  coord_flip() +
  labs(title = "Range of weight for each species",
       x = "Species",
       y = "Weight") +
  theme(plot.title = element_text(size = rel(0.95), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?


```r
deserts %>%
  filter(species=="merriami" & genus=="Dipodomys") %>%
  ggplot(aes(x = year,color=species)) +
  labs(title = "Number of observations of Diplodomys merriami over the years",
       x = "Year",
       y = "Observations")+
  geom_bar()+
  theme(plot.title = element_text(size = rel(0.95), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.


```r
deserts %>% 
  ggplot(aes(x=weight, y=hindfoot_length)) + geom_point(size=0.1,na.rm=T) +  geom_smooth(method="lm",na.rm=T)
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

There is a positive correlation between weight and hindfoot length. As the weight increases, the hindfoot length also increases.


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.


```r
deserts %>%
  filter(!is.na(weight)) %>%
  group_by(species) %>%
  summarize(avg_weight = mean(weight,na.rm=T)) %>%
  arrange(desc(avg_weight))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 22 x 2
##    species      avg_weight
##    <chr>             <dbl>
##  1 albigula          159. 
##  2 spectabilis       120. 
##  3 spilosoma          93.5
##  4 hispidus           65.6
##  5 fulviventer        58.9
##  6 ochrognathus       55.4
##  7 ordii              48.9
##  8 merriami           43.2
##  9 baileyi            31.7
## 10 leucogaster        31.6
## # … with 12 more rows
```

On average, the species with the highest weight is albigula, and the species with the second highest average weight is spectabilis. 


```r
ratio_data <- deserts %>% 
  filter(species == "albigula" | species == "spectabilis") %>% 
  filter(!is.na(weight)) %>% 
  filter(!is.na(hindfoot_length)) %>% 
  mutate(weight_hindfoot_ratio = weight/hindfoot_length) %>%
  
  ggplot(aes(x = species, y = weight_hindfoot_ratio, fill=sex)) +
  geom_boxplot() +
  geom_point(size=0.2) +
  labs(title = "Boxplot describing weight to hindfood ratio distribution",
       x = "Species", y = "Weight to Hindfoot Length Ratio") +
  theme(plot.title = element_text(size = rel(0.95), hjust = 0.5))

ratio_data
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.


```r
deserts %>%
  filter(!is.na(weight)) %>%
  filter(!is.na(sex)) %>%
  ggplot(aes(x = sex, y = weight)) +
  geom_boxplot() +
  geom_point(size=0.1) +
  labs(title = "Boxplot describing distribution of weight by sex",
       x = "Sex", y = "Weight") +
  theme(plot.title = element_text(size = 12, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 10),
        axis.title = element_text(size = 15))
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
