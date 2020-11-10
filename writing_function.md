Writing functions
================
Shane
2020-11-10

## Get Sample

``` r
x_vec = rnorm(30, mean = 10, sd = 5)

x_vec
```

    ##  [1]  6.86773095 10.91821662  5.82185694 17.97640401 11.64753886  5.89765808
    ##  [7] 12.43714526 13.69162353 12.87890676  8.47305806 17.55890584 11.94921618
    ## [13]  6.89379710 -1.07349944 15.62465459  9.77533195  9.91904868 14.71918105
    ## [19] 14.10610598 12.96950661 14.59488686 13.91068150 10.37282492  0.05324152
    ## [25] 13.09912874  9.71935630  9.22102247  2.64623808  7.60924972 12.08970780

``` r
mean(x_vec)
```

    ## [1] 10.41229

``` r
sd(x_vec)
```

    ## [1] 4.620604

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.767120446  0.109493425 -0.993470503  1.637039863  0.267334741
    ##  [6] -0.977065475  0.438222871  0.709719461  0.533829741 -0.419692475
    ## [11]  1.546684110  0.332624325 -0.761479160 -2.485776741  1.128069748
    ## [16] -0.137851865 -0.106748415  0.932105430  0.799422547  0.553437533
    ## [21]  0.905205442  0.757128408 -0.008541293 -2.241925304  0.581490604
    ## [26] -0.149966223 -0.257816586 -1.680744021 -0.606639531  0.363029790

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

    ##  [1] -0.767120446  0.109493425 -0.993470503  1.637039863  0.267334741
    ##  [6] -0.977065475  0.438222871  0.709719461  0.533829741 -0.419692475
    ## [11]  1.546684110  0.332624325 -0.761479160 -2.485776741  1.128069748
    ## [16] -0.137851865 -0.106748415  0.932105430  0.799422547  0.553437533
    ## [21]  0.905205442  0.757128408 -0.008541293 -2.241925304  0.581490604
    ## [26] -0.149966223 -0.257816586 -1.680744021 -0.606639531  0.363029790

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

    ##  [1] -0.767120446  0.109493425 -0.993470503  1.637039863  0.267334741
    ##  [6] -0.977065475  0.438222871  0.709719461  0.533829741 -0.419692475
    ## [11]  1.546684110  0.332624325 -0.761479160 -2.485776741  1.128069748
    ## [16] -0.137851865 -0.106748415  0.932105430  0.799422547  0.553437533
    ## [21]  0.905205442  0.757128408 -0.008541293 -2.241925304  0.581490604
    ## [26] -0.149966223 -0.257816586 -1.680744021 -0.606639531  0.363029790

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
    ## 1 -0.0201  1.04

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
    ## 1  9.83  4.93

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
    ## 1  49.8  10.5

``` r
sim_mean_sd(n=150)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.76  6.25

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
    ##    title                               stars text                               
    ##    <chr>                               <dbl> <chr>                              
    ##  1 Boo                                     1 "We rented this movie because our …
    ##  2 Movie is still silly fun....amazon…     1 "We are getting really frustrated …
    ##  3 Brilliant and awkwardly funny.          5 "I've watched this movie repeatedl…
    ##  4 Great purchase price for great mov…     5 "Great movie and real good digital…
    ##  5 Movie for memories                      5 "I've been looking for this movie …
    ##  6 Love!                                   5 "Love this movie. Great quality"   
    ##  7 Hilarious!                              5 "Such a funny movie, definitely br…
    ##  8 napoleon dynamite                       5 "cool movie"                       
    ##  9 Top 5                                   5 "Best MOVIE ever! Funny one liners…
    ## 10 👍                                      5 "Exactly as described and came on …

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
