Writing functions
================
Shane
2020-11-07

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1]  7.6795191 13.6271844 10.5207727  0.6244347 10.3362487  4.5116781
    ##  [7] 12.0463083 14.7490502 18.1712262 10.9518432 11.9090445 10.2284604
    ## [13] 20.2885502 15.2109965 18.5922140  7.2138621  7.0408512 13.8585691
    ## [19] 11.8492280 14.0607355  3.9391659  7.0795771 12.7144437  7.0844777
    ## [25]  2.9129840  1.7505716  7.7800596  7.6200399 12.4661874 16.1638923

``` r
mean(x_vec)
```

    ## [1] 10.43274

``` r
sd(x_vec)
```

    ## [1] 4.988513

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.55191201  0.64036024  0.01764725 -1.96617811 -0.01934254 -1.18693916
    ##  [7]  0.32345694  0.86525007  1.55126135  0.10405986  0.29594096 -0.04094985
    ## [13]  1.97570128  0.95785209  1.63565278 -0.64525787 -0.67993973  0.68674374
    ## [19]  0.28395013  0.72727013 -1.30170528 -0.67217672  0.45739173 -0.67119433
    ## [25] -1.50741426 -1.74043208 -0.53175761 -0.56383525  0.40762615  1.14887010

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

    ##  [1] -0.55191201  0.64036024  0.01764725 -1.96617811 -0.01934254 -1.18693916
    ##  [7]  0.32345694  0.86525007  1.55126135  0.10405986  0.29594096 -0.04094985
    ## [13]  1.97570128  0.95785209  1.63565278 -0.64525787 -0.67993973  0.68674374
    ## [19]  0.28395013  0.72727013 -1.30170528 -0.67217672  0.45739173 -0.67119433
    ## [25] -1.50741426 -1.74043208 -0.53175761 -0.56383525  0.40762615  1.14887010

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

    ##  [1] -0.55191201  0.64036024  0.01764725 -1.96617811 -0.01934254 -1.18693916
    ##  [7]  0.32345694  0.86525007  1.55126135  0.10405986  0.29594096 -0.04094985
    ## [13]  1.97570128  0.95785209  1.63565278 -0.64525787 -0.67993973  0.68674374
    ## [19]  0.28395013  0.72727013 -1.30170528 -0.67217672  0.45739173 -0.67119433
    ## [25] -1.50741426 -1.74043208 -0.53175761 -0.56383525  0.40762615  1.14887010

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
    ## 1 0.0150  1.03

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
    ## 1  10.0  5.50

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
    ## 1  49.9  9.83

``` r
sim_mean_sd(n=150)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.89  5.85

## Scrape review information

### From Amazon

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

### Turn codes to fucntion

``` r
amazon_review = function(url) {
  
  html = read_html(url)

  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
    )
  
  reviews
  
}
```

### Testing

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

amazon_review(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                    stars text                                          
    ##    <chr>                    <dbl> <chr>                                         
    ##  1 Brilliant and awkwardly…     5 "I've watched this movie repeatedly. If you w…
    ##  2 Great purchase price fo…     5 "Great movie and real good digital purchase p…
    ##  3 Movie for memories           5 "I've been looking for this movie to buy for …
    ##  4 Love!                        5 "Love this movie. Great quality"              
    ##  5 Hilarious!                   5 "Such a funny movie, definitely brought me ba…
    ##  6 napoleon dynamite            5 "cool movie"                                  
    ##  7 Top 5                        5 "Best MOVIE ever! Funny one liners and just q…
    ##  8 👍                           5 "Exactly as described and came on time 👍"    
    ##  9 A top favorite movie !!      5 "Love this movie, needed to add it to my coll…
    ## 10 Best.Movie!                  5 "I enjoyed showing my children this \"classic…

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

multi_reviews = 
  bind_rows(
    amazon_review(dynamite_urls[1]),
    amazon_review(dynamite_urls[2]),
    amazon_review(dynamite_urls[3]),
    amazon_review(dynamite_urls[4]),
    amazon_review(dynamite_urls[5])
  )
```
