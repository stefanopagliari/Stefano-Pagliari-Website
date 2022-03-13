# Inspecting a Dataset




## Peeking at the Data

Once we have loaded a dataset in our working environment in RStudio, can can start exploring this data.
Different functions are also available to peek at the data itself from within R Studio. 

The `head` function displays the first 6 rows of the dataframe.


```r
library(gapminder)

#Create the gap dataset from gapminder function
gap <- gapminder

# Print in the console the initial values for each column
head(gap)
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


It is possible to view the entire dataset from within RStudio using the command `View` followed by the name of the dataframe or by clicking on the data name of the dataset in the Environment panel (top-right).



```r
View(gap)
```

This will open the entire dataset in a separate tab. This viewer allows you to filter the value in a specific column as well as reorder them.

![](images/RStudio-Viewer.png){width=80%}



## Dimensions of a dataframe

One of the first elements we may want to know about a dataset is the number of observations (rows) and variables (columns). These can be computed using the function `dim()`, adding the name of the object where the dataset is stored within parentheses. For instance, let's look at the dimensions of the Gapminder dataset.



```r

library(gapminder)

#Create the gap dataset from gapminder function
gap_df <- gapminder

#List the number of rows and variables using the
dim(gap_df)
#> [1] 1704    6
```

The results of this function show that the dataframe has 6 variables (columns) and 1704 obvervations (rows),

The dimensions of a dataframe are also visible in the RStudio 'environment' panel (top-right), next to the name of the object.

![](images/R_Dimensions_Dataframe.png){width=80%}

## Exploring the variables

### Name of the Variables

We can extract the name of the columns (variables) by using the function `colnames()`


```r

library(gapminder)

#Create the gap dataset from gapminder function
gap_df <- gapminder

# Print in the console the name of the variables
colnames(gap_df)
#> [1] "country"   "continent" "year"      "lifeExp"  
#> [5] "pop"       "gdpPercap"
```


### Type of Variables

Knowing what type of variables are present in a dataset is crucial to be able analyze them. Many of the summaries, models, functions, and pictures we can generate in R depend on knowing what kind of variable we’re using. The wrong kind of variable used with a command or function will often elicit an error message.

The function `str`  can be used to investigate the type (or class) of all the variables in a given dataframe.



```r
library(gapminder)

#Create the gap dataset from gapminder function
gap <- gapminder

#Check type of variable lifeExp
str(gap)
#> tibble [1,704 × 6] (S3: tbl_df/tbl/data.frame)
#>  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
#>  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
#>  $ year     : int [1:1704] 1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
#>  $ lifeExp  : num [1:1704] 28.8 30.3 32 34 36.1 ...
#>  $ pop      : int [1:1704] 8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
#>  $ gdpPercap: num [1:1704] 779 821 853 836 740 ...
```

In this case, we can see that the 6 columns in the gap dataset take different types:

- The variable gdpPercap (*GDP Per Capita*) and lifeExp (*life expectancy*) are assigned to the class "numeric" since these are numeric continuous variables.
- The variable `year` and `pop` are numerical discrete variables which are assigned to an element of class **integer**, a different numeric class which accomodates values that not a fraction. 
- The variable `country` and `continent` are categorical variables and are imported in this case as class of **factor** Factors are used to represent categorical data and can be ordered or unordered. In other cases unordered categorical variables can be stored as **characters**.
  
  
A similar breakdown of the variables types combined with a peak at the initial values can be obtained using the `glimpse()` function (part of the tidyverse).

  

### Summary of the Variables

In order to better understand our data, we may also want to understand the distribution and type of variables. The `summary()` function provides quick summary statistics of a all the variables in our dataframe.

The command will return for each numerical variable the minimum and maximum value that the variable takes, as well as the median, mean, and 1/3rd quartile.

In the case of strings, the command will return the frequency of the most common values found in this variable



```r
library(gapminder)

#Create the gap dataset from gapminder function
gap <- gapminder

#Print to the console summary statistics of teh main variable
summary(gap)
#>         country        continent        year     
#>  Afghanistan:  12   Africa  :624   Min.   :1952  
#>  Albania    :  12   Americas:300   1st Qu.:1966  
#>  Algeria    :  12   Asia    :396   Median :1980  
#>  Angola     :  12   Europe  :360   Mean   :1980  
#>  Argentina  :  12   Oceania : 24   3rd Qu.:1993  
#>  Australia  :  12                  Max.   :2007  
#>  (Other)    :1632                                
#>     lifeExp           pop              gdpPercap       
#>  Min.   :23.60   Min.   :6.001e+04   Min.   :   241.2  
#>  1st Qu.:48.20   1st Qu.:2.794e+06   1st Qu.:  1202.1  
#>  Median :60.71   Median :7.024e+06   Median :  3531.8  
#>  Mean   :59.47   Mean   :2.960e+07   Mean   :  7215.3  
#>  3rd Qu.:70.85   3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
#>  Max.   :82.60   Max.   :1.319e+09   Max.   :113523.1  
#> 
```

  

