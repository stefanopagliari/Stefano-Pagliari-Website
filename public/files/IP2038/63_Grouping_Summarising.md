# Grouping and Summarising Data

## Introduction



This chapter will explore different tools to group and create summarise our data:

- The `group_by()` function adds information into a data object (e.g. a data frame or tibble), which makes subsequent calculations happen on a group-specific basis.
- The `summarise()` function is a data reduction function calculates single-number summaries of one or more variables, respecting the group structure if present.


### Getting Started

A script that uses dplyr will typically start by loading and attaching the package:


```r
library("dplyr")
```

The dplyr package is also part of the tidyverse package, and it can be loaded from there. Remember that you will need first to have first installed dplyr package for this to work.



```r
library("tidyverse")
```


We’ll use the `gapminder` data set in the datasets package to illustrate the working of the functions presented in this  chapter. This is loaded from the `gapminder` package. We can also store the dataset in an object called gapminder, although this is not required.



```r
library("gapminder")
gapminder <- gapminder
```


## Summarising variables with `summarise`

We can use summarise to calculate summaries of variables in our dataset.

The `summarise()` function takes the following argument:

`dataset %>% summarise(<expression1>, <expression2>)`

Here `<expression1>` are `<expression2>`are placeholder for functions or other operations that we want to perform on our data.

For instance, in the example below we take the gapminder dataset and we use the `summarise()` function to calculate the average value of the life expectancy variable, as well as the highest/ and lowest value of the same variable 


```r
gapminder %>%
    summarize(mean(lifeExp),
			        min(lifeExp),
			        max(lifeExp)) 
#> # A tibble: 1 × 3
#>   `mean(lifeExp)` `min(lifeExp)` `max(lifeExp)`
#>             <dbl>          <dbl>          <dbl>
#> 1            59.5           23.6           82.6
```

The `summarise` function returns a table with only one row and three columns, one of each of the operations we specified (`mean`, `min`, `max`).
The order of the expression in the resulting table is the same as the order in which they were used as arguments.

Notice that `summarise` used the expressions to name the variables. We can name the new variables at the same time as they are created by using `=`, placing the name we require on the left hand side. For example:


```r
gapminder %>%
    summarize(mean_le = mean(lifeExp),
			        min_le = min(lifeExp),
			        max_le = max(lifeExp)) 
#> # A tibble: 1 × 3
#>   mean_le min_le max_le
#>     <dbl>  <dbl>  <dbl>
#> 1    59.5   23.6   82.6
```

There are very many base R functions that can be used with summarise. A few useful ones for calculating summaries of numeric variables are:

- `min` and `max` calculate the minimum and maximum values of a vector.
- `mean` and `median` calculate the mean and median of a numeric vector.
- `sd` and `var` calculate the standard deviation and variance of a numeric vector.

##  Summarise Across Groups using `group_by`


In the same way in which we can construct numerical summaries for numeric variables, we can also apply the same calculations and functions to subsets of the same data depending on the values of the categorical variable.

![](images/Summarise_Dplyr.png){width=80%}

Often we need to aggregate the observations in our data in order to gain some insights into the differences across different groups.

For instance, looking at the gapminder dataset we may want to know how life expectancy varies across different continents, rather than knowing the overall mean of these traits across all observations. We could calculate separate means by using filter to create different subsets of gapminder, and then using summary on each of these to calculate the relevant means. This however would be inefficient.

The function `group_by()` allow us to group all the observations in our dataset that correspond to a particular value or set of values. 

For instance, calling the `group_by()` function for the `gapminder` dataset and specifying the `continent` variable will group all the observations where this variable takes the same value (e.g. all the observation for African countries,  Asian countries, European countries, etc...)

![](images/dplyr_groupby.png){width=80%}[source](https://swcarpentry.github.io/r-novice-gapminder/13-dplyr/)
Once we have specified how our data should be grouped, we can use this in conjunction with the `summarize()` function. This variable  allows us to create new variable(s) by applying transformations to variables in each of our groups (for example, for our continent-specific data frames).

![](images/dplyr_Groupby_summarize.png){width=80%}[source](https://swcarpentry.github.io/r-novice-gapminder/13-dplyr/)

When we used summarise on an ungrouped table the result was a new table with one row. Now the resulting tibble has five rows, one for each value of type (in this case one for each continent).



```r
gapminder %>%
    group_by(continent) %>%
    summarize(mean_le = mean(lifeExp))
#> # A tibble: 5 × 2
#>   continent mean_le
#>   <fct>       <dbl>
#> 1 Africa       48.9
#> 2 Americas     64.7
#> 3 Asia         60.1
#> 4 Europe       71.9
#> 5 Oceania      74.3
```

###  Grouping by multiple variables

The function `group_by()` also to create sub-groups based on the variable of a particular variable, and to run subsequent functions on all sub-groups.

For instance, in the example below, instead of calculating the average value of all the observations that belongs to the same continent, we group the observations only by `continent` and by `year` before calculating the average `gdpPercap`. This transformation results into the creation of a dataset where the rows represents each unique combination of the values available for these variables (e.g. Africa in 1957, Asia in 1992).



```r
gapminder %>%
    group_by(continent, year) %>%
    summarize(mean_gdpPercap = mean(gdpPercap))
#> # A tibble: 60 × 3
#> # Groups:   continent [5]
#>    continent  year mean_gdpPercap
#>    <fct>     <int>          <dbl>
#>  1 Africa     1952          1253.
#>  2 Africa     1957          1385.
#>  3 Africa     1962          1598.
#>  4 Africa     1967          2050.
#>  5 Africa     1972          2340.
#>  6 Africa     1977          2586.
#>  7 Africa     1982          2482.
#>  8 Africa     1987          2283.
#>  9 Africa     1992          2282.
#> 10 Africa     1997          2379.
#> # … with 50 more rows
```


###  Summarizing different variables

When you use the `summarize()` function it is possible to define different new variables at the same time.

For instance, in this example, after we grouped together different observations with the same value for the `continent` variable, we can calculate the average, lowest, and highest life expectancy (`lifeExp` variable) among all the observations of that group.


```r
gapminder %>%
    group_by(continent) %>%
    summarize(mean_le = mean(lifeExp),
			  min_le = min(lifeExp),
			  max_le = max(lifeExp)) 
#> # A tibble: 5 × 4
#>   continent mean_le min_le max_le
#>   <fct>       <dbl>  <dbl>  <dbl>
#> 1 Africa       48.9   23.6   76.4
#> 2 Americas     64.7   37.6   80.7
#> 3 Asia         60.1   28.8   82.6
#> 4 Europe       71.9   43.6   81.8
#> 5 Oceania      74.3   69.1   81.2
```



## `summarize()` vs `mutate()`

Both `mutate()` and `summarize()` can be used to create new variables in a dataset. It can be confusing to decide which one is appropriate. The key distinction is whether you want the output to have one row for each group or one row for each row in the original data frame.

When applied with grouped data, mutate creates new columns with as many rows as the original data frame.To do this, group_by() can be combined with mutate(), to make a new column of summary statistics (repeated many times) corresponding to the sub-grouping of interest.

When applied with grouped data, summarize creates a data frame with as many rows as groups. The output of `summarize()` is generally a smaller dataframe with a reduced representation of the input data.
