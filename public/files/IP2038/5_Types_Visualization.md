
# Types of visualizations


## Bar plots for discrete variables with `geom_bar()`

Bar plots are often used to visualize the distribution of a discrete variable, like continent. With ggplot2, this visualization can be achieved using the geom `geom_bar()`.

`geom_bar()` requires the following aestetics (`aes`):
- `x`: by default geom_bar will count the number of observations in each category  and plot this as bar lengths



```r
ggplot(data = gapminder, 
       mapping = aes(x=continent)) + 
  geom_bar()
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-1-1.png" width="672" />

It is possible to transform the  calculation used to determine the height of each bar. In this case, we need to provide also an additional aesthetic `y` to determine the length of each bar.

For instance, instead of counting all the rows in our dataset that belong to a given contintent, we may want to plot the number of countries for each continent. Since there are 12 different observations for each country (one per year) in the gapminder dataset, we can calculate the number of countries by diving the total number of observations for each continent by 12.  


```r
ggplot(gapminder, aes(x=continent)) + 
    geom_bar(aes(y=..count../12)) +
    labs(y="Number of countries")
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-2-1.png" width="672" />

## Histograms


```r
ggplot(data=gapminder, 
       mapping = aes(x=lifeExp)) + 
    geom_histogram(bins=60)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-3-1.png" width="672" />


## Density plots 

Density plots can be used to provide useful graphical summary of continuous variables. Instead of dividing the observations into bins as in a histogram, this geom creates a smoothed density curve

Within ggplot, the function `geom_density()` can be used to calculate and plot the smoothed frequency distribution.


```r
library(gapminder)
ggplot(data=gapminder, aes(x=lifeExp)) + 
    geom_density(size=1.5, fill="pink", alpha=0.3)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-4-1.png" width="672" />


The density plot of life expectancy reveals a bimodal distribution. In order to explore if this distribution is the result of differences among countries in different continent, we can addin ggplot2 another aesthetic attribute `fill=continent`, which result in the plotting of different distributions across continent.  In order to see the see the different distributions using transparent colors (alpha=) makes it easier to see the different distributions across continent.


```r
ggplot(data=gapminder, aes(x=lifeExp, fill=continent)) +
    geom_density(alpha=0.3)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-5-1.png" width="672" />


## Boxplots

In order to view the distributions of life expectancy across continents, we can create create a boxplot


```r
ggplot(data=gapminder, 
       aes(x=continent, 
           y=lifeExp,
           fill=continent)) +
    geom_boxplot(outlier.size=2)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-6-1.png" width="672" />

The continents are ordered alphabetically by default. It might be more useful to order them by the mean or median life expectancy. In order to do so, we need to first use the `reorder` function to sort the levels of the continent, before passing this information to ggplot using the "pipe" notation (`%>%`) 


```r
gapminder %>% 
    mutate(continent = reorder(continent, lifeExp, FUN=median)) %>%
    ggplot(aes(x=continent, y=lifeExp, fill=continent)) +
    geom_boxplot(outlier.size=2)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-7-1.png" width="672" />


## Time Series plots

How has life expectancy changed over time across continents?
In ggplot2 we can use the `geom_line()` function to create a line plot.

This `geom_line()` takes three arguments:

- `x`: the variable to be mapped on the x axis. In a time series plot, this is the time
- `y`: the variable to be mapped on the y axis
- `group`: the variable along which different observations should be grouped


```r
library(dplyr)

gapmindermedian <- gapminder %>%
    group_by(continent, year) %>%
    summarise(lifeExp=median(lifeExp))

ggplot(data = gapmindermedian,
       mapping = aes(x = year, 
                     y = lifeExp, 
                     color = continent,
                     group = continent)) +
     geom_line(size=1) + 
     geom_point(size=1.5)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-8-1.png" width="672" />

Then, rather than joining all the points, we could fit linear regression lines for each continent.


```r

gapmindermedian <- gapminder %>%
    group_by(continent, year) %>%
    summarise(lifeExp=median(lifeExp))

ggplot(gapmindermedian, aes(x=year, y=lifeExp, color=continent)) +
    geom_point(size=1.5) +
    geom_smooth(aes(fill=continent), method="lm")
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-9-1.png" width="672" />

## Scatterplots

We can now explore the relationship between life expectancy and GDP with a scatterplot


```r
ggplot(data=gapminder,
       mapping = aes(x = gdpPercap, 
                     y = lifeExp,
                     color = continent)) +
  geom_point()
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-10-1.png" width="672" />

The variable `gdpPercap` is skewed since there are a few observations with a very high value. As a result, the results are better plotted on a log scale.


```r

ggplot(data=gapminder,
       mapping = aes(x = gdpPercap, 
                     y = lifeExp)) +
  geom_point(aes(color = continent)) +
  geom_smooth(method="loess") +
    scale_x_log10(labels=scales::comma)
```

<img src="5_Types_Visualization_files/figure-html/unnamed-chunk-11-1.png" width="672" />

 

As an illustrative  example, the code below takes the gapminder dataset and plots the relationship between life expectancy and GDP per capita.


```r
ggplot(data = gapminder, 
       mapping = aes(x = gdpPercap, 
                     y = lifeExp,
                     color = continent)) + 
  geom_point() + 
  coord_cartesian() + 
  scale_x_log10()  + 
  labs(x = "log GDP", 
       y = "Life Expectancy", 
       title = "A Gapminder Plot")
```

<img src="5_Types_Visualization_files/figure-html/ggplotgapminder-1.png" width="672" />
 
