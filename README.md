
# Visualizing Time Series Data - Lab

## Introduction

As mentioned in the lecture, time series visualizations play an important role in the analysis of time series data. Time series are often plotted to allow data diagnostics to identify temporal structures. 

In this lab, we'll cover main techniques for visualizing timeseries data in Python using the minimum daily temperatures over 10 years (1981-1990) in the city Melbourne, Australia again. You might remember from the lesson that the units are in degrees Celsius and there are 3,650 observations. The [source](https://datamarket.com/data/set/2324/daily-minimum-temperatures-in-melbourne-australia-1981-1990) of the data is credited as the Australian Bureau of Meteorology.

## Objectives: 

* Explore the temporal structure of time series with line plots
* Understand and describe the distribution of observations using histograms and density plots
* Measure the change in distribution over intervals using box and whisker plots and heat map plots

## Let's get started!

Import the necessary libraries


```python
import warnings
warnings.filterwarnings('ignore')

# Load required libraries
import pandas as pd
from pandas import Series
import matplotlib.pyplot as plt
```


```python
# Load the data from min_temp.csv and check the index
temp_data = pd.read_csv("min_temp.csv")
temp_data.index
```




    RangeIndex(start=0, stop=3650, step=1)



Check the info. Next, make sure the index is the timestamp.


```python
temp_data.info()
temp_data.Date = pd.to_datetime(temp_data.Date, format='%d/%m/%y')
temp_data.set_index('Date', inplace = True)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3650 entries, 0 to 3649
    Data columns (total 2 columns):
    Date         3650 non-null object
    Daily_min    3650 non-null float64
    dtypes: float64(1), object(1)
    memory usage: 57.1+ KB


Check the info again


```python
temp_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 3650 entries, 1981-01-01 to 1990-12-31
    Data columns (total 1 columns):
    Daily_min    3650 non-null float64
    dtypes: float64(1)
    memory usage: 57.0 KB


## Time Series line plot

Create a time series line plot for `temp_data`


```python
# Draw a line plot using temp_data
temp_data.plot(figsize = (22,8))
plt.show()
```


![png](index_files/index_9_0.png)


Some distinguishable patterns appear when we plot the data. Here we can see a pattern in our timeseries i.e. temperature values are maximum at the beginnig of each year and minimum at around the 6th month. Yes, we are talking about Australia here so this is normal. This cyclical pattern is known as seasonality and will be covered in later labs. 

## Time Series dot plot
For a dense timeseries, as seen above, you may want to change the style of a line plot for a more refined visualization with a higher resolution of events. One way could be to change the continuous line to dots, each representing one entry in the time series. this can be achieved by `style` parameter of the line plot. lets pass `style='b.` as an argument to `.plot()` function


```python
# Use dots instead on a continuous line and redraw the timeseries. 
temp_data.plot(figsize = (22,8), style = 'b.')
plt.show()
```


![png](index_files/index_11_0.png)


This plot helps us identify clear outliers in certain years!

## Grouping and Visualizing time series data

Now, let's group data by year and create a line plot for each year for direct comparison.
You'll regroup data per year using `Pandas.grouper()`. 

- Import pandas grouper and use it to group values by year.
- Rearrange the data so you can create subplots for each year.


```python
# Use pandas grouper to group values using annual frequency
year_groups = temp_data.groupby(pd.Grouper(freq ='A'))
```


```python
#Create a new DataFrame and store yearly values in columns 
temp_annual = pd.DataFrame()
```


```python
for yr, group in year_groups:
    temp_annual[yr.year] = group.values.ravel()

# Plot the yearly groups as subplots
temp_annual.plot(figsize = (22,15), subplots=True, legend=True)
plt.show()
```


![png](index_files/index_15_0.png)


You can see 10 subplots correspoding to the number of columns in your new DataFrame. Each plot is 365 days in length following the annual frequency.

Now, plot the same plots in an overlapping way.


```python
# Plot overlapping yearly groups 

temp_annual.plot(figsize = (18,10), subplots=False, legend=True)
plt.show()
```


![png](index_files/index_17_0.png)


We can see in both plots above that due the dense nature of time-series (365 values) and a high correlation between the values in different years (i.e. similar temperature values for each year), we can not clearly identify any differences in these groups. However, if you try this on the CO2 dataset used in the last lab, you should be able to see a clear trend showing an increase every year. 

## Time Series Histogram

Create a histogram for your data.


```python
# Plot a histogram of the temperature dataset
temp_data.hist(figsize = (12,6))
plt.show()
```


![png](index_files/index_20_0.png)


The plot shows a distribution that looks strongly Gaussian/Normal. The plotting function automatically selects the size of the bins based on the spread of values in the data.

## Time Series Density Plots
Create a time series density plot


```python
# Plot a density plot for temperature dataset
temp_data.plot(kind='kde', figsize = (12,6))
plt.show()
```


![png](index_files/index_22_0.png)


We can see that density plot provides a clearer summary of the distribution of observations. We can see that perhaps the distribution is a little asymmetrical and perhaps a little pointy to be Gaussian.

## Time Series Box and Whisker Plots by Interval

Let's use our groups by years to plot a box and whisker plot for each year for direct comparison using `boxplot()`.


```python
# Generate a box and whiskers plot for temp_annual dataframe
temp_annual.boxplot(figsize = (15,8))
plt.show()
```


![png](index_files/index_25_0.png)


In our plot above, we dont see much difference in the mean temperature over years, however, we can spot some outliers showing extremely cold or hot days. 

We can also plot distribution across months within each year. Perform following tasks to achieve this. 
1. Extract observations for year 1990 only, the last year in the dataset.

2. Group observations by month, and add each month to a new DataFrame as a column.

3. Create 12 box and whisker plots, one for each month of 1990.


```python
# Use temp Dataset to extract values for 1990
yr_1990 = temp_data['1990']
groups_monthly = yr_1990.groupby(pd.Grouper(freq ='M'))

# Add each month to dataFrame as a column
months_1990 = pd.concat([pd.DataFrame(x[1].values) for x in groups_monthly], axis=1)
months_df = pd.DataFrame(months_1990)

# Set the column names for each month i.e. 1,2,3, .., 12
months_df.columns = range(1,13)

# Plot the box and whiskers plot for each month 
months_df.boxplot(figsize = (15,10))
plt.show()
```


![png](index_files/index_28_0.png)


We see 12 box and whisker plots, showing the significant change in distribution of minimum temperatures across the months of the year from the Southern Hemisphere summer in January to the Southern Hemisphere winter in the middle of the year, and back to summer again.

## Time Series Heat Maps

Let's create a heatmap of the Minimum Daily Temperatures data. The `matshow()` function from the matplotlib library is used as no heatmap support is provided directly in Pandas.

1. Rotate (transpose) the `temp_annual` dataframe as a new matrix the matrix so that each row represents one year and each column one day. 
2. Use `matshow()` function to draw a heatmap for transposed yearly matrix. 


```python
##### Transpose the yearly group DataFrame and draw a heatmap with matshow()

year_matrix = temp_annual.T
plt.matshow( year_matrix, interpolation=None, aspect='auto', cmap=plt.cm.Spectral_r)
plt.show()
```


![png](index_files/index_31_0.png)


We can now see that the plot shows the cooler minimum temperatures in the middle days of the years and the warmer minimum temperatures in the start and ends of the years, and all the fading and complexity in between.

Following this intuition, let's draw another heatmap comparing the months of the year in 1990. Each column represents one month, with rows representing the days of the month from 1 to 31.


```python
# draw a heatmap comparing the months of the year in 1990.
plt.matshow(months_df, interpolation=None, aspect='auto', cmap=plt.cm.Spectral_r)
plt.show()
```


![png](index_files/index_34_0.png)


The plot shows the same macro trend seen for each year on the zoomed level of month-to-month. We can also see some white patches at the bottom of the plot. This is missing data for those months that have fewer than 31 days, with February being quite an outlier with 28 days in 1990.

## Summary 

In this lab, we discovered how to explore and better understand a time-series dataset in Python and Pandas.
We learnt how to explore the temporal relationships with line, scatter, and autocorrelation plots. We also explored the distribution of observations with histograms and density plots and change in distribution of observations with box and whisker and heat map plots.
