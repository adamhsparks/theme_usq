theme.usq
================

[![Travis-CI Build Status](https://travis-ci.org/adamhsparks/theme.usq.svg?branch=master)](https://travis-ci.org/adamhsparks/theme.usq) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/adamhsparks/theme.usq?branch=master&svg=true)](https://ci.appveyor.com/project/adamhsparks/theme.usq)

Introduction to *theme.usq*
---------------------------

The goal of *theme.usq* is to provide [University of Southern Queensland](https://usq.edu.au) (USQ) staff and students a quick and easy way to apply USQ colours and typography to graphs created in R using the base *graphics* package or *ggplot2* while providing clear graphs for reports and presentations. All of the colours provided are defined in USQ's Visual Identity Colour Palette, but do not all appear in the same order to maintain usability for the purposes of graphing.

This package has been tested on macOS, Ubuntu Linux and USQ computers using Windows 7. For Linux users, if you have not installed the MS Core Fonts, you will need to do that for this package to function properly and generate the graphs with the proper typography. Windows and macOS users should be ready to go with just the installation of this package.

Quickstart
----------

The *theme.usq* package is only available from GitHub. The easiest way to install it is by using the [*devtools*](https://github.com/hadley/devtools) package.

The installation may take some time as some system fonts need to be catalogued to use the Microsoft Verdana font that USQ suggests. Once the installation is done, it should not be necessary to re-catalogue the fonts so loading *theme.usq* will not take any longer than expected.

``` r

if(!require(devtools)){
    install.packages("devtools")
    library(devtools)
}

devtools::install_github("adamhsparks/theme.usq", dependencies = TRUE)

library("ggplot2")
library("theme.usq")
```

### Keeping *theme.usq* up-to-date

Since *theme.usq* is still under development with bug fixes and new features being added and it is not available from CRAN; `update.packages()` will not update it. To keep *theme.usq* updated, use:

``` r
devtools::update_packages("theme.usq")
```

Examples
--------

### Example 1: Scatterplots of discrete data

#### Using `plot_usq()`

Plot plot car weights by miles per gallon.

``` r
plot_usq(x = mtcars$wt, y = mtcars$mpg)
```

![](./man/figures/README-unnamed-chunk-4-1.png)

#### Using *ggplot2* and `theme.usq()`

Plot car weights by miles per gallon and facet by `Transmission` (0 = automatic, 1 = manual) using the `usq_palette` in the `scale_colour_manual` discrete scale function to use USQ colours for the graph.

``` r
p1 <- ggplot(mtcars) +
  geom_point(aes(
    x = wt,
    y = mpg,
    colour = factor(gear)
  )) +
  scale_colour_manual(values = usq_palette) +
  facet_wrap(~ am)

p1
```

![](./man/figures/README-unnamed-chunk-5-1.png)

Now add the `theme_usq()` to the graph.

``` r
p1 + theme_usq()
```

![](./man/figures/README-unnamed-chunk-6-1.png)

### Example 2: Heatmaps or other continuous data

Using the *theme.usq's* `theme_usq()` for *ggplot2*, plot values using the `usq_fill_gradient` to use USQ colours for continuous values in the graph. Two types of gradients are included, warm and cool for both `scale_fill_gradient()` and `scale_colour_gradient()` as necessary.

#### Warm gradients

``` r
p2a <- ggplot(faithfuld, aes(waiting, eruptions)) +
  geom_raster(aes(fill = density), interpolate = TRUE) +
  usq_fill_gradient_warm() +
  theme_usq()
  
p2a
```

![](./man/figures/README-unnamed-chunk-7-1.png)

#### Cool gradients

``` r
p2b <- ggplot(faithfuld, aes(waiting, eruptions)) +
  geom_raster(aes(fill = density), interpolate = TRUE) +
  usq_fill_gradient_cool() +
  theme_usq()
  
p2b
```

![](./man/figures/README-unnamed-chunk-8-1.png)

### Example 3: Heatmaps using other colour palettes

#### Using *ggplot2* and `theme_usq()`

`theme_usq()` can be used with any colour palette that you wish to use, while still applying the graph styling and typography to the graph.

Use the default *ggplot2* colour scheme to fill the density plot while using the `theme_usq()` to theme the graph.

``` r
p3 <- ggplot(faithfuld, aes(waiting, eruptions)) +
  geom_raster(aes(fill = density), interpolate = TRUE) +
  theme_usq()
  
p3
```

![](./man/figures/README-unnamed-chunk-9-1.png)

### Example 4: Histograms

#### Using `hist_usq()`

``` r
hist_usq(diamonds$carat)
```

![](./man/figures/README-unnamed-chunk-10-1.png)

#### Using *ggplot2* and `theme_usq()`

``` r
p4 <- ggplot(diamonds, aes(carat)) +
  geom_histogram(fill = usq_palette[1]) +
  theme_usq()

p4
#> `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](./man/figures/README-unnamed-chunk-11-1.png)

### Example 5: Boxplots

### 3 Using `boxplot_usq()`

Plot the highway miles per gallon (mpg) of 38 popular car models in the US by class of car.

#### Using `boxplot_usq()`

``` r
boxplot_usq(mpg$hwy ~ mpg$class)
```

![](./man/figures/README-unnamed-chunk-12-1.png)

#### Using *ggplot2* and `theme_usq()`

``` r
p5 <- ggplot(mpg, aes(class, hwy)) +
  geom_boxplot(fill = usq_palette[1],
               colour = usq_palette[1],
               alpha = 0.5) +
  theme_usq()
  
p5
```

![](./man/figures/README-unnamed-chunk-13-1.png)

Use the USQ colours to fill the box-plots while using `drv` (*e.g.*, 4-wheel drive, front-wheel drive or rear-wheel drive) for the box-plot colour.

``` r
p5.1 <- ggplot(mpg, aes(class, hwy)) +
  geom_boxplot(aes(fill = drv), colour = usq_palette[1]) +
  scale_fill_manual(values = usq_palette) +
  theme_usq()
  
p5.1
```

![](./man/figures/README-unnamed-chunk-14-1.png)

### Example 6: Timeseries

#### Using `ggplot2` and `theme_usq()` to plot timeseries lines using discrete

colours for each variable of interest. While possible to do with base R graphics, *ggplot2* simplifies the process greatly, so it is the only example provided and suggested for use.

``` r
p6 <- ggplot(economics_long, aes(date, value01, colour = variable)) +
  geom_line() +
  scale_colour_manual(values = usq_palette) +
  theme_usq()
  
p6
```

![](./man/figures/README-example_6-1.png)

### Example 7: Barplots

#### Using `barplot_usq()`

Plot the areas in thousands of square miles of landmasses which exceed 10,000 sqm.

``` r
barplot_usq(islands, col = 5)
```

![](./man/figures/README-example_7.1-1.png)

#### Using *ggplot2* and `theme_usq()`

Plot the areas in thousands of square miles of landmasses which exceed 10,000 sqm.

``` r
library(tibble)
islands_df <- as.data.frame(islands)
islands_df <- rownames_to_column(islands_df, "name")

ggplot(islands_df, aes(x = name, y = islands)) +
  geom_bar(stat = "identity", 
           colour = usq_palette[5],
           fill = usq_palette[5]) +
  theme_usq() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](./man/figures/README-example_7.2-1.png)

Meta
----

-   Please report any [issues or bugs](https://github.com/adamhsparks/theme.usq/issues).

-   License: MIT

-   Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
