Writing functions
================
Shane
2020-11-07

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1] 10.679871 14.813832  9.464004  7.020862 18.201729 15.745667 14.657535
    ##  [8]  8.560762 14.632616 -1.548187  7.148730 12.545449  7.972930 12.066297
    ## [15] 14.669986 13.349772 14.582994  6.407160 18.817172 12.542287 14.340388
    ## [22] 12.178235 18.336018 14.083844  2.879845 11.373601  5.931022  6.448065
    ## [29]  7.080531 10.509461

``` r
mean(x_vec)
```

    ## [1] 11.18308

``` r
sd(x_vec)
```

    ## [1] 4.712783

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.10677591  0.77040466 -0.36476941 -0.88317692  1.48927861  0.96812970
    ##  [7]  0.73724007 -0.55642728  0.73195252 -2.70143366 -0.85604469  0.28907887
    ## [13] -0.68115858  0.18740823  0.73988217  0.45974739  0.72142339 -1.01339765
    ## [19]  1.61986886  0.28840808  0.66994505  0.21116026  1.51777334  0.61550929
    ## [25] -1.76185473  0.04042595 -1.11442880 -1.00471798 -0.87051575 -0.14293508

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

    ##  [1] -0.10677591  0.77040466 -0.36476941 -0.88317692  1.48927861  0.96812970
    ##  [7]  0.73724007 -0.55642728  0.73195252 -2.70143366 -0.85604469  0.28907887
    ## [13] -0.68115858  0.18740823  0.73988217  0.45974739  0.72142339 -1.01339765
    ## [19]  1.61986886  0.28840808  0.66994505  0.21116026  1.51777334  0.61550929
    ## [25] -1.76185473  0.04042595 -1.11442880 -1.00471798 -0.87051575 -0.14293508

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

    ##  [1] -0.10677591  0.77040466 -0.36476941 -0.88317692  1.48927861  0.96812970
    ##  [7]  0.73724007 -0.55642728  0.73195252 -2.70143366 -0.85604469  0.28907887
    ## [13] -0.68115858  0.18740823  0.73988217  0.45974739  0.72142339 -1.01339765
    ## [19]  1.61986886  0.28840808  0.66994505  0.21116026  1.51777334  0.61550929
    ## [25] -1.76185473  0.04042595 -1.11442880 -1.00471798 -0.87051575 -0.14293508

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
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0142  1.02

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
    ## 1  10.1  4.80

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
    ## 1  49.7  10.4
