---
title: "Lab 3 Homework"
author: "Shefali Suresh"
date: "2023-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

The data was taken from the National Academy of Sciences. 

```r
msleep
```

```
## # A tibble: 83 × 11
##    name         genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake  brainwt
##    <chr>        <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>    <dbl>
##  1 Cheetah      Acin… carni Carn… lc         12.1    NA    NA      11.9 NA      
##  2 Owl monkey   Aotus omni  Prim… <NA>       17       1.8  NA       7    0.0155 
##  3 Mountain be… Aplo… herbi Rode… nt         14.4     2.4  NA       9.6 NA      
##  4 Greater sho… Blar… omni  Sori… lc         14.9     2.3   0.133   9.1  0.00029
##  5 Cow          Bos   herbi Arti… domest…     4       0.7   0.667  20    0.423  
##  6 Three-toed … Brad… herbi Pilo… <NA>       14.4     2.2   0.767   9.6 NA      
##  7 Northern fu… Call… carni Carn… vu          8.7     1.4   0.383  15.3 NA      
##  8 Vesper mouse Calo… <NA>  Rode… <NA>        7      NA    NA      17   NA      
##  9 Dog          Canis carni Carn… domest…    10.1     2.9   0.333  13.9  0.07   
## 10 Roe deer     Capr… herbi Arti… lc          3      NA    NA      21    0.0982 
## # … with 73 more rows, 1 more variable: bodywt <dbl>, and abbreviated variable
## #   names ¹​conservation, ²​sleep_total, ³​sleep_rem, ⁴​sleep_cycle
```

2. Store these data into a new data frame `sleep`.


```r
sleep <- msleep
```


3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below. 

The function str() is used to find the dimensions. This tells that there are 83 observations and 11 variables. 

```r
str(sleep)
```

```
## tibble [83 × 11] (S3: tbl_df/tbl/data.frame)
##  $ name        : chr [1:83] "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr [1:83] "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr [1:83] "carni" "omni" "herbi" "omni" ...
##  $ order       : chr [1:83] "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr [1:83] "lc" NA "nt" "lc" ...
##  $ sleep_total : num [1:83] 12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num [1:83] NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num [1:83] NA NA NA 0.133 0.667 ...
##  $ awake       : num [1:83] 11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num [1:83] NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num [1:83] 50 0.48 1.35 0.019 600 ...
```

The function dim() can also be used to find the dimensions. This also tells us that there are 83 columns and 11 rows. 

```r
dim(sleep)
```

```
## [1] 83 11
```


4. Are there any NAs in the data? How did you determine this? Please show your code.  

Yes, there are NAs in the data. I used the function anyNA() because it uses True or False to show if there is NAs in the data. For example, the function will be True if a NA exists. 

```r
anyNA(sleep)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

Using the table() function, there are 32 herbivores represented in the data.

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```


```r
num_herbi <- filter(sleep, vore == "herbi")
num_herbi
```

```
## # A tibble: 32 × 11
##    name  genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake brainwt  bodywt
##    <chr> <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>   <dbl>   <dbl>
##  1 Moun… Aplo… herbi Rode… nt         14.4     2.4  NA       9.6 NA      1.35e+0
##  2 Cow   Bos   herbi Arti… domest…     4       0.7   0.667  20    0.423  6   e+2
##  3 Thre… Brad… herbi Pilo… <NA>       14.4     2.2   0.767   9.6 NA      3.85e+0
##  4 Roe … Capr… herbi Arti… lc          3      NA    NA      21    0.0982 1.48e+1
##  5 Goat  Capri herbi Arti… lc          5.3     0.6  NA      18.7  0.115  3.35e+1
##  6 Guin… Cavis herbi Rode… domest…     9.4     0.8   0.217  14.6  0.0055 7.28e-1
##  7 Chin… Chin… herbi Rode… domest…    12.5     1.5   0.117  11.5  0.0064 4.2 e-1
##  8 Tree… Dend… herbi Hyra… lc          5.3     0.5  NA      18.7  0.0123 2.95e+0
##  9 Asia… Elep… herbi Prob… en          3.9    NA    NA      20.1  4.60   2.55e+3
## 10 Horse Equus herbi Peri… domest…     2.9     0.6   1      21.1  0.655  5.21e+2
## # … with 22 more rows, and abbreviated variable names ¹​conservation,
## #   ²​sleep_total, ³​sleep_rem, ⁴​sleep_cycle
```


7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.


Small Animals 

```r
small <- filter(msleep, bodywt <= 1)
small
```

```
## # A tibble: 36 × 11
##    name  genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake  brainwt bodywt
##    <chr> <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>    <dbl>  <dbl>
##  1 Owl … Aotus omni  Prim… <NA>       17       1.8  NA       7    0.0155   0.48 
##  2 Grea… Blar… omni  Sori… lc         14.9     2.3   0.133   9.1  0.00029  0.019
##  3 Vesp… Calo… <NA>  Rode… <NA>        7      NA    NA      17   NA        0.045
##  4 Guin… Cavis herbi Rode… domest…     9.4     0.8   0.217  14.6  0.0055   0.728
##  5 Chin… Chin… herbi Rode… domest…    12.5     1.5   0.117  11.5  0.0064   0.42 
##  6 Star… Cond… omni  Sori… lc         10.3     2.2  NA      13.7  0.001    0.06 
##  7 Afri… Cric… omni  Rode… <NA>        8.3     2    NA      15.7  0.0066   1    
##  8 Less… Cryp… omni  Sori… lc          9.1     1.4   0.15   14.9  0.00014  0.005
##  9 Big … Epte… inse… Chir… lc         19.7     3.9   0.117   4.3  0.0003   0.023
## 10 Euro… Erin… omni  Erin… lc         10.1     3.5   0.283  13.9  0.0035   0.77 
## # … with 26 more rows, and abbreviated variable names ¹​conservation,
## #   ²​sleep_total, ³​sleep_rem, ⁴​sleep_cycle
```

Large Animals 

```r
large <- filter(msleep, bodywt >= 200)
large
```

```
## # A tibble: 7 × 11
##   name    genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake brainwt bodywt
##   <chr>   <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>
## 1 Cow     Bos   herbi Arti… domest…     4       0.7   0.667  20     0.423   600 
## 2 Asian … Elep… herbi Prob… en          3.9    NA    NA      20.1   4.60   2547 
## 3 Horse   Equus herbi Peri… domest…     2.9     0.6   1      21.1   0.655   521 
## 4 Giraffe Gira… herbi Arti… cd          1.9     0.4  NA      22.1  NA       900.
## 5 Pilot … Glob… carni Ceta… cd          2.7     0.1  NA      21.4  NA       800 
## 6 Africa… Loxo… herbi Prob… vu          3.3    NA    NA      20.7   5.71   6654 
## 7 Brazil… Tapi… herbi Peri… vu          4.4     1     0.9    19.6   0.169   208.
## # … with abbreviated variable names ¹​conservation, ²​sleep_total, ³​sleep_rem,
## #   ⁴​sleep_cycle
```


8. What is the mean weight for both the small and large mammals?

```r
small_weight_mean <- small$bodywt
mean(small_weight_mean)
```

```
## [1] 0.2596667
```


```r
large_weight_mean <- large$bodywt
mean(large_weight_mean)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average? 

Small animals sleep longer on average. The average total hours of sleep for small animals was a little more than 12 hours whereas the average for large animals was about 3 hours. 


```r
small_sleep_mean <- small$sleep_total
mean(small_sleep_mean)
```

```
## [1] 12.65833
```


```r
large_sleep_mean <- large$sleep_total
mean(large_sleep_mean)
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?

The little brwon bat is the sleepiest. 


```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```


```r
filter(sleep, sleep_total == 19.9)
```

```
## # A tibble: 1 × 11
##   name    genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake brainwt bodywt
##   <chr>   <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>
## 1 Little… Myot… inse… Chir… <NA>       19.9       2     0.2   4.1 0.00025   0.01
## # … with abbreviated variable names ¹​conservation, ²​sleep_total, ³​sleep_rem,
## #   ⁴​sleep_cycle
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
