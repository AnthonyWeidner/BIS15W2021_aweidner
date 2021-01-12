---
title: "Lab 2 Homework"
author: "Anthony Weidner"
date: "2021-01-12"
output:
  html_document: 
    keep_md: yes
    theme: spacelab
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

A vector in R is a common way of organizing and/or storing data. Vectors can be created using the c command in R, which stands for concatenate. 

2. What is a data matrix in R?  

A data matrix in R is a series of stacked vectors (similar to a data table). It is a great way of storing data and information, which can be used to perform certain calculations, including the sum and mean of specific columns and rows if wanted. New data matrices can be built using the matrix command. 

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

I ran the code chunk to generate the vectors.

4. Build a data matrix that has the springs as rows and the columns as scientists.  


```r
spring_combinedVector <- c(spring_1,spring_2,spring_3,spring_4,spring_5,spring_6,spring_7,spring_8)
spring_combinedVector
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
spring_matrix <- matrix(spring_combinedVector,nrow=8,byrow=T)
spring_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
spring_names <- c("1. Bluebell Spring", "2. Opal Spring", "3. Riverside Spring", "4. Too Hot Spring", "5. Mystery Spring", "6. Emerald Spring", "7. Black Spring", "8. Pearl Spring")
spring_names
```

```
## [1] "1. Bluebell Spring"  "2. Opal Spring"      "3. Riverside Spring"
## [4] "4. Too Hot Spring"   "5. Mystery Spring"   "6. Emerald Spring"  
## [7] "7. Black Spring"     "8. Pearl Spring"
```

```r
scientist_names <- c("Jill", "Steve", "Susan")
scientist_names
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
colnames(spring_matrix) <- scientist_names
rownames(spring_matrix) <- spring_names

spring_matrix
```

```
##                      Jill Steve Susan
## 1. Bluebell Spring  36.25 35.40 35.30
## 2. Opal Spring      35.15 35.35 33.35
## 3. Riverside Spring 30.70 29.65 29.20
## 4. Too Hot Spring   39.70 40.05 38.65
## 5. Mystery Spring   31.85 31.40 29.30
## 6. Emerald Spring   30.20 30.65 29.75
## 7. Black Spring     32.90 32.50 32.80
## 8. Pearl Spring     36.80 36.45 33.15
```


6. Calculate the mean temperature of all three springs.


```r
AverageTemp <- rowMeans(spring_matrix)
AverageTemp
```

```
##  1. Bluebell Spring      2. Opal Spring 3. Riverside Spring   4. Too Hot Spring 
##            35.65000            34.61667            29.85000            39.46667 
##   5. Mystery Spring   6. Emerald Spring     7. Black Spring     8. Pearl Spring 
##            30.85000            30.20000            32.73333            35.46667
```


7. Add this as a new column in the data matrix.  


```r
spring_matrixRevised <- cbind(spring_matrix,AverageTemp)
spring_matrixRevised
```

```
##                      Jill Steve Susan AverageTemp
## 1. Bluebell Spring  36.25 35.40 35.30    35.65000
## 2. Opal Spring      35.15 35.35 33.35    34.61667
## 3. Riverside Spring 30.70 29.65 29.20    29.85000
## 4. Too Hot Spring   39.70 40.05 38.65    39.46667
## 5. Mystery Spring   31.85 31.40 29.30    30.85000
## 6. Emerald Spring   30.20 30.65 29.75    30.20000
## 7. Black Spring     32.90 32.50 32.80    32.73333
## 8. Pearl Spring     36.80 36.45 33.15    35.46667
```


8. Show Susan's value for Opal Spring only.


```r
spring_matrixRevised[2,3]
```

```
## [1] 33.35
```

As shown above, the command gives Susan's value for Opal Spring, which is 33.35.


9. Calculate the mean for Jill's column only.  


```r
jill_data <- spring_matrixRevised[,1]

jill_average <- mean(jill_data)
jill_average
```

```
## [1] 34.19375
```

As shown above, the mean for Jill's column is 34.19375.

10. Use the data matrix to perform one calculation or operation of your interest.


```r
spring_matrixRevised[1:5, 4]
```

```
##  1. Bluebell Spring      2. Opal Spring 3. Riverside Spring   4. Too Hot Spring 
##            35.65000            34.61667            29.85000            39.46667 
##   5. Mystery Spring 
##            30.85000
```

I used the above command to display the average temperatures for only the first 5 springs (Bluebell, Opal, Riverside, Too Hot, and Mystery). 

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
