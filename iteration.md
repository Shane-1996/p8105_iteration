Iteration
================
Shane
2020-11-07

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1]  9.4317128  7.9826213 14.1617850 10.7755524  6.5596544 13.6917193
    ##  [7] 17.7414472 13.7124944 12.8282962 18.3824249 11.7421633  5.9533049
    ## [13]  3.4951034 13.3436904  0.6649489 13.3575569  5.8392787  8.3714331
    ## [19] 11.3336590  7.5280889 14.9665675  6.9711902 22.6487308  5.0437874
    ## [25] 11.6596879  8.3110047 -0.5426961  7.2418877  5.8962117 10.1280502

``` r
mean(x_vec)
```

    ## [1] 9.974045

``` r
sd(x_vec)
```

    ## [1] 5.114299

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.10604238 -0.38938358  0.81882972  0.15671888 -0.66761663  0.72691767
    ##  [7]  1.51876189  0.73097982  0.55809234  1.64409241  0.34572053 -0.78617628
    ## [13] -1.26682900  0.65886750 -1.82020974  0.66157880 -0.80847185 -0.31335913
    ## [19]  0.26584560 -0.47825840  0.97618903 -0.58714894  2.47828418 -0.96401445
    ## [25]  0.32959409 -0.32517471 -2.05634086 -0.53421940 -0.79733973  0.03011263

z\_score function a number minuous mean over the sd

## Simple Function

### Simple function for z\_score

``` r
z_score_1 = function(x){
  
  z = (x-mean(x)) / sd(x)
  
  return(z)
  
}

z_score_1(x_vec)
```

    ##  [1] -0.10604238 -0.38938358  0.81882972  0.15671888 -0.66761663  0.72691767
    ##  [7]  1.51876189  0.73097982  0.55809234  1.64409241  0.34572053 -0.78617628
    ## [13] -1.26682900  0.65886750 -1.82020974  0.66157880 -0.80847185 -0.31335913
    ## [19]  0.26584560 -0.47825840  0.97618903 -0.58714894  2.47828418 -0.96401445
    ## [25]  0.32959409 -0.32517471 -2.05634086 -0.53421940 -0.79733973  0.03011263

### Simple function with Check

``` r
z_score_2 = function(x) {
  
  if (!is.numeric(x)) {
    stop("input must be numeric")
  }
  
  if (length(x)<5) {
    stop("input must exceed 5 objects")
  }
  
  z = (x-mean(x)) / sd(x)
  
  return(z)
  
}

z_score_2(x_vec)
```

    ##  [1] -0.10604238 -0.38938358  0.81882972  0.15671888 -0.66761663  0.72691767
    ##  [7]  1.51876189  0.73097982  0.55809234  1.64409241  0.34572053 -0.78617628
    ## [13] -1.26682900  0.65886750 -1.82020974  0.66157880 -0.80847185 -0.31335913
    ## [19]  0.26584560 -0.47825840  0.97618903 -0.58714894  2.47828418 -0.96401445
    ## [25]  0.32959409 -0.32517471 -2.05634086 -0.53421940 -0.79733973  0.03011263

### with other input

``` r
z_score_2(3)
```

    ## Error in z_score_2(3): input must exceed 5 objects

``` r
z_score_2("i love potato")
```

    ## Error in z_score_2("i love potato"): input must be numeric

``` r
z_score_2(mtcars)
```

    ## Error in z_score_2(mtcars): input must be numeric

``` r
z_score_2(c(TRUE, FALSE, TRUE, FALSE))
```

    ## Error in z_score_2(c(TRUE, FALSE, TRUE, FALSE)): input must be numeric

Not working or working in the wrong direction with simple function.

## Multiple function

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

xx_vec = rnorm(1000)

mean_sd(xx_vec)
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0235 0.964

## Multiple input

simple input

``` r
sim_data = 
  tibble(
    x = rnorm(n=100, mean=10, sd=5)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.69  4.41

``` r
sim_mean_sd = function(n, mu, sigma) {
  
  sim_data = 
   tibble(
     x = rnorm(n=n, mean=mu, sd=sigma)
    )

  sim_data %>% 
    summarize(
      mean = mean(x),
     sd = sd(x)
   )
  
}

sim_mean_sd(1000, 50, 10)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  50.0  10.3
