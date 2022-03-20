# Working with Observations

## Introduction

This chapter will explore different tools used to  manipulate observations (i.e. rows) of a data frame or tibble:

- The `filter()` function extracts a subset of observations based on supplied conditions involving the variables in our data.
- The `arrange()` function reorders the rows according to the values in one or more variables.


### Getting Started

A script that uses dplyr will typically start by loading and attaching the package:


```r
library("dplyr")
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
```

The dplyr package is also part of the tidyverse package, and it can be loaded from there. Remember that you will need first to have first installed dplyr package for this to work.



```r
library("tidyverse")
```


We’ll use the `gapminder` data set in the datasets package to illustrate the working of the functions presented in this  chapter. This is loaded from the `gapminder` package. We can also store the dataset in an object called gapminder, although this is not required.



```r
library("gapminder")
```

## Subset Observations with Filter()

![](images/Filter_Dplyr.png){width=80%}

The `filter()` function is used to subset a data frame, retaining all rows where the condition expressed within the `filter()` is true.

For instance, it is possible to retain only those observations (row) where a variable takes a certain value.

In the example below we want to retain only those observations from the gapminder dataset  that pertain to Argentina. In order to do so, we filter those rows where the variable `country` is equal to (`==`) the string `Argentina`


```r
gapminder_argentina <- gapminder %>%
    filter(country == "Argentina")

head (gapminder_argentina)	
#> # A tibble: 6 × 6
#>   country   continent  year lifeExp      pop gdpPercap
#>   <fct>     <fct>     <int>   <dbl>    <int>     <dbl>
#> 1 Argentina Americas   1952    62.5 17876956     5911.
#> 2 Argentina Americas   1957    64.4 19610538     6857.
#> 3 Argentina Americas   1962    65.1 21283783     7133.
#> 4 Argentina Americas   1967    65.6 22934225     8053.
#> 5 Argentina Americas   1972    67.1 24779799     9443.
#> 6 Argentina Americas   1977    68.5 26983828    10079.
```

### Logical operations

The `filter()` functions can also be used to  filter rows that match multiple conditions. The rows that will be returned in this case depends on the logical operator that connects these conditions.


#### & (and)

The boolean operator `&` (`*and*`) can be used to retain those rows that meet multiple conditions

When multiple expressions are listed within parentheses separated by a comma, the function will only retain those rows where all the criteria are met.

In this example we filter those observations that pertains to African countries after the year 1982. In the first example, the two conditions are separated by the `&` operator within the same `filter` function, in the second example by a comma, and in the third example by calling different filters in sequential order. 
 

```r
gapminder_africa <- gapminder %>%
	filter(continent == "Africa" & year > 1982) 

#Alternative formulation
gapminder_africa <- gapminder %>%
	filter(continent == "Africa",
		 year > 1982)  


#Alternative formulation
gapminder_africa <- gapminder %>%
	filter(continent == "Africa") %>%
	filter(year > 1982) 

head(gapminder_africa)
#> # A tibble: 6 × 6
#>   country continent  year lifeExp      pop gdpPercap
#>   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
#> 1 Algeria Africa     1987    65.8 23254956     5681.
#> 2 Algeria Africa     1992    67.7 26298373     5023.
#> 3 Algeria Africa     1997    69.2 29072015     4797.
#> 4 Algeria Africa     2002    71.0 31287142     5288.
#> 5 Algeria Africa     2007    72.3 33333216     6223.
#> 6 Angola  Africa     1987    39.9  7874230     2430.
```

#### | (or)

While the boolean operator `&` can be used to retain those rows that meet multiple conditions, the boolean operator `or (`|`) can be used to retain the rows that meet any of multiple conditions.

For instance, in this example we can filter all the rows where the variable `continent` takes the value "Africa" or "Asia".


```r

gapminder_africa_asia <- gapminder %>%
	filter(continent == "Africa" | continent == "Asia")  
```

### ! (*not*)

The `!` operator can be used to check if a condition is "not" present. For instance, we can use `!=` to filter observations that are different from a given value.

In this example we exclude from the analysis all the rows where the variable `continent` takes the value "Oceania".



```r

gapminder_notOceania <- gapminder %>%
	filter(continent != "Oceania")  
```




### %in% helper

When we are interested in a subset of rows that can contain several different values, instead of writing a long OR command, its useful to just give a vector of values of interest.
In this case, the `%in%` operator can be called to retain all the rows where the value includes one of multiple values using 


```r

gapminder_africa_asia <- gapminder %>%
	filter(continent %in% c("Africa","Asia") ) 
```

### Filter using Comparison operators 

Observations can be filtered using [comparison operators](https://rdrr.io/r/base/Comparison.html) such as
  - `==` (equal to)
  - `>` (greater than)
  - `>=` (greater or equal to)
  - `<` (smaller than), etc...
  
For instance, in the example below we want to retain all the observations after the year 1982.


```r

gapminder_recent <- gapminder %>%
	filter(year > 1982) 
```
 
 

## Sorting observations

We can use the `arrange()` to sort the rows in our data frame according to values in a certain column. 


```r
gapminder %>%
    arrange(year)
#> # A tibble: 1,704 × 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Albania     Europe     1952    55.2  1282697     1601.
#>  3 Algeria     Africa     1952    43.1  9279525     2449.
#>  4 Angola      Africa     1952    30.0  4232095     3521.
#>  5 Argentina   Americas   1952    62.5 17876956     5911.
#>  6 Australia   Oceania    1952    69.1  8691212    10040.
#>  7 Austria     Europe     1952    66.8  6927772     6137.
#>  8 Bahrain     Asia       1952    50.9   120447     9867.
#>  9 Bangladesh  Asia       1952    37.5 46886859      684.
#> 10 Belgium     Europe     1952    68    8730405     8343.
#> # … with 1,694 more rows
```


### desc() helper function

When we call the `arrange()` function, the default is to order numbers from lowest -> highest.
To invert the standard order, we can use the ‘descending’ desc() helper function.  


```r
gapminder %>%
    arrange(desc(year))
#> # A tibble: 1,704 × 6
#>    country     continent  year lifeExp       pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
#>  1 Afghanistan Asia       2007    43.8  31889923      975.
#>  2 Albania     Europe     2007    76.4   3600523     5937.
#>  3 Algeria     Africa     2007    72.3  33333216     6223.
#>  4 Angola      Africa     2007    42.7  12420476     4797.
#>  5 Argentina   Americas   2007    75.3  40301927    12779.
#>  6 Australia   Oceania    2007    81.2  20434176    34435.
#>  7 Austria     Europe     2007    79.8   8199783    36126.
#>  8 Bahrain     Asia       2007    75.6    708573    29796.
#>  9 Bangladesh  Asia       2007    64.1 150448339     1391.
#> 10 Belgium     Europe     2007    79.4  10392226    33693.
#> # … with 1,694 more rows
```


### Sorting by multiple columns

It is possible to refine the order by giving additional columns of data.
In the example below we organize our rows first by `year` (recent first), and then by `continent`. 


```r
gapminder_with_extra_vars <- gapminder %>%
    group_by(continent, year) %>%
    mutate(mean_gdpPercap = mean(gdpPercap),
              sd_gdpPercap = sd(gdpPercap),
              mean_pop = mean(pop),
              sd_pop = sd(pop)) %>%
    arrange(desc(year), continent)

head(gapminder_with_extra_vars)
#> # A tibble: 6 × 10
#> # Groups:   continent, year [1]
#>   country      continent  year lifeExp      pop gdpPercap
#>   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>
#> 1 Algeria      Africa     2007    72.3 33333216     6223.
#> 2 Angola       Africa     2007    42.7 12420476     4797.
#> 3 Benin        Africa     2007    56.7  8078314     1441.
#> 4 Botswana     Africa     2007    50.7  1639131    12570.
#> 5 Burkina Faso Africa     2007    52.3 14326203     1217.
#> 6 Burundi      Africa     2007    49.6  8390505      430.
#> # … with 4 more variables: mean_gdpPercap <dbl>,
#> #   sd_gdpPercap <dbl>, mean_pop <dbl>, sd_pop <dbl>
```

