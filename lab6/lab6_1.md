---
title: "`summarize()`, `tabyl()`, and `group_by()`"
date: "2021-01-26"
output:
  html_document: 
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Use a combination of `select()`, `filter()`, and `mutate()` to transform data frames.  
2. Use the `skimr` package to produce summaries of data.  
3. Produce clean summaries of data using `summarize()`.  
4. Use `group_by()` in combination with `summarize()` to produce grouped summaries of data.  

## Review
At this point, you should be comfortable using the functions of `dplyr`. If you need extra help, please [email me](mailto: jmledford@ucdavis.edu).  

## Package updates
In order to use some of the new function in the second part of lab today, you need to update your installed R packages. Please navigate to `Tools` >`Check for Package Updates...`. Follow the directions to update the packages.  

## Load the tidyverse and janitor

```r
library("tidyverse")
library("janitor")
```

## Install `skimr`

```r
#install.packages("skimr")
library("skimr")
```

```
## Warning: package 'skimr' was built under R version 3.6.2
```

## Load the data
For this lab, we will use the built-in data on mammal sleep patterns. From: _V. M. Savage and G. B. West. A quantitative, theoretical framework for understanding mammalian sleep. Proceedings of the National Academy of Sciences, 104 (3):1051-1056, 2007_.

```r
?msleep
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

We will also use the awesome [palmerpenguins](https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data in the second part of lab so let's install it now.

```r
#remotes::install_github("allisonhorst/palmerpenguins")
```

## dplyr Practice
1. Let's do a bit more practice to make sure that we understand `select()`, `filter()`, and `mutate()`. Start by building a new data frame `msleep24` from the `msleep` data that: contains the `name` and `vore` variables along with a new column called `sleep_total_24` which is the amount of time a species sleeps expressed as a proportion of a 24-hour day. Remove any rows with NA's and restrict the `sleep_total_24` values to less than 0.3. Arrange the output in descending order.  


Did `dplyr` do what we expected? How do we check our output? Remember, just because your code runs it doesn't mean that it did what you intended.

```r
#summary(msleep24)
```

Try out the new function `skim()` as part of the `skimr` package.


Histograms are also a quick way to check the output.


Don't forget we can also use `tabyl()` across one or many variables.


## Practice
1. Which taxonomic orders have species that belong to more than one class of `vore`?


## `summarize()`
`summarize()` will produce summary statistics for a given variable in a data frame. For example, if you are asked to calculate the mean of `sleep_total` for large and small mammals you could do this using a combination of commands, but it isn't very efficient or clean. We can do better!  

```r
head(msleep)
```

```
## # A tibble: 6 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Chee… Acin… carni Carn… lc                  12.1      NA        NA      11.9
## 2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA       7  
## 3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
## 4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
## 5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
## # … with 2 more variables: brainwt <dbl>, bodywt <dbl>
```

For example, if we define "large" as having a `bodywt` greater than 200 then we get the following:

```r
large <- 
  msleep %>% 
  select(name, genus, bodywt, sleep_total) %>% 
  filter(bodywt > 200) %>% 
  arrange(desc(bodywt))
large
```

```
## # A tibble: 7 x 4
##   name             genus         bodywt sleep_total
##   <chr>            <chr>          <dbl>       <dbl>
## 1 African elephant Loxodonta      6654          3.3
## 2 Asian elephant   Elephas        2547          3.9
## 3 Giraffe          Giraffa         900.         1.9
## 4 Pilot whale      Globicephalus   800          2.7
## 5 Cow              Bos             600          4  
## 6 Horse            Equus           521          2.9
## 7 Brazilian tapir  Tapirus         208.         4.4
```


```r
mean(large$sleep_total)
```

```
## [1] 3.3
```

We can accomplish the same task using the `summarize()` function to make things cleaner.

```r
msleep %>% 
  filter(bodywt > 200) %>%
  summarize(mean_sleep_lg = mean(sleep_total))
```

```
## # A tibble: 1 x 1
##   mean_sleep_lg
##           <dbl>
## 1           3.3
```

You can also combine functions to make useful summaries for multiple variables.

```r
msleep %>% 
    filter(bodywt > 200) %>% 
    summarize(mean_sleep_lg = mean(sleep_total), 
              min_sleep_lg = min(sleep_total),
              max_sleep_lg = max(sleep_total),
              total = n())
```

```
## # A tibble: 1 x 4
##   mean_sleep_lg min_sleep_lg max_sleep_lg total
##           <dbl>        <dbl>        <dbl> <int>
## 1           3.3          1.9          4.4     7
```

## Practice
1. What is the mean, min, and max `bodywt` for the taxonomic order Primates? Provide the total number of observations.


`n_distinct()` is a very handy way of cleanly presenting the number of distinct observations. Here we show the number of distinct genera over 100 in body weight.

```r
msleep %>% 
  filter(bodywt > 100) %>% 
  summarise(n_genera=n_distinct(genus))
```

```
## # A tibble: 1 x 1
##   n_genera
##      <int>
## 1        9
```

There are many other useful summary statistics, depending on your needs: sd(), min(), max(), median(), sum(), n() (returns the length of a column), first() (returns first value in a column), last() (returns last value in a column) and n_distinct() (number of distinct values in a column).

## Practice
1. How many genera are represented in the msleep data frame?


2. What are the min, max, and mean `sleep_total` for all of the mammals? Be sure to include the total n.


## `group_by()`
The `summarize()` function is most useful when used in conjunction with `group_by()`. Although producing a summary of body weight for all of the mammals in the data set is helpful, what if we were interested in body weight by feeding ecology?

```r
msleep %>%
  group_by(vore) %>% #we are grouping by feeding ecology
  summarize(min_bodywt = min(bodywt),
            max_bodywt = max(bodywt),
            mean_bodywt = mean(bodywt),
            total=n())
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 5 x 5
##   vore    min_bodywt max_bodywt mean_bodywt total
##   <chr>        <dbl>      <dbl>       <dbl> <int>
## 1 carni        0.028      800        90.8      19
## 2 herbi        0.022     6654       367.       32
## 3 insecti      0.01        60        12.9       5
## 4 omni         0.005       86.2      12.7      20
## 5 <NA>         0.021        3.6       0.858     7
```

## Practice
1. Calculate mean brain weight by taxonomic order in the msleep data.


2. What does `NA` mean? How are NA's being treated by the summarize function?


3. Try running the code again, but this time add `na.rm=TRUE`. What is the problem with Cetacea? Compare this to Carnivora. 


## That's it! Take a break and I will see you on Zoom!  

-->[Home](https://jmledford3115.github.io/datascibiol/)  
