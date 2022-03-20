# Working with Variables



## Introduction

This chapter will explore different verbs from the `dplyr` package that can be used to operate on the variables (i.e. the columns) of a data frame:

- The `select()` function selects a subset of variables to retain
- The `mutate()` function creates new variables from preexisting ones and retains the original variables.
- The `rename()` function renames one or more variables while keeping the remaining variable names unchanged.


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
```


## Selecting Variables

![](images/Select_Dplyr.png){width=80%}

While many real-world datasets include a large number of variables (columns), sometimes we need to include only a subset of these in our analysis.
The `select()` function can be used to select variables in a data frame based on their name.
In order to call the subset the variables we need to:

- call the function `select()`,
- within parentheses, first specify the name of the dataset
- and then list the name variables (without quotation marks and separated by a comma) we would like to retain in the order in which they should appear 

![](images/dplyr-select1.png){width=60%}

In the example below we first load the `gapminder` dataset and then  select the `year`, `country`, and `gdpPercap` variables. This narrower dataset is then stored in a different object.



```r
library(tidyverse)

# Load gapmidner data
library(gapminder)

#Select three variables and place them in a new object 
gap_narrow <- select(gapminder, year, country, gdpPercap)

#print the first rows of the gap_narrow dataset
head(gap_narrow )
#> # A tibble: 6 × 3
#>    year country     gdpPercap
#>   <int> <fct>           <dbl>
#> 1  1952 Afghanistan      779.
#> 2  1957 Afghanistan      821.
#> 3  1962 Afghanistan      853.
#> 4  1967 Afghanistan      836.
#> 5  1972 Afghanistan      740.
#> 6  1977 Afghanistan      786.
```


Alternatively it is possible to insert this function in a chain of transformation using the pipe symbol `%>%`. This includes the following steps:

- summon the dataframe 
- pass it on to the next step using the pipe symbol `%>%`
- use the `select()` function to choose which variables we want, listing these within parentheses.



```r
# Alternative approach with %>% to select three variables and place them in a new object 
gapminder %>% 
  select(year, country, gdpPercap)
#> # A tibble: 1,704 × 3
#>     year country     gdpPercap
#>    <int> <fct>           <dbl>
#>  1  1952 Afghanistan      779.
#>  2  1957 Afghanistan      821.
#>  3  1962 Afghanistan      853.
#>  4  1967 Afghanistan      836.
#>  5  1972 Afghanistan      740.
#>  6  1977 Afghanistan      786.
#>  7  1982 Afghanistan      978.
#>  8  1987 Afghanistan      852.
#>  9  1992 Afghanistan      649.
#> 10  1997 Afghanistan      635.
#> # … with 1,694 more rows
```


### Removing variables

Instead of specifying the variables that we want to retain, we can also use the `select` function to specify which variables we would like to drop. In order to do this, we need to add a minus (`-`) sign in front of the name of the variable we would like to remove.
The code below has removed the variable `gdpPercap` and `pop` from the gapminder dataset, while retaining the existing variables in their order


```r

gapminder %>% 
  select(-gdpPercap, -pop)
#> # A tibble: 1,704 × 4
#>    country     continent  year lifeExp
#>    <fct>       <fct>     <int>   <dbl>
#>  1 Afghanistan Asia       1952    28.8
#>  2 Afghanistan Asia       1957    30.3
#>  3 Afghanistan Asia       1962    32.0
#>  4 Afghanistan Asia       1967    34.0
#>  5 Afghanistan Asia       1972    36.1
#>  6 Afghanistan Asia       1977    38.4
#>  7 Afghanistan Asia       1982    39.9
#>  8 Afghanistan Asia       1987    40.8
#>  9 Afghanistan Asia       1992    41.7
#> 10 Afghanistan Asia       1997    41.8
#> # … with 1,694 more rows
```


### Selecting a Range of Variables

In the case we want to retain a range of variable, it is possible to list the first and last variable we want to retain from the existing dataset, separated by `:`.


```r

gap_narrow <- gapminder %>% 
  select(year:gdpPercap)
  
```


### Selection Helpers

It is possible to use **selection helpers** to select all variables whose name includes certain characters. Examples of functions that can be called inside `select()` include:


-   [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html): select all variables whose name starts with a prefix.
-   [`ends_with()`](https://tidyselect.r-lib.org/reference/starts_with.html): select all variables whose name ends with a suffix.
-   [`contains()`](https://tidyselect.r-lib.org/reference/starts_with.html): select all variables whose name contains a literal string.
-   [`matches()`](https://tidyselect.r-lib.org/reference/starts_with.html): Matches a regular expression.

For instance, in this example we take a pre-loaded dataset named `iris` and select  those variable whose name starts with "Petal" and ends with "Width"


```r

new_iris <- iris %>% select(Species, starts_with("Petal") & ends_with("Width"))

head(new_iris)
#>   Species Petal.Width
#> 1  setosa         0.2
#> 2  setosa         0.2
#> 3  setosa         0.2
#> 4  setosa         0.2
#> 5  setosa         0.2
#> 6  setosa         0.4
```



## Rename Variables

It is often the case that the name of the variables in a dataset are not clearly informative of the content of the variable.
We can change the name of columns in our dataset with the `rename()` function. This function takes the syntax `"new_name" = "old_name"` within parentheses.
When using this function,  column names are changed but rows and the column order are not affected.


```r
#load gapminder data
library(gapminder)

gap_renamed <- rename( gapminder, "population" = "pop")

#List the name of the columns
colnames(gap_renamed) 
#> [1] "country"    "continent"  "year"       "lifeExp"   
#> [5] "population" "gdpPercap"
```

Similarly to other functions that are part of dplyr, it is possible to use the pipe `%>%` operator to chain the function.


```r
gap_renamed <- gapminder %>%
  rename("population" = pop)

#Print the name of the variables
colnames(gap_renamed)
#> [1] "country"    "continent"  "year"       "lifeExp"   
#> [5] "population" "gdpPercap"
```

## Create variables with `mutate`

The data that we find in a dataset is often not in the right format. For instance, in many cases there may be a mismatch between the data available in a variable and the concept we are interested in exploring. In other cases, the data as made available to us is not in a format that can easily be visualized or analyzed. In all these cases, we may need to take steps create new variables or transform the values in one or more variables of our dataset.

There are two main ways to transform the values included in a variable in R.

### Applying function to specific column (using $)

First, in base R (that is, R without loading any additional package) it is possible to target the values of a specific variables in a dataset or create a new one by using the expression `Dataframe$VariableName`.

For instance, the expression below will create a new variable to the gapminder dataset called `gdpPercapRound` whose value is the result of applying the `round()` function to all the values in the `gdpPercap` variable of the same dataset


```r
library(gapminder)
gapminder$gdpPercapRound <- round(gapminder$gdpPercap)
```

### Using `mutate()` from the `dplyr` package


```r
library(tidyverse)
gapminder <- gapminder %>%
  mutate(gdpPercapRound = round(gdpPercap))
```



![](images/Mutate_Dplyr.png){width=80%}

The function `mutate()` from the `dplyr` package can be used to add new variables or to overwrite existing variables with the same name. 

- In order to create a new variable, we need to assign the results of the mutate function to a new variable name
- In order to overwrite an existing variable, we need to assign the results of the mutate function to this variable

### Types of transformations

A number of different transformations of our data are possible using the `mutate()` function.

#### Arithmetical Operations

In the case our variable is a numerical one, we can specify a number of mathematical transformations that will then be applied to each value of the same variable.
For instance, in the example below we create a new variable called `pop_million` where we  transform the  scale of the `pop` variable by dividing the value by one million.



```r
gapminder %>%
  mutate(pop_million = pop/1000000)  #create variable to express population of a country in millions 
#> # A tibble: 1,704 × 8
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # … with 1,694 more rows, and 2 more variables:
#> #   gdpPercapRound <dbl>, pop_million <dbl>
```


#### Create new variable by from existing variables
We can create new variables or amend the value of an existing one on the basis of the data found in other existing variables in our dataset.

 For instance, in the example below we first create a new variable called `gdp` by multiplying the values for each observations of two existing variables (gdp per capita and  population). Here the newly created column will contain the results of a mathematical operation, performed row by row.


```r
# Create new variable by from existing variables
gapminder %>%
  mutate(gdp = gdpPercap * pop) #create "gdp" variable multiplying gdpPercap by population
#> # A tibble: 1,704 × 8
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # … with 1,694 more rows, and 2 more variables:
#> #   gdpPercapRound <dbl>, gdp <dbl>
```


### Use function to transform a variable
It is possible to transform a variable by applying a function, using the following format:

```r

# Round numerical variable
gapminder %>%
  mutate(gdpPercap_Round =
           round(gdpPercap)) #remove decimals from GDP Per capita
#> # A tibble: 1,704 × 8
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # … with 1,694 more rows, and 2 more variables:
#> #   gdpPercapRound <dbl>, gdpPercap_Round <dbl>
```





## Logical filtering 
While the examples below illustrate how `mutate()` can be used to perform the same transformations to all the rows of the same dataset, in some cases we would like to return a different transformation based on certain conditions.  The creation of new variable or the manipulation of existing variables can be associated with a logical test.

### Recode()

The recode function from the `dplyr` package can be used to change or recode specific values in a variable.

For instance, in the example below we take the *continent* variable in the gapminder dataset and replaces all the values in this variable from "Asia" to "Asian Continent". All the other observations where the value of the same variable is different from Asia are left untouched.



```r
gapminder %>%
  mutate(continent = recode(continent, "Asia" = "Asian Continent"))
#> # A tibble: 1,704 × 7
#>    country     continent      year lifeExp     pop gdpPercap
#>    <fct>       <fct>         <int>   <dbl>   <int>     <dbl>
#>  1 Afghanistan Asian Contin…  1952    28.8  8.43e6      779.
#>  2 Afghanistan Asian Contin…  1957    30.3  9.24e6      821.
#>  3 Afghanistan Asian Contin…  1962    32.0  1.03e7      853.
#>  4 Afghanistan Asian Contin…  1967    34.0  1.15e7      836.
#>  5 Afghanistan Asian Contin…  1972    36.1  1.31e7      740.
#>  6 Afghanistan Asian Contin…  1977    38.4  1.49e7      786.
#>  7 Afghanistan Asian Contin…  1982    39.9  1.29e7      978.
#>  8 Afghanistan Asian Contin…  1987    40.8  1.39e7      852.
#>  9 Afghanistan Asian Contin…  1992    41.7  1.63e7      649.
#> 10 Afghanistan Asian Contin…  1997    41.8  2.22e7      635.
#> # … with 1,694 more rows, and 1 more variable:
#> #   gdpPercapRound <dbl>
```


### ifelse()

Alternatively, we can  combine the `mutate()` function with the `ifelse()` statement.
`ifelse()` is a function that tests each value in a column of data for a particular condition (a logical test), and returns one answer when the condition==TRUE, and another when the condition==FALSE.

`ifelse()` statements takes three commands `ifelse(test_expression, x, y)`, where:

- `test_expression` is a statement that returns a  true or false (logic) outcome (e.g. `continent == "Africa"`, `gdpPercap> 1000`)
- `x` is  the value returned if `test_expression` is `TRUE`
- `y` is  the value returned it `test_expression` is `FALSE`.

For instance, below we create a new variable called `development` which takes the value `developed country` if the GDP per capita is above $25,000, and the value `developing country` if the GDP per capita is below or equal to the same threshold. Remember that we need two closing brackets, one for the `mutate()` function, and one for the `ifelse()` inside it.


```r
gap_dev <- gapminder %>%
	mutate(development = ifelse(gdpPercap> 25000, 
								"developed country", 
								"developing country")) %>%
  select(country, gdpPercap, development)

head(gap_dev)
#> # A tibble: 6 × 3
#>   country     gdpPercap development       
#>   <fct>           <dbl> <chr>             
#> 1 Afghanistan      779. developing country
#> 2 Afghanistan      821. developing country
#> 3 Afghanistan      853. developing country
#> 4 Afghanistan      836. developing country
#> 5 Afghanistan      740. developing country
#> 6 Afghanistan      786. developing country
```

### case_when()

At times we want to create a label column that tests multiple conditions. One option is to put multiple ifelse() commands inside each other (`ifelse(test1, valueiftest3true, ifelse(test2, valueiftest2true, ifelse(test 3, valueiftest3true)))`). This however would be difficult to read.

An alternative option is to use the function `case_when()`.

This command takes multiple conditions and tests them in order. 
case_when() takes a conditional command in the same format as the first command in ifelse(), however only the action for the TRUE condition is given, separated with a tilde ~.
The catch-all command for rows that do not satisfy any other conditions, is given at the end.

Let’s use case_when() to make a label for the level of development of countries, distinguishing between low-income countries, middle-income countries, and advanced economies, and assign observations in the gapminder dataset to these categories based on their level of GDP per capita.  


```r
gap_dev <- gapminder %>%
	mutate(development = case_when(
	  gdpPercap< 25000 ~ "low-income country",
	  gdpPercap>= 25000 & gdpPercap<50000  ~ "middle- income country", 
	  gdpPercap>= 50000  ~ "advanced economy",
	  TRUE ~ 'other')) %>%
  select(country, gdpPercap, development)

head(gap_dev)
#> # A tibble: 6 × 3
#>   country     gdpPercap development       
#>   <fct>           <dbl> <chr>             
#> 1 Afghanistan      779. low-income country
#> 2 Afghanistan      821. low-income country
#> 3 Afghanistan      853. low-income country
#> 4 Afghanistan      836. low-income country
#> 5 Afghanistan      740. low-income country
#> 6 Afghanistan      786. low-income country
```

Because we have captured the possible numerical values of GDP per capita, we don’t expect the catch-all output, “other”, to be present in the new variable. 
 

