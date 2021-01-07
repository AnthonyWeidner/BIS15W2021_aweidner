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
10/(2+3)
```

```
## [1] 2
```



```r
10/2+3
```

```
## [1] 8
```


```r
10*2
```

```
## [1] 20
```

This is sample text.


```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](Test-RMarkdown_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

