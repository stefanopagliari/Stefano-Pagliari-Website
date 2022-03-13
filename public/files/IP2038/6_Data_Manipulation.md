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


In order to access these and other functions that used in this section, you need to load `dplyr` at the beginning of the session. This can also be done by loading the `tidyverse`

```r

#load tidyverse
library(tidyverse)

#Alternatively load dplyr
library(dplyr)
```

In this section we will explore the use of these data manipulation toolsthe `gapminder` dataset. If starting from a new Rstudio session you should run the following code to load the `gapminder` data in your environment.


```r
#load gapminder
library(gapminder)
```


## The pipe (%>%) operator

Similarly to other functions included in R, dplyr functions can be launched by calling the name of the functions followed by a series of arguments specifying the input. In the case of most functions that are part of the `dplyr` package this takes the form of: 

- call the function (e.g. `select()`),
- within parentheses, first specify the name of the dataset we would like to manipulate (e.g. `gapminder`)
- and then list the input of the function (e.g. name of the columns to be selected)

As we will see more in detail in the examples below, it is possible to call the different dplyr functions using a different syntax that takes advantage of the *pipe* (`%>%`) operator to create code that is easier to write and to understand. 

This syntax looks like this:

- call the name of the dataset to be manipulated
- add the `%>%` operator (read '*and then*'). This will pass the value of the data to the next step
- call the function we want to use to manipulate our data

For instance the two lines of code below are equivalent ways to display the top lines in the gapminder dataset:


```r
head(gapminder)

gapminder %>% head

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
gap_narrow <- gapminder %>% 
  select(year, country, gdpPercap)
```


<table>
 <thead>
  <tr>
   <th style="text-align:right;"> year </th>
   <th style="text-align:left;"> country </th>
   <th style="text-align:right;"> gdpPercap </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1952 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 779.4453 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1957 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 820.8530 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1962 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 853.1007 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 836.1971 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 739.9811 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 786.1134 </td>
  </tr>
</tbody>
</table>



### Removing variables

Instead of specifying the variables that we want to retain, we can also use the `select` function to specify which variables we would like to drop. In order to do this, we need to add a minus (`-`) sign in front of the name of the variable we would like to remove.


```r

gap_narrow <- gapminder %>% 
  select(-gdpPercap, -pop)
```

The code below has removed the variable `gdpPercap` and `pop` from the gapminder dataset, while retaining the existing variables in their order

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> country </th>
   <th style="text-align:left;"> continent </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> lifeExp </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1952 </td>
   <td style="text-align:right;"> 28.801 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1957 </td>
   <td style="text-align:right;"> 30.332 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1962 </td>
   <td style="text-align:right;"> 31.997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:right;"> 34.020 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 36.088 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 38.438 </td>
  </tr>
</tbody>
</table>


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
 

## Create and modify variables

![](images/Mutate_Dplyr.png){width=80%}

The function `mutate()` can be used to add new variables, while preserving existing ones, or to overwrite existing variables with the same name. When we use this variable, the number of rows will not be affected

We can perform operations using the data in the existing columns. For instance, in the example below we first create a new variable called `gdp` by multiplying the values for each observations of two existing variables (gdp per capita and  population). Here the newly created column will contain the results of a mathematical operation, performed row by row.

The transformations can also involve data that is not found in the columns. For instance, in teh example below we create a new variable called `gdp_billions` where we  transforme the  scale of the `gdp` variables by dividing the value by one billion (10^9)



```r
gap_gdp <- gapminder %>%
	mutate(gdp = gdpPercap*pop) %>%
  mutate(gdp_billions = gdp/10^9) 

head(gap_gdp)
#> # A tibble: 6 × 8
#>   country  continent  year lifeExp     pop gdpPercap     gdp
#>   <fct>    <fct>     <int>   <dbl>   <int>     <dbl>   <dbl>
#> 1 Afghani… Asia       1952    28.8  8.43e6      779. 6.57e 9
#> 2 Afghani… Asia       1957    30.3  9.24e6      821. 7.59e 9
#> 3 Afghani… Asia       1962    32.0  1.03e7      853. 8.76e 9
#> 4 Afghani… Asia       1967    34.0  1.15e7      836. 9.65e 9
#> 5 Afghani… Asia       1972    36.1  1.31e7      740. 9.68e 9
#> 6 Afghani… Asia       1977    38.4  1.49e7      786. 1.17e10
#> # … with 1 more variable: gdp_billions <dbl>
```


### Logical filtering with ifelse()

While the examples below illustrate how `mutate()` can be used to perform the same transformations to all the rows of the same dataset, in some cases we would like to return a different transformation based on certain conditions.  The creation of new variable or the manipulation of existing variables can be associated with a logical test.

An easy way to do this is to combine the `mutate()` function with the `ifelse()` statement.
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

### Logical filtering with case_when()

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

## Summarise Across Groups

![](images/Summarise_Dplyr.png){width=80%}

Often we need to aggregate the observations in our data in order to gain some insights into the differences across different groups.The function `group_by()` allow us to group all the observations in our dataset that correspond to a particular value or set of values. 

For instance, calling the `group_by()` function for the `gapminder` dataset and specifying the `continent` variable will group all the observations where this variable takes the same value (e.g. all the observation for African countries,  Asian countries, European countries, etc...)

![](images/dplyr_groupby.png){width=80%}[source](https://swcarpentry.github.io/r-novice-gapminder/13-dplyr/)
Once we have specified how our data should be grouped, we can use this in conjunction with the `summarize()` function. This variable  allows us to create new variable(s) by applying transformations to variables in each of our groups (for example, for our continent-specific data frames).

![](images/dplyr_Groupby_summarize.png){width=80%}[source](https://swcarpentry.github.io/r-novice-gapminder/13-dplyr/)

The output is a new data frame reduced in size, with one row per group:


```r
gdp_by_continents <- gapminder %>%
    group_by(continent) %>%
    summarize(mean_gdpPercap = mean(gdpPercap))

gdp_by_continents
#> # A tibble: 5 × 2
#>   continent mean_gdpPercap
#>   <fct>              <dbl>
#> 1 Africa             2194.
#> 2 Americas           7136.
#> 3 Asia               7902.
#> 4 Europe            14469.
#> 5 Oceania           18622.
```

### Grouping by multiple variables

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


### Summarizing different variables

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



### Counting the number of observations by group

A common operation when analysing a dataset involves counting the number of observations that belongs to each group. We can achieve this by first grouping our data, creating a new variable using `summarize` and specify within this function the `n()` function to get the number of observations that below to that group.
The `dplyr` packages also includes a `tally()` function to count the observations within each group that could be used to replace `summarize(n())`




```r
gapminder %>%
    group_by(continent) %>%
    summarize(observation_num= n())
#> # A tibble: 5 × 2
#>   continent observation_num
#>   <fct>               <int>
#> 1 Africa                624
#> 2 Americas              300
#> 3 Asia                  396
#> 4 Europe                360
#> 5 Oceania                24
```




```r

gapminder %>%
    group_by(continent) %>%
    tally()
```

Alternatively we can achieve the same outcome by using the `count()` function. count() is a shortcut function that combines group_by() and summarize(), which is useful for counting ‘character data’, e.g. labels. We can also optionally sort the results in descending order by adding `sort=TRUE`:


```r
gapminder %>%
    count(continent, sort = TRUE)
```

### mutate() vs. summarize()

Both `mutate` or `summarize` can be used to create new variables in a data 
It can be confusing to decide which one is appropriate.
The key distinction is whether you want the output to have one row for each group or one row for each row in the original data frame.

When applied with grouped data, `mutate` creates new columns with as many rows as the original data frame.To do this, group_by() can be combined with mutate(), to make a new column of summary statistics (repeated many times) corresponding to the sub-grouping of interest. 

![](images/Group_by_summarise.png)

When applied with grouped data, `summarize` creates a data frame with as many rows as groups. The output of summarize() is generally a smaller dataframe with a reduced representation of the input data.



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



## Chaining different transformations with `%>%`

It is possible to link different data transformations using the pipe operator (`%>%`). In this case the result of a command is passed to the next one.

Once we’re happy with the output of this chain of functions, we can assign it to a new object in the environment.



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


