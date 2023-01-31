---
title: "Midterm 1"
author: "Shefali Suresh"
date: "2023-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ecs21351-sup-0003-SupplementS1.csv`. These data are from Soykan, C. U., J. Sauer, J. G. Schuetz, G. S. LeBaron, K. Dale, and G. M. Langham. 2016. Population trends for North American winter birds based on hierarchical models. Ecosphere 7(5):e01351. 10.1002/ecs2.1351.  

Please load these data as a new object called `ecosphere`. In this step, I am providing the code to load the data, clean the variable names, and remove a footer that the authors used as part of the original publication.

```r
ecosphere <- read_csv("data/ecs21351-sup-0003-SupplementS1.csv", skip=2) %>% 
  clean_names() %>%
  slice(1:(n() - 18)) # this removes the footer
```

Problem 1. (1 point) Let's start with some data exploration. What are the variable names?

```r
names(ecosphere)
```

```
##  [1] "order"                       "family"                     
##  [3] "common_name"                 "scientific_name"            
##  [5] "diet"                        "life_expectancy"            
##  [7] "habitat"                     "urban_affiliate"            
##  [9] "migratory_strategy"          "log10_mass"                 
## [11] "mean_eggs_per_clutch"        "mean_age_at_sexual_maturity"
## [13] "population_size"             "winter_range_area"          
## [15] "range_in_cbc"                "strata"                     
## [17] "circles"                     "feeder_bird"                
## [19] "median_trend"                "lower_95_percent_ci"        
## [21] "upper_95_percent_ci"
```

Problem 2. (1 point) Use the function of your choice to summarize the data.

```r
glimpse(ecosphere)
```

```
## Rows: 551
## Columns: 21
## $ order                       <chr> "Anseriformes", "Anseriformes", "Anserifor…
## $ family                      <chr> "Anatidae", "Anatidae", "Anatidae", "Anati…
## $ common_name                 <chr> "American Black Duck", "American Wigeon", …
## $ scientific_name             <chr> "Anas rubripes", "Anas americana", "Buceph…
## $ diet                        <chr> "Vegetation", "Vegetation", "Invertebrates…
## $ life_expectancy             <chr> "Long", "Middle", "Middle", "Long", "Middl…
## $ habitat                     <chr> "Wetland", "Wetland", "Wetland", "Wetland"…
## $ urban_affiliate             <chr> "No", "No", "No", "No", "No", "No", "No", …
## $ migratory_strategy          <chr> "Short", "Short", "Moderate", "Moderate", …
## $ log10_mass                  <dbl> 3.09, 2.88, 2.96, 3.11, 3.02, 2.88, 2.56, …
## $ mean_eggs_per_clutch        <dbl> 9.0, 7.5, 10.5, 3.5, 9.5, 13.5, 10.0, 8.5,…
## $ mean_age_at_sexual_maturity <dbl> 1.0, 1.0, 3.0, 2.5, 2.0, 1.0, 0.6, 2.0, 1.…
## $ population_size             <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ winter_range_area           <dbl> 3212473, 7145842, 1812841, 360134, 854350,…
## $ range_in_cbc                <dbl> 99.1, 61.7, 69.8, 53.7, 5.3, 0.5, 17.9, 72…
## $ strata                      <dbl> 82, 124, 37, 19, 36, 5, 26, 134, 145, 103,…
## $ circles                     <dbl> 1453, 1951, 502, 247, 470, 97, 479, 2189, …
## $ feeder_bird                 <chr> "No", "No", "No", "No", "No", "No", "No", …
## $ median_trend                <dbl> 1.014, 0.996, 1.039, 0.998, 1.004, 1.196, …
## $ lower_95_percent_ci         <dbl> 0.971, 0.964, 1.016, 0.956, 0.975, 1.152, …
## $ upper_95_percent_ci         <dbl> 1.055, 1.009, 1.104, 1.041, 1.036, 1.243, …
```

Problem 3. (2 points) How many distinct orders of birds are represented in the data?

```r
ecosphere %>%  
  summarize(distinct_order_of_birds = n_distinct(order))
```

```
## # A tibble: 1 × 1
##   distinct_order_of_birds
##                     <int>
## 1                      19
```

Problem 4. (2 points) Which habitat has the highest diversity (number of species) in the data?

```r
ecosphere %>%
  group_by(habitat) %>% #looking at habitat
  summarize(high_diversity = n_distinct(scientific_name)) 
```

```
## # A tibble: 7 × 2
##   habitat   high_diversity
##   <chr>              <int>
## 1 Grassland             36
## 2 Ocean                 44
## 3 Shrubland             82
## 4 Various               45
## 5 Wetland              153
## 6 Woodland             177
## 7 <NA>                  14
```

Run the code below to learn about the `slice` function. Look specifically at the examples (at the bottom) for `slice_max()` and `slice_min()`. If you are still unsure, try looking up examples online (https://rpubs.com/techanswers88/dplyr-slice). Use this new function to answer question 5 below.

```r
?slice_max
```

Problem 5. (4 points) Using the `slice_max()` or `slice_min()` function described above which species has the largest and smallest winter range?

```r
ecosphere %>% slice_max(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Procella… Proce… Sooty … Puffin… Vert… Long    Ocean   No      Long        2.9
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```


```r
ecosphere %>% slice_min(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Passerif… Alaud… Skylark Alauda… Seed  Short   Grassl… No      Reside…    1.57
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 6. (2 points) The family Anatidae includes ducks, geese, and swans. Make a new object `ducks` that only includes species in the family Anatidae. Restrict this new dataframe to include all variables except order and family.

```r
ducks <- filter(ecosphere, family == "Anatidae")
ducks
```

```
## # A tibble: 44 × 21
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Anserif… Anati… "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09
##  2 Anserif… Anati… "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88
##  3 Anserif… Anati… "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96
##  4 Anserif… Anati… "Black… Branta… Vege… Long    Wetland No      Modera…    3.11
##  5 Anserif… Anati… "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02
##  6 Anserif… Anati… "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88
##  7 Anserif… Anati… "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56
##  8 Anserif… Anati… "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6 
##  9 Anserif… Anati… "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45
## 10 Anserif… Anati… "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08
## # … with 34 more rows, 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

```r
select(ducks, -order, -family)
```

```
## # A tibble: 44 × 19
##    commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷ mean_…⁸
##    <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>
##  1 "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09     9       1  
##  2 "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88     7.5     1  
##  3 "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96    10.5     3  
##  4 "Black… Branta… Vege… Long    Wetland No      Modera…    3.11     3.5     2.5
##  5 "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02     9.5     2  
##  6 "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88    13.5     1  
##  7 "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56    10       0.6
##  8 "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6      8.5     2  
##  9 "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45     5       1  
## 10 "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08     8       1  
## # … with 34 more rows, 9 more variables: population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass, ⁷​mean_eggs_per_clutch, ⁸​mean_age_at_sexual_maturity
```


Problem 7. (2 points) We might assume that all ducks live in wetland habitat. Is this true for the ducks in these data? If there are exceptions, list the species below.

```r
filter(ducks, habitat != "Wetland")
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Anserifo… Anati… Common… Somate… Inve… Middle  Ocean   No      Short      3.31
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 8. (4 points) In ducks, how is mean body mass associated with migratory strategy? Do the ducks that migrate long distances have high or low average body mass?

Ducks that migrate long distances have a low average body mass. The shorter the distance of migration, the heavier the mean body mass of the ducks are. 

```r
ducks %>% 
  group_by(migratory_strategy)%>%
  summarise(mean_body_mass=mean(log10_mass))
```

```
## # A tibble: 5 × 2
##   migratory_strategy mean_body_mass
##   <chr>                       <dbl>
## 1 Long                         2.87
## 2 Moderate                     3.11
## 3 Resident                     4.03
## 4 Short                        2.98
## 5 Withdrawal                   2.92
```

Problem 9. (2 points) Accipitridae is the family that includes eagles, hawks, kites, and osprey. First, make a new object `eagles` that only includes species in the family Accipitridae. Next, restrict these data to only include the variables common_name, scientific_name, and population_size.

```r
eagles <- filter(ecosphere, family == "Accipitridae")
eagles
```

```
## # A tibble: 20 × 21
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Falconi… Accip… Bald E… Haliae… Vert… Long    Wetland No      Short      3.67
##  2 Falconi… Accip… Broad-… Buteo … Vert… Middle  Woodla… No      Long       2.66
##  3 Falconi… Accip… Cooper… Accipi… Vert… Middle  Woodla… Yes     Short      2.63
##  4 Falconi… Accip… Ferrug… Buteo … Vert… Middle  Grassl… No      Short      3.17
##  5 Falconi… Accip… Golden… Aquila… Vert… Long    Various No      Short      3.63
##  6 Falconi… Accip… Gray H… Buteo … Vert… Middle  Woodla… No      Withdr…    2.63
##  7 Falconi… Accip… Harris… Parabu… Vert… Middle  Shrubl… Yes     Reside…    2.93
##  8 Falconi… Accip… Hook-b… Chondr… Inve… Middle  Woodla… No      Reside…    2.49
##  9 Falconi… Accip… Northe… Accipi… Vert… Middle  Woodla… No      Withdr…    2.94
## 10 Falconi… Accip… Northe… Circus… Vert… Middle  Various No      Short      2.59
## 11 Falconi… Accip… Red-sh… Buteo … Inve… Middle  Woodla… No      Short      2.78
## 12 Falconi… Accip… Red-ta… Buteo … Vert… Long    Woodla… Yes     Short      3.04
## 13 Falconi… Accip… Rough-… Buteo … Vert… Middle  Grassl… No      Modera…    2.98
## 14 Falconi… Accip… Sharp-… Accipi… Vert… Middle  Woodla… Yes     Short      2.12
## 15 Falconi… Accip… Short-… Buteo … Vert… Middle  Woodla… No      Withdr…    2.71
## 16 Falconi… Accip… Snail … Rostrh… Inve… Middle  Wetland No      Reside…    2.57
## 17 Falconi… Accip… Swains… Buteo … Vert… Middle  Various No      Long       2.98
## 18 Falconi… Accip… White-… Buteo … Vert… Middle  Grassl… No      Reside…    3.01
## 19 Falconi… Accip… White-… Elanus… Vert… Short   Various No      Withdr…    2.54
## 20 Falconi… Accip… Zone-t… Buteo … Vert… Middle  Woodla… No      Short      2.88
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

```r
select(eagles, common_name,scientific_name,population_size)
```

```
## # A tibble: 20 × 3
##    common_name         scientific_name          population_size
##    <chr>               <chr>                              <dbl>
##  1 Bald Eagle          Haliaeetus leucocephalus              NA
##  2 Broad-winged Hawk   Buteo platypterus                1700000
##  3 Cooper's Hawk       Accipiter cooperii                700000
##  4 Ferruginous Hawk    Buteo regalis                      80000
##  5 Golden Eagle        Aquila chrysaetos                 130000
##  6 Gray Hawk           Buteo nitidus                         NA
##  7 Harris's Hawk       Parabuteo unicinctus               50000
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA
##  9 Northern Goshawk    Accipiter gentilis                200000
## 10 Northern Harrier    Circus cyaneus                    700000
## 11 Red-shouldered Hawk Buteo lineatus                   1100000
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000
## 13 Rough-legged Hawk   Buteo lagopus                     300000
## 14 Sharp-shinned Hawk  Accipiter striatus                500000
## 15 Short-tailed Hawk   Buteo brachyurus                      NA
## 16 Snail Kite          Rostrhamus sociabilis                 NA
## 17 Swainson's Hawk     Buteo swainsoni                   540000
## 18 White-tailed Hawk   Buteo albicaudatus                    NA
## 19 White-tailed Kite   Elanus leucurus                       NA
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA
```


Problem 10. (4 points) In the eagles data, any species with a population size less than 250,000 individuals is threatened. Make a new column `conservation_status` that shows whether or not a species is threatened.



```r
eagles <- eagles %>% 
  mutate(conservation_status = ifelse(population_size < 250000, "threatened", "not threatened"))
eagles
```

```
## # A tibble: 20 × 22
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Falconi… Accip… Bald E… Haliae… Vert… Long    Wetland No      Short      3.67
##  2 Falconi… Accip… Broad-… Buteo … Vert… Middle  Woodla… No      Long       2.66
##  3 Falconi… Accip… Cooper… Accipi… Vert… Middle  Woodla… Yes     Short      2.63
##  4 Falconi… Accip… Ferrug… Buteo … Vert… Middle  Grassl… No      Short      3.17
##  5 Falconi… Accip… Golden… Aquila… Vert… Long    Various No      Short      3.63
##  6 Falconi… Accip… Gray H… Buteo … Vert… Middle  Woodla… No      Withdr…    2.63
##  7 Falconi… Accip… Harris… Parabu… Vert… Middle  Shrubl… Yes     Reside…    2.93
##  8 Falconi… Accip… Hook-b… Chondr… Inve… Middle  Woodla… No      Reside…    2.49
##  9 Falconi… Accip… Northe… Accipi… Vert… Middle  Woodla… No      Withdr…    2.94
## 10 Falconi… Accip… Northe… Circus… Vert… Middle  Various No      Short      2.59
## 11 Falconi… Accip… Red-sh… Buteo … Inve… Middle  Woodla… No      Short      2.78
## 12 Falconi… Accip… Red-ta… Buteo … Vert… Long    Woodla… Yes     Short      3.04
## 13 Falconi… Accip… Rough-… Buteo … Vert… Middle  Grassl… No      Modera…    2.98
## 14 Falconi… Accip… Sharp-… Accipi… Vert… Middle  Woodla… Yes     Short      2.12
## 15 Falconi… Accip… Short-… Buteo … Vert… Middle  Woodla… No      Withdr…    2.71
## 16 Falconi… Accip… Snail … Rostrh… Inve… Middle  Wetland No      Reside…    2.57
## 17 Falconi… Accip… Swains… Buteo … Vert… Middle  Various No      Long       2.98
## 18 Falconi… Accip… White-… Buteo … Vert… Middle  Grassl… No      Reside…    3.01
## 19 Falconi… Accip… White-… Elanus… Vert… Short   Various No      Withdr…    2.54
## 20 Falconi… Accip… Zone-t… Buteo … Vert… Middle  Woodla… No      Short      2.88
## # … with 12 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, conservation_status <chr>, and abbreviated
## #   variable names ¹​common_name, ²​scientific_name, ³​life_expectancy,
## #   ⁴​urban_affiliate, ⁵​migratory_strategy, ⁶​log10_mass
```

```r
#ifelse(test, yes, no)
```

Problem 11. (2 points) Consider the results from questions 9 and 10. Are there any species for which their threatened status needs further study? How do you know?

```r
anyNA(eagles$conservation_status)
```

```
## [1] TRUE
```


```r
is.na(eagles)
```

```
##       order family common_name scientific_name  diet life_expectancy habitat
##  [1,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [2,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [3,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [4,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [5,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [6,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [7,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [8,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##  [9,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [10,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [11,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [12,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [13,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [14,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [15,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [16,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [17,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [18,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [19,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
## [20,] FALSE  FALSE       FALSE           FALSE FALSE           FALSE   FALSE
##       urban_affiliate migratory_strategy log10_mass mean_eggs_per_clutch
##  [1,]           FALSE              FALSE      FALSE                FALSE
##  [2,]           FALSE              FALSE      FALSE                FALSE
##  [3,]           FALSE              FALSE      FALSE                FALSE
##  [4,]           FALSE              FALSE      FALSE                FALSE
##  [5,]           FALSE              FALSE      FALSE                FALSE
##  [6,]           FALSE              FALSE      FALSE                FALSE
##  [7,]           FALSE              FALSE      FALSE                FALSE
##  [8,]           FALSE              FALSE      FALSE                FALSE
##  [9,]           FALSE              FALSE      FALSE                FALSE
## [10,]           FALSE              FALSE      FALSE                FALSE
## [11,]           FALSE              FALSE      FALSE                FALSE
## [12,]           FALSE              FALSE      FALSE                FALSE
## [13,]           FALSE              FALSE      FALSE                FALSE
## [14,]           FALSE              FALSE      FALSE                FALSE
## [15,]           FALSE              FALSE      FALSE                FALSE
## [16,]           FALSE              FALSE      FALSE                FALSE
## [17,]           FALSE              FALSE      FALSE                FALSE
## [18,]           FALSE              FALSE      FALSE                FALSE
## [19,]           FALSE              FALSE      FALSE                FALSE
## [20,]           FALSE              FALSE      FALSE                FALSE
##       mean_age_at_sexual_maturity population_size winter_range_area
##  [1,]                       FALSE            TRUE             FALSE
##  [2,]                       FALSE           FALSE             FALSE
##  [3,]                       FALSE           FALSE             FALSE
##  [4,]                       FALSE           FALSE             FALSE
##  [5,]                       FALSE           FALSE             FALSE
##  [6,]                       FALSE            TRUE             FALSE
##  [7,]                       FALSE           FALSE             FALSE
##  [8,]                       FALSE            TRUE             FALSE
##  [9,]                       FALSE           FALSE             FALSE
## [10,]                       FALSE           FALSE             FALSE
## [11,]                       FALSE           FALSE             FALSE
## [12,]                       FALSE           FALSE             FALSE
## [13,]                       FALSE           FALSE             FALSE
## [14,]                       FALSE           FALSE             FALSE
## [15,]                       FALSE            TRUE             FALSE
## [16,]                       FALSE            TRUE             FALSE
## [17,]                       FALSE           FALSE             FALSE
## [18,]                       FALSE            TRUE             FALSE
## [19,]                       FALSE            TRUE             FALSE
## [20,]                       FALSE            TRUE             FALSE
##       range_in_cbc strata circles feeder_bird median_trend lower_95_percent_ci
##  [1,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [2,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [3,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [4,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [5,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [6,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [7,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [8,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##  [9,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [10,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [11,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [12,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [13,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [14,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [15,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [16,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [17,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [18,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [19,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
## [20,]        FALSE  FALSE   FALSE       FALSE        FALSE               FALSE
##       upper_95_percent_ci conservation_status
##  [1,]               FALSE                TRUE
##  [2,]               FALSE               FALSE
##  [3,]               FALSE               FALSE
##  [4,]               FALSE               FALSE
##  [5,]               FALSE               FALSE
##  [6,]               FALSE                TRUE
##  [7,]               FALSE               FALSE
##  [8,]               FALSE                TRUE
##  [9,]               FALSE               FALSE
## [10,]               FALSE               FALSE
## [11,]               FALSE               FALSE
## [12,]               FALSE               FALSE
## [13,]               FALSE               FALSE
## [14,]               FALSE               FALSE
## [15,]               FALSE                TRUE
## [16,]               FALSE                TRUE
## [17,]               FALSE               FALSE
## [18,]               FALSE                TRUE
## [19,]               FALSE                TRUE
## [20,]               FALSE                TRUE
```

Yes, there are species that their threatened status needs further study because if there are NAs present in the data it might mean that the researchers weren't able to collect enough data. 

Problem 12. (4 points) Use the `ecosphere` data to perform one exploratory analysis of your choice. The analysis must have a minimum of three lines and two functions. You must also clearly state the question you are attempting to answer.

```r
#Which animal (common name) has the lowest mass? 
ecosphere %>% 
  select(common_name, log10_mass) %>% 
  arrange(desc(log10_mass))
```

```
## # A tibble: 551 × 2
##    common_name            log10_mass
##    <chr>                       <dbl>
##  1 Trumpeter Swan               4.04
##  2 Mute Swan                    4.03
##  3 California Condor            3.93
##  4 Tundra Swan                  3.81
##  5 Whooping Crane               3.77
##  6 Wild Turkey                  3.76
##  7 American White Pelican       3.75
##  8 Common Loon                  3.7 
##  9 Yellow-billed Loon           3.69
## 10 Bald Eagle                   3.67
## # … with 541 more rows
```
Please provide the names of the students you have worked with with during the exam:

```r
#Ingird Jimenez Ledesma 
#Rafael Vasquez-Chan
```

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
