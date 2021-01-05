---
title: "RMarkdown Practice"
output: 
  html_document: 
    keep_md: yes
---



## R Markdown
### Test
# Test1

```r
4*2
```

```
## [1] 8
```

```r
5-3*7
```

```
## [1] -16
```


```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](Test-RMarkdown_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

