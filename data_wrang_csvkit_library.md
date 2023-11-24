**Data Wrangling Using Command Prompt**

In this project, we want to use the bash command prompt to first combine three large data sets and then filter some rows. Some datasets are too large to load into Python, so looking at them or transforming them beforehand can be useful. Even with smaller ones, it's quicker to explore them using the command line instead of Python. Tasks like combining datasets are also faster when done on the command line. 

First, checking the current working directory:

Command: **pwd**

<img width="277" alt="image" src="https://github.com/sajadahmadia/command_prompt_and_git/assets/61582647/c16d0b76-8785-45da-aee0-4304e4f4d1b1">


Changing the current directory to the directory containing raw data:

Command: **cd “/Users/sajad/Downloads/housing\_raw\_data”**

Checking the files in this directory and their access levels:

As we can see, there are 3 files for different years(2005,2007,2013). These are large csv files and we want to stack them without loading them in Python notebooks. 

First, let’s find out how many rows each file has.

Command: **wc -l Hud\_2007.csv**

The Hud\_2005, 2007, and 2013 contain 46854, 42730, and 64536 rows in order(including the header row).

Let’s see the first 5 rows of the Hud\_2005 file. 

Command: head -5 Hud\_2005.csv


We want to Merge Hud\_2005.csv, Hud\_2007.csv, and Hud\_2013.csv in that order into one file(named Combined\_hud.csv). additionally, we want to add one column that shows the origin data source based on the year of that data source. From now, we use the **Csvkit library**. 

Command: **csvstack -n year -g 2005,2007,2013 Hud\_2005.csv Hud\_2007.csv Hud\_2013.csv > Combined\_hud.csv**


Showing the first 5 rows:

Command: **head -5 Combined\_hud.csv**


For a better presentation of our data, we use csvlook and piping in combination with each other. 

Command: **head -10 Combined\_hud.csv | csvlook**


Finding the column names and their corresponding index.

Command: **csvcut -n Combined\_hud.csv**

The second column is AGE1. We show the first 10 values of this column:

Interesting! We see -9 as one of the age values, which is not possible. The min value of age should be 0 to make sense. 

We can also find the mean of all columns.

Command: **csvstat --mean Combined\_hud.csv**

Now, we dive deeper into the details of the AGE1 column.

Command**: csvcut -c 2 Combined\_hud.csv | csvstat**

We have 11553 rows whose age value is -9. In this step, we show the first 10 rows that have such a value in their age1 column.

Command: **csvgrep -c 2 -m -9 Combined\_hud.csv | head -10 | csvlook**


In the final step, we filter out these problematic rows from the dataset since they have data quality issues.  We use the -i flag in combination with the csvgrep command to select inverse rows. We output our results into a new file in CSV format(positive\_ages\_only.csv). 

Command: **csvgrep -c 2 -m -9 -i Combined\_hud.csv > positive\_ages\_only.csv**


Checking our final output file:

AGE1 is the second column.

We check the stat of the AGE1 column to find out whether the min value is still -9 or not:

Command: **csvcut -c 2 positive\_ages\_only.csv | csvstat**


As we can see, there are no rows with -9 in the AGE1 column anymore. Also, the number or rows has decreased to 142564. 

Doing such an operation using Python would be more time-consuming. But by using the command prompt(bash here) we did it easily and quickly. 


