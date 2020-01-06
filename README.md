
# Visualizing Time Series Data - Lab

## Introduction

As mentioned in an earlier lesson, time series visualizations play an important role in the analysis of time series data. Time series are often plotted to allow data diagnostics to identify temporal structures. 

In this lab, we'll cover main techniques for visualizing time series data in Python using the minimum daily temperatures over 10 years (1981-1990) in the city of Melbourne, Australia. The units are in degrees Celsius and there are 3,650 observations. The [source](https://datamarket.com/data/set/2324/daily-minimum-temperatures-in-melbourne-australia-1981-1990) of the data is credited to the Australian Bureau of Meteorology.

## Objectives

You will be able to:

- Explore the temporal structure of time series with line plots 
- Construct and interpret time series histogram and density plots 
- Create a time series heat map

## Let's get started! 

Run the cell below to import the necessary classes and libraries: 


```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
from pandas import Series
import matplotlib.pyplot as plt
```

- Import the dataset which is available in `'min_temp.csv'` 
- Print the first five rows of the data 


```python
# Load the data from 'min_temp.csv'
temp_data = None

# Print the first five rows

```

- Make sure the `'Date'` column is treated as an actual date by Python (notice how the date is formatted before attempting to changing the data type) 
- Set the index of `temp_data` to this `'Date'` column 


```python
# Change the data type of the 'Date' column


# Set the index to the 'Date' column

```

Print the index of `temp_data`. 


```python
# Print the index of the data

```

## Time Series line plot

Create a time series line plot for `temp_data`


```python
# Draw a line plot using temp_data 

```

Some distinguishable patterns appear when we plot the data. Here we can see a pattern in our time series, i.e., temperature values are maximum at the beginning of each year and minimum at around the 6th month. Yes, we are talking about Australia here so this is normal. This cyclical pattern is known as seasonality and will be covered in later labs. 

## Time Series dot plot

For a dense time series, as seen above, you may want to change the style of a line plot for a more refined visualization with a higher resolution of events. One way could be to change the continuous line to dots, each representing one entry in the time series. 


```python
# Use dots instead on a continuous line and redraw the time series 

```

This plot helps us identify clear outliers in certain years!

## Grouping and Visualizing time series data

Now, let's group the data by year and create a line plot for each *year* for direct comparison. You can regroup data per year using Pandas' `grouper()` function in conjunction with the `.groupby()` method.  


```python
# Use pandas grouper to group values using annual frequency
year_groups = None
```

Rearrange the data so you can create subplots for each year. 


```python
# Create a new DataFrame and store yearly values in columns 
temp_annual = None


# Plot the yearly groups as subplots

```

You can see 10 subplots corresponding to the number of columns in your new DataFrame. Each plot is 365 days in length following the annual frequency.

Now, plot all the years on the same graph instead of different subplots. 


```python
# Plot all years on the same graph

```

We can see in both plots above that due to the dense nature of time-series (365 values) and a high correlation between the values in different years (i.e. similar temperature values for each year), we can not clearly identify any differences in these groups. However, if you try this on the CO2 dataset used in the last lab, you should be able to see a clear trend showing an increase every year. 

## Time Series Histogram

Create a histogram for your data.


```python
# Plot a histogram of the temperature dataset

```

The plot shows a distribution that looks strongly Gaussian/Normal. 


## Time Series Density Plots

Create a time series density plot


```python
# Plot a density plot for temperature dataset

```

The density plot provides a clearer summary of the distribution of observations. We can see that perhaps the distribution is a little asymmetrical and perhaps a little pointy to be Gaussian.

## Time Series Box and Whisker Plots by Interval

Let's use our groups by years to plot a box and whisker plot for each year for direct comparison using the `.boxplot()` method. 


```python
# Generate a box and whiskers plot for temp_annual
```

In our plot above, we don't see much difference in the mean temperature over years, however, we can spot some outliers showing extremely cold or hot days. 

We can also plot distribution across months within each year. Perform the following tasks to achieve this: 

- Extract observations for the year 1990 only, the last year in the dataset 
- Group observations by month, and add each month to a new DataFrame as a column 
- Create 12 box and whisker plots, one for each month of 1990 


```python
# Use temp_data to extract values for 1990
yr_1990 = None

# Group observations by month
groups_monthly = None

# Add each month to DataFrame as a column
months_1990 = None
months_df = None

# Set the column names for each month i.e. 1,2,3, .., 12
months_df.columns = None

# Plot the box and whiskers plot for each month 

```

We see 12 box and whisker plots, showing the significant change in the distribution of minimum temperatures across the months of the year from the Southern Hemisphere summer in January to the Southern Hemisphere winter in the middle of the year, and back to summer again.

## Time Series Heat Maps

Let's create a heat map of the minimum daily temperatures data. 

- Rotate (transpose) the `temp_annual` DataFrame so that each row represents one year and each column one day  
- Use the `matshow()` function to draw a heat map for transposed yearly matrix 


```python
# Transpose the yearly group DataFrame
year_matrix = None

# Draw a heatmap with matshow()

```

We can now see that the plot shows the cooler minimum temperatures in the middle days of the years and the warmer minimum temperatures in the start and ends of the years, and all the fading and complexity in between.

Following this intuition, let's draw another heatmap comparing the months of the year in 1990. Each column represents one month, with rows representing the days of the month from 1 to 31.


```python
# Draw a heatmap comparing the months of the year in 1990 

```

The plot shows the same macro trend seen for each year on the zoomed level of month-to-month. We can also see some white patches at the bottom of the plot. This is missing data for those months that have fewer than 31 days, with February being quite an outlier with 28 days in 1990.

## Summary 

In this lab, you learned how to explore and better understand a time-series dataset using Pandas. You also learned how to explore the temporal relationships with line, scatter, box and whisper plots, histograms, density plots, and heat maps. 
