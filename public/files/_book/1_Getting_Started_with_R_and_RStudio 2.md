# Getting Started with R and RStudio




      
 
## An Introduction to R

![](images/RStats_Logo.jpeg)
<br>

In this module, you will learn how to download, manipulate, and examine and visualize data in "R". R is a programming language designed for statistical computing that is widely used in academia and the data science community.

Unlike softwares like Excel or SPSS, R works by entering instructions (code) at a prompt. When you type an instruction and hit return, R interprets it and sends any resulting output back to the console. 
For instance, the interactive boxes below simulate how typing instructions in R works. You can click on "Run Code" below to see the result of your instructions

For instance, you can click on "Run Code" below to see what happens when you instruct R to calculate the square root of 9.


```
#> [1] 3
```

...or a more advanced example, see what happens when you create a plot to compare the evolution of the gdp per capita of the United Kingdom and Japan in the postwar era.


<img src="1_Getting_Started_with_R_and_RStudio_files/figure-html/fertility-1.png" width="672" />

The code-boxes in this tutorial allow you to see the output of running different instructions using R. You can edit the content of the code and click on "Run Code" to see the output. For instance, what happens if you replace "Japan" with France?

---

### Why  R?

- **R** is s free and open-source (unlike Stata, SPSS, and SASS)
- It is powerful and able to handle large datasets (more so than MS Excel)
- It has a shorter learning curve than other computing languages (like Python).
- It has a large and expanding user community building "packages" containing tools we can use to import and analyze data

---

### How will we learn Data Analysis with R

There is no assigned textbook for R for this module. Instead, the lectures, interactive online tutorials such as this one, and the weekly sessions in the computer lab will introduce you to all the main steps required to get started with doing data analysis in R and to complete the assignment for this module.
More specifically:
- The weekly lectures will provide you with an introduction to the different steps involved in the doing exploratory data analysis
- The online interactive tutorials will explain how these steps can be completed using R and provide interactive examples you can use
- The weekly computer labs will allow you to put in practice what we learnt with the help the tutorial leader



### Additional Resources
These two freely available e-books provide an excellent and accessible reference to respectively data analysis and data visualization using R.



:::: {style="display: grid; grid-template-columns: 40% 40%; grid-column-gap: 5%; "}

::: {}

+ Wickham, Hadley, and Garrett Grolemund (2017). "R for Data Science". O’Reilly. [link](https://r4ds.had.co.nz)

:::

::: {}
![](images/RFDS_cover.png){width=80%}

:::

::: {}

+ Healey, Kieran (2018), "Data Visualization. A Practical Introduction", Princeton University Press [link](https://socviz.co)

:::

::: {}



![](images/Healy_Data_Visualization_Cover.jpg){width=80%}

:::

::::


---


### Searching for Help

A key skill in learning to programme is to learn to search the internet. This is something that both novel programmers as well as experienced data analysts do routinely. It is important that you learn how to "look for help" on the internet. 

Two common approaches are:

+ Search what you are trying to achieve on a search engine, by specifying the language (R) or specific package (e.g. `tidyverse`, `ggplot`) you are using. For instance, you could search on Google "how to filter a dataframe in R" or "how to change the label colour in ggplot")
+ Often the first results that will be returned by a search engine are pages on Stackoverflow. This is a website where users post questions and receive advise. The most useful answer is usually voted up and found at the top. You can search this website for previously posted questions similar to yours or ask new questions. 

![](images/StackOverflow_Most_Asked_Questions.png){width=50%}
[source](https://i.imgur.com/AFH3v7h.png)

---


## RStudio and RStudio Cloud

The original interface is simple but difficult to use. Instead, in this module we will learn to use R from Rstudio.

**RStudio** is an integrated development environment (IDE) used to programme in R. In other words, RStudio is a software that helps you to write R code, examine data, produce graphs, present your findings.

![](images/RStudio_Image.png){width=80%}


---

### (Optional) Installing RStudio on your personal computer 


R and RStudio are free to download and use on your personal computer. In order to install RStudio on your computer you need to:

+ **Download and install R** (the programming language) from https://cran.r-project.org/. R is maintained by an international team of developers who make the language available through the web page of [The Comprehensive R Archive Network](http://cran.r-project.org/). The top of the web page provides three links for downloading R. Follow the link that describes your operating system: Windows, Mac, or Linux.
+ **Download and install RStudio Desktop** on your computer from https://rstudio.com/products/rstudio/download/. Once you’ve installed RStudio, you can open it like any other program on your computer—usually by clicking an icon on your desktop.

*Please note that this is not required for this module as we will use the browser-based version RStudio (RStudio Cloud)*

---


### RStudio Cloud
![](images/RStudio_Cloud_Logo.png){width=70%}

 
**RStudio Cloud** (<https://rstudio.cloud/>) is a lightweight, cloud-based version of RStudio that runs on a browser. City has paid for a module subscription, which will make it easier for you to access RStudio and complete the module.
You can switch over to a free individual RStudio Cloud plan once the module is completed.

---

### Getting Started in RStudio Cloud

- You should have received an email from RStudio Cloud that looks like this:

![](images/EmailRStudioCloud.png){width=70%}

- The email contains a link that will direct you towards the webpage where you can create your account. In order to do to:

+ You must use your City email address

![](images/RStudioCloudRegister.png){width=70%}

+ Choose a strong password
+ You must use your first and last names registered with City


After you have completed your registration, you can access RStudio Cloud from your browser by navigating to: https://rstudio.cloud/ using your browser. Here you will need to sign in using your city email and your new RStudio Cloud password. 

---

### RStudio Cloud Interface

![](images/RStudioCloudInterface.png){width=90%}

When you open RStudio Cloud, you will see different elements:

+ **Your Workspace**: here you can create your own projects as well as copy your own versions of the weekly worksheets
+ **Tutorials**: This folder contains code used in the tutorials of this module. You can run the code included in these projects but you cannot edit it. In order to edit it, you will need to create your own copy of the projects. Save this weekly project as a new file by clicking on "Save a Permanent Copy".

![](images/RStudioSavePermanent.png){width=90%}


### RStudio Working Environment 
When you open a project in RStudio Cloud, you will access the main working environment.

![](images/RStudio_Panels.png){width=90%}


This has has four main panels.

+ **Console**: where you can execute commands and see the results. Commands run here are temporary, and will be lost if you close R. 
+ **Code Editor**: where you work on different code files that can be saved.
+ **Environment/History**: this space shows the objects that you create and work with
+ **Files/Help/Packages**: in this space you can see your files, plots and graphs that you produce, R packages that you install, help with packages.
 

---


### Importing/Exporting files from RStudio Cloud

 
From the Files tab you can manage the files that are stored in your project folder. In particular, here you will find the commands to:

- **upload** a file from your computer to the project folder
- **delete** a file from  the project folder
- **rename** a file in your project folder
- download a file from the project folder to your computer (**more --> export **)

---


## Writing R Code

It is possible to execute commands by writing them in the Console on the bottom of the RStudio environment. However, if you close R, you lose this command history and thus the ability to replicate what you have done.

Just as we "save" files (like MS Word documents), so that we don't lose our work, in R you can save your R Scripts, so that you don't lose the various R commands that you have executed. A script is a series of commands that you can save, modify, and re-run whenever you like.

In RStudio, you will create, edit, and save scripts from the code editor panel

![](images/Code_Editor.png){width=90%}

---

### How to create a R Script

In order to create a new R Script:

- Go to the Code Editor panel and click on the "New File" button
- Select "R Script"
![](images/CreateRScript.png){width=90%}
 
 Once you have created a script, you can then type multiple instructions within different lines of your code, run, and save the script.

![](images/ExampleRScript.png){width=50%}
 

### How to Run a R Script

There are different ways to run a script. You can:

- Type the command in the Console 
- You can run your script line by clicking on a line and typing Command+Enter (Mac OS) or Control+Enter (Windows) to run the line, or hitting `Run` from the dropdown menu.
- To run the whole script, type Ctrl+Shift+Enter on Windows, Command+Shift+Enter.
- You can also process your script by clicking on the "Run" icon at the top of the script editor. If a line is selected, this will run only that line. Otherwise, the entire script will be compiled.

![](images/RStudio_Run_Script.png){width=90%}

---

### How to Save a R Script

In order to save a R script you can:

- type Control + S (panels) or Command + S (Mac OS)
- Click the "save" button
- By default the script will be saved in the folder of your project.



---

### Commenting your code

 R code can be hard to read. If someone else tries to read it (or you later on), it might be hard to figure out what you have done. Adding comments make code easier to read. 

In R you can insert a comment in your script by placing `#` at the beginning of your code. 
 
See this example:


```
#> [1] TRUE
```

It is also possible to place a comment (`#`) at the end of a line of code.

![](images/RCommentEndLine.png){width=80%}





