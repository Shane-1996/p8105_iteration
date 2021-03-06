Iteration and list columns
================
Shane
2020-11-10

## Lists

### List for everything.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, FALSE, TRUE, TRUE, FALSE, FALSE, TRUE),
  mat = matrix(1:8, nrow = 4, ncol = 2),
  summary = summary(rnorm(100))
)
```

### get from list

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE  TRUE  TRUE FALSE FALSE  TRUE
    ## 
    ## $mat
    ##      [,1] [,2]
    ## [1,]    1    5
    ## [2,]    2    6
    ## [3,]    3    7
    ## [4,]    4    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.2147 -0.4942  0.1139  0.1089  0.6915  2.4016

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]][1:3]
```

    ## [1] 5 6 7

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## ‘for’ loop

``` r
list_norm = list(
  a = rnorm(20, 3, 1),
  b = rnorm(40, 5, 3),
  c = rnorm(60, 0, .2),
  d = rnorm(50, -3, 1)
)
```

``` r
mean_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("input must be numeric")
  }
  
  if (length(x)<5) {
    stop("input must exceed 5 objects")
  }
  
  mean_x = mean(x)
  
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

### old method

``` r
mean_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.11 0.814

``` r
mean_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.07  2.86

``` r
mean_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0389 0.205

``` r
mean_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.13 0.992

### new method

``` r
output_1 = vector("list", length = 4)

for (i in 1:4) {
  
  output_1[[i]] = mean_sd(list_norm[[i]])
  
}
```

## Map

``` r
output_2 = map(list_norm, mean_sd)

output_3 = map(list_norm, median)

output_2
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.11 0.814
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.07  2.86
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0389 0.205
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.13 0.992

``` r
output_3
```

    ## $a
    ## [1] 2.807644
    ## 
    ## $b
    ## [1] 4.036492
    ## 
    ## $c
    ## [1] 0.03133407
    ## 
    ## $d
    ## [1] -3.083917

``` r
output_3 = map_dbl(list_norm, median)

output_2 = map_df(list_norm, mean_sd, .id = "input")


output_2
```

    ## # A tibble: 4 x 3
    ##   input    mean    sd
    ##   <chr>   <dbl> <dbl>
    ## 1 a      3.11   0.814
    ## 2 b      4.07   2.86 
    ## 3 c      0.0389 0.205
    ## 4 d     -3.13   0.992

``` r
output_3
```

    ##           a           b           c           d 
    ##  2.80764439  4.03649203  0.03133407 -3.08391678

## List columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 2.379633 3.042116 2.089078 3.158029 2.345415 4.767287 3.716707 3.910174
    ##  [9] 3.384185 4.682176 2.364264 2.538355 4.432282 2.349304 2.792619 2.607192
    ## [17] 2.680007 2.720887 3.494188 2.822670
    ## 
    ## $b
    ##  [1]  3.482127614  9.029116476  4.356261774  4.461330410  4.699427776
    ##  [6]  7.137998921  4.779306788  4.887097486  2.955018564  4.027189183
    ## [11]  5.180481321  3.233316541  6.594488578  0.444817755  5.919673582
    ## [16]  0.390650529  4.097071619  3.415160287  3.043715658  4.829309666
    ## [21] -0.743078277  8.529749936  0.005082691  3.609408796  1.652239685
    ## [26]  2.747542996 11.261499637  5.052186859  1.141098409  0.078183397
    ## [31]  6.350561304  4.944320502  4.045794876  2.211913558  0.537619070
    ## [36]  1.774423110  8.000086411  3.136199916  0.846719458 10.607871867
    ## 
    ## $c
    ##  [1]  0.085020075 -0.047729420  0.211696610  0.177284530 -0.123848610
    ##  [6]  0.441220493 -0.051005406 -0.284898930 -0.028879920  0.041507668
    ## [11]  0.461595680  0.021160474  0.091399761 -0.015430587 -0.066800168
    ## [16] -0.006945206  0.157527921  0.415049002  0.205478488  0.241581680
    ## [21] -0.246264684  0.196779114  0.043984961 -0.293450006  0.104204549
    ## [26] -0.031750921  0.292917462 -0.153216400 -0.086042351 -0.185221899
    ## [31] -0.035420792  0.080402356 -0.146349635  0.166074634 -0.241616557
    ## [36] -0.209596883  0.288231541 -0.203169493  0.082394942 -0.076215210
    ## [41]  0.081880368  0.337774657  0.317317687 -0.066181560 -0.457047107
    ## [46]  0.499532318  0.133413233  0.108265467 -0.002679905  0.102021685
    ## [51] -0.032875166  0.084138929 -0.080049349 -0.274041576  0.197567653
    ## [56]  0.303949005 -0.061748114 -0.250657951  0.128448261 -0.008941827
    ## 
    ## $d
    ##  [1] -4.733218 -2.997868 -3.630300 -3.340969 -4.156572 -1.196858 -3.331132
    ##  [8] -4.605513 -2.802807 -2.736824 -3.985827 -5.888921 -3.640482 -2.429492
    ## [15] -3.059723 -3.098179 -2.439179 -4.186459 -1.903223 -3.005344 -2.292689
    ## [22] -1.965892 -2.776520 -3.878708 -1.837035 -5.000165 -3.544791 -3.255671
    ## [29] -3.166121 -1.979536 -2.863778 -2.592832 -3.069655 -3.247664 -2.304449
    ## [36] -1.853772 -5.403096 -2.427260 -2.625276 -3.425268 -2.048987 -3.389237
    ## [43] -3.284331 -2.142590 -1.280373 -2.729945 -3.422184 -4.189113 -3.331033
    ## [50] -3.939829

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

``` r
listcol_df$samp[[1]]
```

    ##  [1] 2.379633 3.042116 2.089078 3.158029 2.345415 4.767287 3.716707 3.910174
    ##  [9] 3.384185 4.682176 2.364264 2.538355 4.432282 2.349304 2.792619 2.607192
    ## [17] 2.680007 2.720887 3.494188 2.822670

``` r
mean_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.11 0.814

``` r
map(listcol_df$samp, mean_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.11 0.814
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.07  2.86
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0389 0.205
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.13 0.992

``` r
listcol_summary =
  listcol_df %>% 
  mutate(
    summary = map(samp, mean_sd),
    medians = map_dbl(samp, median)
  )
```

## Example: Weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/shane/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-01 09:52:54 (7.52)

    ## file min/max dates: 1869-01-01 / 2020-09-30

    ## using cached file: /Users/shane/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-01 09:52:59 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: /Users/shane/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-01 09:53:02 (0.877)

    ## file min/max dates: 1999-09-01 / 2020-09-30

### get nested

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

### regression

``` r
lm(tmax ~ tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
weather_lm = function(df) {
  
  lm(tmax ~ tmin, data = df)
  
}

weather_lm(weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
output = vector("list", 3)

for (i in 1:3) {
  
  output[i] = weather_lm(weather_nest$data[[i]])
  
}
```

### map

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

### map in list column

``` r
weather_reg = 
  weather_nest %>% 
  mutate(models = map(data, weather_lm))

weather_reg$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221
