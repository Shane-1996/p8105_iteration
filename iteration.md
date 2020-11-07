Writing functions
================
Shane
2020-11-07

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1] 11.0305328 11.2717530  7.4984575  6.3585714 14.0252623 14.9532583
    ##  [7] 14.1774556 12.4244094  4.7234323 12.5291064 11.5378840 12.5609982
    ## [13]  5.2094399  8.4838847 16.6238221 16.0597278 10.1766068 10.7347658
    ## [19]  2.5140985 10.1515788 19.6005465  4.5581031 10.8394089 13.3752870
    ## [25] 15.5497700  3.7081206 -2.1222622 -0.4309813 20.1526163  6.1395623

``` r
mean(x_vec)
```

    ## [1] 10.14717

``` r
sd(x_vec)
```

    ## [1] 5.47488

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.1613476403  0.2054070893 -0.4837944376 -0.6919973755  0.7083421955
    ##  [6]  0.8778428980  0.7361406638  0.4159425521 -0.9906594696  0.4350657037
    ## [11]  0.2540165572  0.4408908335 -0.9018890114 -0.3038037846  1.1829754035
    ## [16]  1.0799422194  0.0053759875  0.1073250703 -1.3941996068  0.0008045655
    ## [21]  1.7266812789 -1.0208572531  0.1264384001  0.5896226344  0.9867972012
    ## [26] -1.1761085936 -2.2410420635 -1.9321255370  1.8275181587 -0.7319999200

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

    ##  [1]  0.1613476403  0.2054070893 -0.4837944376 -0.6919973755  0.7083421955
    ##  [6]  0.8778428980  0.7361406638  0.4159425521 -0.9906594696  0.4350657037
    ## [11]  0.2540165572  0.4408908335 -0.9018890114 -0.3038037846  1.1829754035
    ## [16]  1.0799422194  0.0053759875  0.1073250703 -1.3941996068  0.0008045655
    ## [21]  1.7266812789 -1.0208572531  0.1264384001  0.5896226344  0.9867972012
    ## [26] -1.1761085936 -2.2410420635 -1.9321255370  1.8275181587 -0.7319999200

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

    ##  [1]  0.1613476403  0.2054070893 -0.4837944376 -0.6919973755  0.7083421955
    ##  [6]  0.8778428980  0.7361406638  0.4159425521 -0.9906594696  0.4350657037
    ## [11]  0.2540165572  0.4408908335 -0.9018890114 -0.3038037846  1.1829754035
    ## [16]  1.0799422194  0.0053759875  0.1073250703 -1.3941996068  0.0008045655
    ## [21]  1.7266812789 -1.0208572531  0.1264384001  0.5896226344  0.9867972012
    ## [26] -1.1761085936 -2.2410420635 -1.9321255370  1.8275181587 -0.7319999200

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
    ## 1 0.0103 0.975

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
    ## 1  9.80  5.18

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
    ## 1  49.5  10.0
