# (PART) Data Wrangling {-} 

# Data Wrangling with dplyr





Data wrangling - also called data cleaning, data remediation, or data munging - refers to a variety of processes designed to transform raw data into more readily used formats. 
Indeed, this is a key step when dealing with real-word datasets, since these often do not present the information in the precise way that is relevant to our analysis. Often most of the time in the process of analysing data is spent in manipualting and cleaning the data.

![](images/Data_Wrangling_From_Messy_To_Clean_Data_Management.jpg){width=80%}

## Dplyr

`Dplyr` is a package (part of the `tidyverse`) which includes a number of helpful functions that can be used to manipulate our data in order to transform it into a more helpful format.

![](images/Dplyr_Logo.png){width=80%}

In this section we will explore  some of the most commonly used `dplyr` functions for exploring and transforming data frames. The input to and the output from all these functions are dataframes. These include

1. `select()` to select variables in a dataframe
2. `rename()` to change the title of a variable
3. `filter()` to select a number of observations that meet certain criteria
4. `arrange()` to reorder the observations
5. `mutate()` to generate new variables or amend the existing ones
6. `group_by()` to divide the observations in groups
7. `summarize()` to generate new variables containing information about groups of observations
8. `count()` to count the number of observations with a given value
9. `join()` to merge data from different datasets


In order to access these and other functions that used in this section, you need to load `dplyr` at the beginning of the session. This can also be done by loading the `tidyverse`

```r

#load tidyverse
library(tidyverse)

#Alternatively load dplyr
library(dplyr)
```

In this section we will explore the use of these data manipulation tools on the `gapminder` dataset. If starting from a new Rstudio session you should run the following code to load the `gapminder` data in your environment.


```r
#load gapminder
library(gapminder)
```


## The pipe (%>%) operator

As we will see more in detail in the examples below, it is possible to call the different dplyr functions using a different syntax that takes advantage of the *pipe* (`%>%`) operator to create code that is easier to write and to understand. 

This syntax looks like this:

- call the name of the dataset to be manipulated
- add the `%>%` operator (read '*and then*'). This will pass the value of the data to the next step
- call the function we want to use to manipulate our data

For instance the two lines of code below are equivalent ways to display the top lines in the gapminder dataset:


```r
head(gapminder)

gapminder %>% head()

```

Both lines of code will return the same information:


```
#> # A tibble: 6 × 6
#>   country     continent  year lifeExp      pop gdpPercap
#>   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#> 1 Afghanistan Asia       1952    28.8  8425333      779.
#> 2 Afghanistan Asia       1957    30.3  9240934      821.
#> 3 Afghanistan Asia       1962    32.0 10267083      853.
#> 4 Afghanistan Asia       1967    34.0 11537966      836.
#> 5 Afghanistan Asia       1972    36.1 13079460      740.
#> 6 Afghanistan Asia       1977    38.4 14880372      786.
```

This syntax facilitate the chaining of different steps in our analysis. If we are to perform another transformation to the same data, we can add the `%>%` operator (read '*and then*') to pass this data to a next operation. This avoids the need to save the data from each step to a new or existing data frame.

## Chaining different operations

We don’t often use the various steps involved in wrangling our data in isolation. Instead, starting with our raw data, we often need to perform a variety of transformations to prepare the data for further analysis (e.g. making a plot, calculating summaries, fitting a statistical model, and so on). 


It is possible to link different data transformations using the pipe operator (`%>%`). Every time we see the %>% operator it means the following: take whatever is produced by the left hand expression and use it as an argument in the function on the right hand side.

Here are some examples illustrating how these different tools to manipulate the dataset can be chained.

### Example: Ranking European Countries by GDP

For instance, the code below takes the  gapminder dataset and manipulates this data in order to rank European countries by GDP in 2007


```r

gapminder %>% #take the dataset gap
  filter(year ==2007) %>%   #filter observations where year is equal to 2007
  filter(continent == "Europe") %>% #filter observations where variable continent is "Europe"
  mutate(GDP = pop * gdpPercap) %>% #create a new variable "GDP" which multiplies pop * gdpPercap
  arrange(desc(GDP)) #order the observations based on GDP
#> # A tibble: 30 × 7
#>    country  continent  year lifeExp    pop gdpPercap     GDP
#>    <fct>    <fct>     <int>   <dbl>  <int>     <dbl>   <dbl>
#>  1 Germany  Europe     2007    79.4 8.24e7    32170. 2.65e12
#>  2 United … Europe     2007    79.4 6.08e7    33203. 2.02e12
#>  3 France   Europe     2007    80.7 6.11e7    30470. 1.86e12
#>  4 Italy    Europe     2007    80.5 5.81e7    28570. 1.66e12
#>  5 Spain    Europe     2007    80.9 4.04e7    28821. 1.17e12
#>  6 Netherl… Europe     2007    79.8 1.66e7    36798. 6.10e11
#>  7 Turkey   Europe     2007    71.8 7.12e7     8458. 6.02e11
#>  8 Poland   Europe     2007    75.6 3.85e7    15390. 5.93e11
#>  9 Belgium  Europe     2007    79.4 1.04e7    33693. 3.50e11
#> 10 Sweden   Europe     2007    80.9 9.03e6    33860. 3.06e11
#> # … with 20 more rows
```

### Example: Evolution of Average Life Expectancy in Africa since 1960

The code below takes the  gapminder dataset and manipulates this data in order to calculate the evolution of the average life expectancy in Africa since 1960


```r
library(gapminder)
library(tidyverse)
		
# take the gapminder dataset (gap) and produce a dataset that includes the evolution in the average life expectancy in African countries since 1960
# 
gapminder %>%       #take the dataset gapminder
  filter(year >=1960) %>%  #filter observations where year is equal or higher than 1960
  group_by(continent, year) %>%  # group the data across different combinations of year and continent
	filter(continent == "Africa") %>% #filter observations where variable continent is "Africa"
  summarise(Average_Life_Expectancy = mean(lifeExp)) #create a variable Average_Life_Expectancy that captures the mean lifeExp in each continent in each year
#> # A tibble: 10 × 3
#> # Groups:   continent [1]
#>    continent  year Average_Life_Expectancy
#>    <fct>     <int>                   <dbl>
#>  1 Africa     1962                    43.3
#>  2 Africa     1967                    45.3
#>  3 Africa     1972                    47.5
#>  4 Africa     1977                    49.6
#>  5 Africa     1982                    51.6
#>  6 Africa     1987                    53.3
#>  7 Africa     1992                    53.6
#>  8 Africa     1997                    53.6
#>  9 Africa     2002                    53.3
#> 10 Africa     2007                    54.8
```
