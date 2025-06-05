# Intro to R
Yoonjin Lim

``` r
#My first R script
x <- 1:50
plot(x)
```

![](Class04_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
plot (x, sin(x))
```

![](Class04_files/figure-commonmark/unnamed-chunk-1-2.png)

``` r
plot (x, sin(x), typ="l", col="blue", lwd=3, 
      xlab="Silly x axis", ylab="Sensible y axis")
```

![](Class04_files/figure-commonmark/unnamed-chunk-1-3.png)
