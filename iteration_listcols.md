Iteration and list columns
================
Shane
2020-11-10

## Lists

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, FALSE, TRUE, TRUE, FALSE, FALSE, TRUE),
  mat = matrix(1:8, nrow = 4, ncol = 2),
  summary = summary(rnorm(100))
)
```

List for everything.

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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.32241 -0.50136  0.06895  0.19548  0.90662  2.43037

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

get from list

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
    ## 1  3.02  1.09

``` r
mean_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.88  3.00

``` r
mean_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0116 0.192

``` r
mean_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.04  1.15

### new method

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_sd(list_norm[[i]])
  
}
```
