Writing functions
================
Shane
2020-11-07

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1] 10.1548282  0.5569086 17.1377620  7.9081246  4.9294909  8.9767759
    ##  [7] 10.9415938  4.5703564  8.9763339  5.0936750  7.7959948 10.8888073
    ## [13]  1.2345977  3.1948311  6.6131209 18.4096493 14.4179038 17.6103718
    ## [19]  4.0948997 14.0418653 11.1850449  6.1864820  9.0301624  6.3701271
    ## [25]  2.9381709  9.8711701  4.4281032 10.5869185 11.8711858  9.4065814

``` r
mean(x_vec)
```

    ## [1] 8.647395

``` r
sd(x_vec)
```

    ## [1] 4.648684

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.32427106 -1.74038202  1.82640239 -0.15902780 -0.79977554  0.07085474
    ##  [7]  0.49351584 -0.87703062  0.07075967 -0.76445713 -0.18314856  0.48216070
    ## [13] -1.59460118 -1.17292626 -0.43760207  2.10000398  1.24132106  1.92806768
    ## [19] -0.97930833  1.16042965  0.54588574 -0.52937834  0.08233896 -0.48987360
    ## [25] -1.22813763  0.26325204 -0.90763137  0.41722001  0.69348472  0.16331221

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

    ##  [1]  0.32427106 -1.74038202  1.82640239 -0.15902780 -0.79977554  0.07085474
    ##  [7]  0.49351584 -0.87703062  0.07075967 -0.76445713 -0.18314856  0.48216070
    ## [13] -1.59460118 -1.17292626 -0.43760207  2.10000398  1.24132106  1.92806768
    ## [19] -0.97930833  1.16042965  0.54588574 -0.52937834  0.08233896 -0.48987360
    ## [25] -1.22813763  0.26325204 -0.90763137  0.41722001  0.69348472  0.16331221

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

    ##  [1]  0.32427106 -1.74038202  1.82640239 -0.15902780 -0.79977554  0.07085474
    ##  [7]  0.49351584 -0.87703062  0.07075967 -0.76445713 -0.18314856  0.48216070
    ## [13] -1.59460118 -1.17292626 -0.43760207  2.10000398  1.24132106  1.92806768
    ## [19] -0.97930833  1.16042965  0.54588574 -0.52937834  0.08233896 -0.48987360
    ## [25] -1.22813763  0.26325204 -0.90763137  0.41722001  0.69348472  0.16331221

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
    ## 1 0.00573 0.981

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
    ## 1  9.82  5.27

``` r
sim_mean_sd = function(n, mu=3, sigma=6) {
  
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
    ## 1  50.2  9.83

``` r
sim_mean_sd(n=150)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.52  5.94
