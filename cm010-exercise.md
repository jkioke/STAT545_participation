cm008 Exercises
================

Install `nycflights13` package
------------------------------

``` r
install.packages("nycflights13")
```

``` r
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(nycflights13))
```

Types of mutating join
----------------------

### Let's join tibbles using four mutating functions: `left_join`, `right_join`, `inner_join` and `full_join`.

### create two tibbles named `a` and `b`

``` r
(a <- tibble(x1 = LETTERS[1:3], x2 = 1:3))
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1
    ## 2 B         2
    ## 3 C         3

``` r
(b <- tibble(x1 = LETTERS[c(1,2,4)], x3 = c("T", "F", "T")))
```

    ## # A tibble: 3 x 2
    ##   x1    x3   
    ##   <chr> <chr>
    ## 1 A     T    
    ## 2 B     F    
    ## 3 D     T

### left\_join: Join matching rows from `b` to `a` by matching "x1" variable

``` r
left_join(a, b, "x1")
```

    ## # A tibble: 3 x 3
    ##   x1       x2 x3   
    ##   <chr> <int> <chr>
    ## 1 A         1 T    
    ## 2 B         2 F    
    ## 3 C         3 <NA>

### right\_join: Join matching rows from `a` to `b` by matching "x1" variable.

``` r
right_join(a, b, "x1")
```

    ## # A tibble: 3 x 3
    ##   x1       x2 x3   
    ##   <chr> <int> <chr>
    ## 1 A         1 T    
    ## 2 B         2 F    
    ## 3 D        NA T

### inner\_join: Join data. Retain only rows in both sets `a` to `b` by matching "x1" variable.

``` r
inner_join(a,b,"x1")
```

    ## # A tibble: 2 x 3
    ##   x1       x2 x3   
    ##   <chr> <int> <chr>
    ## 1 A         1 T    
    ## 2 B         2 F

### full\_join: Join data. Retain all values, all rows of `a` to `b` by matching "x1"

``` r
full_join(a,b)
```

    ## Joining, by = "x1"

    ## # A tibble: 4 x 3
    ##   x1       x2 x3   
    ##   <chr> <int> <chr>
    ## 1 A         1 T    
    ## 2 B         2 F    
    ## 3 C         3 <NA> 
    ## 4 D        NA T

### what happen if we do not specify `by` option?

``` r
left_join(a, b)
```

    ## Joining, by = "x1"

    ## # A tibble: 3 x 3
    ##   x1       x2 x3   
    ##   <chr> <int> <chr>
    ## 1 A         1 T    
    ## 2 B         2 F    
    ## 3 C         3 <NA>

### what happen if we join two different variables (e.g., "x1" to "x3") from two tibbles `a` to `b`?

``` r
left_join(a, b, by = c("x1" = "x3"))
```

    ## # A tibble: 3 x 3
    ##   x1       x2 x1.y 
    ##   <chr> <int> <chr>
    ## 1 A         1 <NA> 
    ## 2 B         2 <NA> 
    ## 3 C         3 <NA>

### what happen if two columns of `a` and `c` datasets have the identical colnames?

``` r
# make data frame c and use inner_join()
(c <- tibble(x1 = c(LETTERS[1:2],"x"), x2 = c(1,4,5)))
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <dbl>
    ## 1 A        1.
    ## 2 B        4.
    ## 3 x        5.

``` r
c
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <dbl>
    ## 1 A        1.
    ## 2 B        4.
    ## 3 x        5.

``` r
inner_join(a, c)
```

    ## Joining, by = c("x1", "x2")

    ## # A tibble: 1 x 2
    ##   x1       x2
    ##   <chr> <dbl>
    ## 1 A        1.

In class practice
-----------------

`nycflights13` dataset has several tibbles e.g., `flights`, `airports`, `planes`, `weather`.

### 1. Explore `nycflights13` dataset

``` r
#check the tibbles included in `nycflights13` package
class(flights)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
colnames(flights)
```

    ##  [1] "year"           "month"          "day"            "dep_time"      
    ##  [5] "sched_dep_time" "dep_delay"      "arr_time"       "sched_arr_time"
    ##  [9] "arr_delay"      "carrier"        "flight"         "tailnum"       
    ## [13] "origin"         "dest"           "air_time"       "distance"      
    ## [17] "hour"           "minute"         "time_hour"

``` r
colnames(airlines)
```

    ## [1] "carrier" "name"

``` r
colnames(weather)
```

    ##  [1] "origin"     "year"       "month"      "day"        "hour"      
    ##  [6] "temp"       "dewp"       "humid"      "wind_dir"   "wind_speed"
    ## [11] "wind_gust"  "precip"     "pressure"   "visib"      "time_hour"

### 2. Drop unimportant variables so it's easier to understand the join results. Also take first 1000 rows to run it faster.

``` r
flights2 <- flights[1:1000,] %>% 
  select(year, tailnum, carrier, time_hour)
flights2
```

    ## # A tibble: 1,000 x 4
    ##     year tailnum carrier time_hour          
    ##    <int> <chr>   <chr>   <dttm>             
    ##  1  2013 N14228  UA      2013-01-01 05:00:00
    ##  2  2013 N24211  UA      2013-01-01 05:00:00
    ##  3  2013 N619AA  AA      2013-01-01 05:00:00
    ##  4  2013 N804JB  B6      2013-01-01 05:00:00
    ##  5  2013 N668DN  DL      2013-01-01 06:00:00
    ##  6  2013 N39463  UA      2013-01-01 05:00:00
    ##  7  2013 N516JB  B6      2013-01-01 06:00:00
    ##  8  2013 N829AS  EV      2013-01-01 06:00:00
    ##  9  2013 N593JB  B6      2013-01-01 06:00:00
    ## 10  2013 N3ALAA  AA      2013-01-01 06:00:00
    ## # ... with 990 more rows

### 3. Add airline names to `flights2` from `airlines` dataset.

``` r
# Which join function to use?
summary(airlines)
```

    ##    carrier              name          
    ##  Length:16          Length:16         
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character

``` r
airlines
```

    ## # A tibble: 16 x 2
    ##    carrier name                       
    ##    <chr>   <chr>                      
    ##  1 9E      Endeavor Air Inc.          
    ##  2 AA      American Airlines Inc.     
    ##  3 AS      Alaska Airlines Inc.       
    ##  4 B6      JetBlue Airways            
    ##  5 DL      Delta Air Lines Inc.       
    ##  6 EV      ExpressJet Airlines Inc.   
    ##  7 F9      Frontier Airlines Inc.     
    ##  8 FL      AirTran Airways Corporation
    ##  9 HA      Hawaiian Airlines Inc.     
    ## 10 MQ      Envoy Air                  
    ## 11 OO      SkyWest Airlines Inc.      
    ## 12 UA      United Air Lines Inc.      
    ## 13 US      US Airways Inc.            
    ## 14 VX      Virgin America             
    ## 15 WN      Southwest Airlines Co.     
    ## 16 YV      Mesa Airlines Inc.

``` r
left_join(flights2, airlines, by = "carrier")
```

    ## # A tibble: 1,000 x 5
    ##     year tailnum carrier time_hour           name                    
    ##    <int> <chr>   <chr>   <dttm>              <chr>                   
    ##  1  2013 N14228  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  2  2013 N24211  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  3  2013 N619AA  AA      2013-01-01 05:00:00 American Airlines Inc.  
    ##  4  2013 N804JB  B6      2013-01-01 05:00:00 JetBlue Airways         
    ##  5  2013 N668DN  DL      2013-01-01 06:00:00 Delta Air Lines Inc.    
    ##  6  2013 N39463  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  7  2013 N516JB  B6      2013-01-01 06:00:00 JetBlue Airways         
    ##  8  2013 N829AS  EV      2013-01-01 06:00:00 ExpressJet Airlines Inc.
    ##  9  2013 N593JB  B6      2013-01-01 06:00:00 JetBlue Airways         
    ## 10  2013 N3ALAA  AA      2013-01-01 06:00:00 American Airlines Inc.  
    ## # ... with 990 more rows

### 4. Add `weather` information to the `flights2` dataset by matching "year" and "time\_hour" variables.

``` r
summary(weather)
```

    ##     origin               year          month             day       
    ##  Length:26115       Min.   :2013   Min.   : 1.000   Min.   : 1.00  
    ##  Class :character   1st Qu.:2013   1st Qu.: 4.000   1st Qu.: 8.00  
    ##  Mode  :character   Median :2013   Median : 7.000   Median :16.00  
    ##                     Mean   :2013   Mean   : 6.504   Mean   :15.68  
    ##                     3rd Qu.:2013   3rd Qu.: 9.000   3rd Qu.:23.00  
    ##                     Max.   :2013   Max.   :12.000   Max.   :31.00  
    ##                                                                    
    ##       hour            temp             dewp           humid       
    ##  Min.   : 0.00   Min.   : 10.94   Min.   :-9.94   Min.   : 12.74  
    ##  1st Qu.: 6.00   1st Qu.: 39.92   1st Qu.:26.06   1st Qu.: 47.05  
    ##  Median :11.00   Median : 55.40   Median :42.08   Median : 61.79  
    ##  Mean   :11.49   Mean   : 55.26   Mean   :41.44   Mean   : 62.53  
    ##  3rd Qu.:17.00   3rd Qu.: 69.98   3rd Qu.:57.92   3rd Qu.: 78.79  
    ##  Max.   :23.00   Max.   :100.04   Max.   :78.08   Max.   :100.00  
    ##                  NA's   :1        NA's   :1       NA's   :1       
    ##     wind_dir       wind_speed         wind_gust         precip        
    ##  Min.   :  0.0   Min.   :   0.000   Min.   :16.11   Min.   :0.000000  
    ##  1st Qu.:120.0   1st Qu.:   6.905   1st Qu.:20.71   1st Qu.:0.000000  
    ##  Median :220.0   Median :  10.357   Median :24.17   Median :0.000000  
    ##  Mean   :199.8   Mean   :  10.518   Mean   :25.49   Mean   :0.004469  
    ##  3rd Qu.:290.0   3rd Qu.:  13.809   3rd Qu.:28.77   3rd Qu.:0.000000  
    ##  Max.   :360.0   Max.   :1048.361   Max.   :66.75   Max.   :1.210000  
    ##  NA's   :460     NA's   :4          NA's   :20778                     
    ##     pressure          visib          time_hour                  
    ##  Min.   : 983.8   Min.   : 0.000   Min.   :2013-01-01 01:00:00  
    ##  1st Qu.:1012.9   1st Qu.:10.000   1st Qu.:2013-04-01 21:30:00  
    ##  Median :1017.6   Median :10.000   Median :2013-07-01 14:00:00  
    ##  Mean   :1017.9   Mean   : 9.255   Mean   :2013-07-01 18:26:37  
    ##  3rd Qu.:1023.0   3rd Qu.:10.000   3rd Qu.:2013-09-30 13:00:00  
    ##  Max.   :1042.1   Max.   :10.000   Max.   :2013-12-30 18:00:00  
    ##  NA's   :2729

``` r
flights2 %>% 
  left_join(weather, by = c("year", "time_hour"))
```

    ## # A tibble: 2,888 x 17
    ##     year tailnum carrier time_hour           origin month   day  hour
    ##    <dbl> <chr>   <chr>   <dttm>              <chr>  <dbl> <int> <int>
    ##  1 2013. N14228  UA      2013-01-01 05:00:00 EWR       1.     1     5
    ##  2 2013. N14228  UA      2013-01-01 05:00:00 JFK       1.     1     5
    ##  3 2013. N14228  UA      2013-01-01 05:00:00 LGA       1.     1     5
    ##  4 2013. N24211  UA      2013-01-01 05:00:00 EWR       1.     1     5
    ##  5 2013. N24211  UA      2013-01-01 05:00:00 JFK       1.     1     5
    ##  6 2013. N24211  UA      2013-01-01 05:00:00 LGA       1.     1     5
    ##  7 2013. N619AA  AA      2013-01-01 05:00:00 EWR       1.     1     5
    ##  8 2013. N619AA  AA      2013-01-01 05:00:00 JFK       1.     1     5
    ##  9 2013. N619AA  AA      2013-01-01 05:00:00 LGA       1.     1     5
    ## 10 2013. N804JB  B6      2013-01-01 05:00:00 EWR       1.     1     5
    ## # ... with 2,878 more rows, and 9 more variables: temp <dbl>, dewp <dbl>,
    ## #   humid <dbl>, wind_dir <dbl>, wind_speed <dbl>, wind_gust <dbl>,
    ## #   precip <dbl>, pressure <dbl>, visib <dbl>

### 5. Add `weather` information to the `flights2` dataset by matching only "time\_hour" variable

``` r
flights2 %>% 
  left_join(weather, by = "time_hour")
```

    ## # A tibble: 2,888 x 18
    ##    year.x tailnum carrier time_hour           origin year.y month   day
    ##     <int> <chr>   <chr>   <dttm>              <chr>   <dbl> <dbl> <int>
    ##  1   2013 N14228  UA      2013-01-01 05:00:00 EWR     2013.    1.     1
    ##  2   2013 N14228  UA      2013-01-01 05:00:00 JFK     2013.    1.     1
    ##  3   2013 N14228  UA      2013-01-01 05:00:00 LGA     2013.    1.     1
    ##  4   2013 N24211  UA      2013-01-01 05:00:00 EWR     2013.    1.     1
    ##  5   2013 N24211  UA      2013-01-01 05:00:00 JFK     2013.    1.     1
    ##  6   2013 N24211  UA      2013-01-01 05:00:00 LGA     2013.    1.     1
    ##  7   2013 N619AA  AA      2013-01-01 05:00:00 EWR     2013.    1.     1
    ##  8   2013 N619AA  AA      2013-01-01 05:00:00 JFK     2013.    1.     1
    ##  9   2013 N619AA  AA      2013-01-01 05:00:00 LGA     2013.    1.     1
    ## 10   2013 N804JB  B6      2013-01-01 05:00:00 EWR     2013.    1.     1
    ## # ... with 2,878 more rows, and 10 more variables: hour <int>, temp <dbl>,
    ## #   dewp <dbl>, humid <dbl>, wind_dir <dbl>, wind_speed <dbl>,
    ## #   wind_gust <dbl>, precip <dbl>, pressure <dbl>, visib <dbl>

Types of filtering join
-----------------------

### Let's filter tibbles using two filtering functions: `semi_join`, `anti_join`

### example for `semi_join`: All rows in `a` that have a match in `b`

``` r
semi_join(a, b, by = "x1")
```

    ## # A tibble: 2 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1
    ## 2 B         2

``` r
a
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1
    ## 2 B         2
    ## 3 C         3

``` r
b
```

    ## # A tibble: 3 x 2
    ##   x1    x3   
    ##   <chr> <chr>
    ## 1 A     T    
    ## 2 B     F    
    ## 3 D     T

### example for `anti_join`: All rows in `a` that do not have a match in `b`

``` r
anti_join(a, b, "x1")
```

    ## # A tibble: 1 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 C         3

``` r
anti_join(b, a, "x1")
```

    ## # A tibble: 1 x 2
    ##   x1    x3   
    ##   <chr> <chr>
    ## 1 D     T

### example of joinin by matching two variables (e.g., "x1", "x2") from both datasets `a` and `c`

``` r
semi_join(a,c, by = c("x1", "x2"))
```

    ## # A tibble: 1 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1

Types of Set Operations for two datasets
----------------------------------------

### Let's use three `set` functions: `intersect`, `union` and `setdiff`

### create two tibbles named `y` and `z`, similar to Data Wrangling Cheatsheet

``` r
(y <-  tibble(x1 = LETTERS[1:3], x2 = 1:3))
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1
    ## 2 B         2
    ## 3 C         3

``` r
(z <- tibble(x1 = c("B", "C", "D"), x2 = 2:4))
```

    ## # A tibble: 3 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 B         2
    ## 2 C         3
    ## 3 D         4

### example for `intersect`: Rows that appear in both `y` and `z`

``` r
intersect(y, z, by = c("x1", "x2"))
```

    ## # A tibble: 2 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 B         2
    ## 2 C         3

``` r
#or
intersect(y,z)
```

    ## # A tibble: 2 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 B         2
    ## 2 C         3

### example for `union`: Rows that appear in either or both `y` and `z`

``` r
union(y, z)
```

    ## # A tibble: 4 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 D         4
    ## 2 C         3
    ## 3 B         2
    ## 4 A         1

### example for `setdiff`: Rows that appear in `y` but not `z`. **Caution:** `setdiff` for `y` to `z` and `z` to `y` are different.

``` r
setdiff(y, z)
```

    ## # A tibble: 1 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1

``` r
setdiff(z, y)
```

    ## # A tibble: 1 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 D         4

### what happen if colnames are differentin `y` and `x`? Is there any error message and why?

``` r
(x <- tibble(x1 = c("B", "C", "D"), x3 = 2:4))
```

    ## # A tibble: 3 x 2
    ##   x1       x3
    ##   <chr> <int>
    ## 1 B         2
    ## 2 C         3
    ## 3 D         4

``` r
#setdiff(y, x)
#intersect(y,x)
```

Types of binding datasets
-------------------------

### Let's bind datasets by rows or column using two binding functions:

### example for `bind_rows`: Append `z` to `y` as new rows

``` r
bind_rows(y,z)
```

    ## # A tibble: 6 x 2
    ##   x1       x2
    ##   <chr> <int>
    ## 1 A         1
    ## 2 B         2
    ## 3 C         3
    ## 4 B         2
    ## 5 C         3
    ## 6 D         4

### example for `bind_cols`: Append `z` to `y` as new columns. **Caution**: matches rows by position. Check colnames after binding.

``` r
bind_cols(y,z)
```

    ## # A tibble: 3 x 4
    ##   x1       x2 x11     x21
    ##   <chr> <int> <chr> <int>
    ## 1 A         1 B         2
    ## 2 B         2 C         3
    ## 3 C         3 D         4

### what happen if colnames are different between `y` and `x` datasets?

``` r
bind_rows(y,x)
```

    ## # A tibble: 6 x 3
    ##   x1       x2    x3
    ##   <chr> <int> <int>
    ## 1 A         1    NA
    ## 2 B         2    NA
    ## 3 C         3    NA
    ## 4 B        NA     2
    ## 5 C        NA     3
    ## 6 D        NA     4

``` r
bind_cols(y,x)
```

    ## # A tibble: 3 x 4
    ##   x1       x2 x11      x3
    ##   <chr> <int> <chr> <int>
    ## 1 A         1 B         2
    ## 2 B         2 C         3
    ## 3 C         3 D         4

Practice Exercises
------------------

Practice these concepts in the following exercises. It might help you to first identify the type of function you are applying.

### 1. Filter the rows of `flights2` by matching "year" and "time\_hour" variables to `weather` dataset. Use both `semi_join()` and `anti_join()`

``` r
semi_join(flights2, weather, by = c("year", "time_hour"))
```

    ## # A tibble: 1,000 x 4
    ##     year tailnum carrier time_hour          
    ##    <int> <chr>   <chr>   <dttm>             
    ##  1  2013 N14228  UA      2013-01-01 05:00:00
    ##  2  2013 N24211  UA      2013-01-01 05:00:00
    ##  3  2013 N619AA  AA      2013-01-01 05:00:00
    ##  4  2013 N804JB  B6      2013-01-01 05:00:00
    ##  5  2013 N668DN  DL      2013-01-01 06:00:00
    ##  6  2013 N39463  UA      2013-01-01 05:00:00
    ##  7  2013 N516JB  B6      2013-01-01 06:00:00
    ##  8  2013 N829AS  EV      2013-01-01 06:00:00
    ##  9  2013 N593JB  B6      2013-01-01 06:00:00
    ## 10  2013 N3ALAA  AA      2013-01-01 06:00:00
    ## # ... with 990 more rows

``` r
anti_join(flights2, weather, by = c("year", "time_hour"))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: year <int>, tailnum <chr>, carrier <chr>,
    ## #   time_hour <dttm>

### 2. Can we apply `set` and `binding` funcions between `flights2` and `weather` datasets. Why and why not?

``` r
bind_rows(flights2, weather)
```

    ## # A tibble: 27,115 x 17
    ##     year tailnum carrier time_hour           origin month   day  hour
    ##    <dbl> <chr>   <chr>   <dttm>              <chr>  <dbl> <int> <int>
    ##  1 2013. N14228  UA      2013-01-01 05:00:00 <NA>      NA    NA    NA
    ##  2 2013. N24211  UA      2013-01-01 05:00:00 <NA>      NA    NA    NA
    ##  3 2013. N619AA  AA      2013-01-01 05:00:00 <NA>      NA    NA    NA
    ##  4 2013. N804JB  B6      2013-01-01 05:00:00 <NA>      NA    NA    NA
    ##  5 2013. N668DN  DL      2013-01-01 06:00:00 <NA>      NA    NA    NA
    ##  6 2013. N39463  UA      2013-01-01 05:00:00 <NA>      NA    NA    NA
    ##  7 2013. N516JB  B6      2013-01-01 06:00:00 <NA>      NA    NA    NA
    ##  8 2013. N829AS  EV      2013-01-01 06:00:00 <NA>      NA    NA    NA
    ##  9 2013. N593JB  B6      2013-01-01 06:00:00 <NA>      NA    NA    NA
    ## 10 2013. N3ALAA  AA      2013-01-01 06:00:00 <NA>      NA    NA    NA
    ## # ... with 27,105 more rows, and 9 more variables: temp <dbl>, dewp <dbl>,
    ## #   humid <dbl>, wind_dir <dbl>, wind_speed <dbl>, wind_gust <dbl>,
    ## #   precip <dbl>, pressure <dbl>, visib <dbl>

``` r
#bind_cols(weather, flights2) argument lengths don't match
#intersect(flights2, weather) incompatible col names
#setdiff(flights2, weather)
```

### 3. Let's create a tibble `p` with "x1" and "x2" coulmns and have duplicated element in "x1" column. Create another tibble `q` with "x1" and "x3" columns. Then apply `left_join` function `p` to `q` and `q` to `p`.
