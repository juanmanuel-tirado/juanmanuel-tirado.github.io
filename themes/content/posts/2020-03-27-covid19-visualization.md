---
categories:
- Data science
date: "2020-03-27T18:09:57Z"
excerpt: 'This is the first post of a series of publications describing solutions
  used in data science. The content is self-explained in a Jupyter Notebook available
  at my Github account. This first post explains different ways to display time series
  using Python with Pandas for data management, and Matplotlib or Seaborn for data
  visualization. The input data used corresponds to the Covid-19 virus propagation
  time series. '
tags:
- covid19
- datascience
- matplotlib
- pandas
- python
- time series
title: Covid-19 visualization
url: /covid19-visualization/
---

This is the first post of a series of publications describing solutions used in data science. The content is self-explained in a Jupyter Notebook available at my [Github account](https://github.com/juanmanuel-tirado/python/blob/master/covid19/Covid19%20plots%20visualization.ipynb). This first post explains different ways to display time series using Python with Pandas for data management, and Matplotlib or Seaborn for data visualization. The input data used corresponds to the Covid-19 virus propagation time series. 

# Introduction

No need to say that the Covid19 crisis is a global challenge that is going to change how we see the world. There is a lot of interest in understanding the internals of virus propagation and several disciplines can be really helpful in this task. There is a lot of data going around and we have really accessible tools to work with this data. 

For any data scientist this is a nice opportunity to explore and understand time series, graph theory and other fascinating disciplines. If you are just a newbie or a consolidated practitioner, I have decided to share a series of Jupyter Notebooks with some examples of tools and methods that you can find helpful. I will make my best to make all the code available.

[Kaggle](http://www.kaggle.com) has opened a challenge to forecast the propagation of the virus. You can check the challenge with more details at the Kaggle site [here](https://www.kaggle.com/c/covid19-global-forecasting-week-2). I invite you to check the notebooks uploaded by the community. I have not considered to participate in the challenge, but this could be a good opportunity if you plan to start with these kind of challenges.

In this part, I will use Kaggle data to show how we can visualize the virus evolution in different manners. You can download the data (after registration) [here](https://www.kaggle.com/c/covid19-global-forecasting-week-2/data). After downloading the zip file with the dataset we have three CSV files:
* train.csv
* test.csv
* submission.csv

For this exercise we will only use the train.csv file.

**Assumptions**
* You have an already running Jupyter environment
* You are familiar with Pandas
* You have heard about Matplotlib
* The covid19 files are available in the path covid19-global-forecasting-week-2

# Loading a CSV with Pandas

There are several solutions to read CSV files in Python. However, with no disussion Pandas is the most suitable option for many scenarios. We import the pandas library and read the csv file with all the training data.


```python
import pandas as pd
```


```python
data = pd.read_csv("covid19-global-forecasting-week-2/train.csv")
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Country_Region</th>
      <th>Province_State</th>
      <th>Date</th>
      <th>ConfirmedCases</th>
      <th>Fatalities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>2020-01-22</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>2020-01-23</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>2020-01-24</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>2020-01-25</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>2020-01-26</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>18811</th>
      <td>29360</td>
      <td>Zimbabwe</td>
      <td>NaN</td>
      <td>2020-03-21</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>18812</th>
      <td>29361</td>
      <td>Zimbabwe</td>
      <td>NaN</td>
      <td>2020-03-22</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>18813</th>
      <td>29362</td>
      <td>Zimbabwe</td>
      <td>NaN</td>
      <td>2020-03-23</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>18814</th>
      <td>29363</td>
      <td>Zimbabwe</td>
      <td>NaN</td>
      <td>2020-03-24</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>18815</th>
      <td>29364</td>
      <td>Zimbabwe</td>
      <td>NaN</td>
      <td>2020-03-25</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>18816 rows × 6 columns</p>
</div>



We have a six columns dataframe indicating the country, state, date, number of confirmed cases and number of fatalities. We are going to focus on one country. Let's say Spain.


```python
spain = data[data['Country_Region']=='Spain']
spain
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Country_Region</th>
      <th>Province_State</th>
      <th>Date</th>
      <th>ConfirmedCases</th>
      <th>Fatalities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13376</th>
      <td>20901</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-01-22</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>13377</th>
      <td>20902</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-01-23</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>13378</th>
      <td>20903</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-01-24</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>13379</th>
      <td>20904</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-01-25</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>13380</th>
      <td>20905</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-01-26</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13435</th>
      <td>20960</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-03-21</td>
      <td>25374.0</td>
      <td>1375.0</td>
    </tr>
    <tr>
      <th>13436</th>
      <td>20961</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-03-22</td>
      <td>28768.0</td>
      <td>1772.0</td>
    </tr>
    <tr>
      <th>13437</th>
      <td>20962</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-03-23</td>
      <td>35136.0</td>
      <td>2311.0</td>
    </tr>
    <tr>
      <th>13438</th>
      <td>20963</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-03-24</td>
      <td>39885.0</td>
      <td>2808.0</td>
    </tr>
    <tr>
      <th>13439</th>
      <td>20964</td>
      <td>Spain</td>
      <td>NaN</td>
      <td>2020-03-25</td>
      <td>49515.0</td>
      <td>3647.0</td>
    </tr>
  </tbody>
</table>
<p>64 rows × 6 columns</p>
</div>



We have data for 64 days with no information at a province/state level. Now we would like to have a visual representation of the time series.

# Matplotlib

The first solution to be considered is Pyplot from the [Matplotlib](https://matplotlib.org/) library. 


```python
from matplotlib import pyplot

pyplot.plot(spain.ConfirmedCases)
pyplot.title('Confirmed cases in Spain')
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_7_0.png)
    


The figure above is the representation of the number of confirmed cases in Spain until March 26th. We have not set the X axis, so pyplot is considering the id column defined by Pandas. To define a more reasonable X ticks we simply pass a list with the same number of items of the Y axis starting from zero.


```python
pyplot.plot(range(0,spain.ConfirmedCases.size),spain.ConfirmedCases)
pyplot.title('Confirmed cases in Spain')
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_9_0.png)
    


Now we have a clearer view of the X axis. However, we would like to have a comparison of the number of fatalities vs the number of confirmed cases.


```python
pyplot.plot(range(0,spain.ConfirmedCases.size),spain.ConfirmedCases,label='ConfirmedCases')
pyplot.plot(range(0,spain.Fatalities.size),spain.Fatalities,label='Fatalities')
pyplot.legend()
pyplot.title('Confirmed cases vs fatalities in Spain')
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_11_0.png)
    


The increment shows an exponential behaviour. A logarithmic scale would help a better view.


```python
pyplot.plot(range(0,spain.ConfirmedCases.size),spain.ConfirmedCases,label='ConfirmedCases')
pyplot.plot(range(0,spain.Fatalities.size),spain.Fatalities,label='Fatalities')
pyplot.yscale('log')
pyplot.title('Confirmed cases vs fatalities in Spain log scale')
pyplot.legend()
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_13_0.png)
    


What about displaying the date in the X axis? To do that we need pyplot to format the x axis. This requires datetime structures to set the datetime of every observation. We already have them in the Date column. The main difference is setting the formatter for the x axis using *mdates* from *matplotlib*.


```python
import matplotlib.dates as mdates

# convert date strings to datenums
dates = mdates.datestr2num(spain.Date)

pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.plot(dates,spain.ConfirmedCases,label='confirmed')
pyplot.plot(dates,spain.Fatalities,label='fatalities')
pyplot.title('Confirmed cases vs fatalities in Spain with datetime in x axis')
pyplot.legend()
pyplot.gcf().autofmt_xdate()
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_15_0.png)
    


# Seaborn

For those familiar with [ggplot](https://ggplot2.tidyverse.org/), [Seaborn](https://seaborn.pydata.org) will look familiar. Seaborn is built on top of Matplotlib and offers a high level interface for drawing statistical graphics. It is particularly suitable to used in conjunction with Pandas.

We can replicate some of the plots above:


```python
import seaborn as sns

g = sns.relplot(x=range(spain.Date.size),y='ConfirmedCases', data=spain,kind='line',)
g.set_axis_labels(x_var='') # I remove the xlabel for consistency with the previous plot
pyplot.title('Confirmed cases in Spain')
pyplot.show()

```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_17_0.png)
    


To set the x axis with datetimes we do the same we did with matplotlib. However, now we are going to directly transform the Date column from the Pandas Dataframe so we can directly call seaborn to use it.


```python
# Transform the Date column to matplotlib datenum
spain.Date = spain.Date.apply(lambda x : mdates.datestr2num(x))
```

    /Users/juan/miniconda3/lib/python3.7/site-packages/pandas/core/generic.py:5303: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self[name] = value


After this, the Date column type is a datenum that can be used to correctly format the x axis.

(By the way, this operation triggers a warning message. I let you to investigate why this is happening ;) )


```python
sns.relplot(x='Date',y='ConfirmedCases', data=spain,kind='line',)
pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.gcf().autofmt_xdate()
pyplot.title('Confirmed cases in Spain with datetime in x axis')
pyplot.show()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_21_0.png)
    


So far we replicated the same plots we already created using pyplot. Why is this seaborn interesting then? I find seaborn particularly relevant to create plots where we can easily compare different series. What if we try to compare the evolution of cases in different countries? We are going to select a sample of countries and compare their evolutions.

To do that we have to run two operations. 
* First. We filter the countries included in a list.
* Second. For some countries the values per day reflect observations per province. We are only interested in the observations per country and day. We aggregate the confirmed cases and fatalities columns for every country in the same day. 


```python
# sample of countries to study
chosen = ['Spain', 'Iran', 'Singapore', 'France', 'United Kingdom']

# 1) Filter rows which country is in the list  2) group by country and date and finally sum the result
sample = data[data.Country_Region.isin(chosen)].groupby(['Date','Country_Region'], as_index=False,).sum()
sample

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country_Region</th>
      <th>Id</th>
      <th>ConfirmedCases</th>
      <th>Fatalities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-01-22</td>
      <td>France</td>
      <td>112510</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-01-22</td>
      <td>Iran</td>
      <td>13601</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-01-22</td>
      <td>Singapore</td>
      <td>20401</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-01-22</td>
      <td>Spain</td>
      <td>20901</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-01-22</td>
      <td>United Kingdom</td>
      <td>198807</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>315</th>
      <td>2020-03-25</td>
      <td>France</td>
      <td>113140</td>
      <td>25600.0</td>
      <td>1333.0</td>
    </tr>
    <tr>
      <th>316</th>
      <td>2020-03-25</td>
      <td>Iran</td>
      <td>13664</td>
      <td>27017.0</td>
      <td>2077.0</td>
    </tr>
    <tr>
      <th>317</th>
      <td>2020-03-25</td>
      <td>Singapore</td>
      <td>20464</td>
      <td>631.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>318</th>
      <td>2020-03-25</td>
      <td>Spain</td>
      <td>20964</td>
      <td>49515.0</td>
      <td>3647.0</td>
    </tr>
    <tr>
      <th>319</th>
      <td>2020-03-25</td>
      <td>United Kingdom</td>
      <td>199248</td>
      <td>9640.0</td>
      <td>466.0</td>
    </tr>
  </tbody>
</table>
<p>320 rows × 5 columns</p>
</div>




```python
# As a sanity check we are going to check that the previous operation was correct.
# Lets check how many confirmed cases France had on 2020-03-24
france = data[(data.Country_Region=='France') & (data.Date=='2020-03-24')]
print('These are the values for France on 2020-03-24 before running the aggregation')
display(france)
print('Total number of confirmed cases: ', france.ConfirmedCases.sum())
print('And this is the aggregation we obtained')
sample[(sample.Country_Region=='France') & (sample.Date=='2020-03-24')]
```

    These are the values for France on 2020-03-24 before running the aggregation



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Country_Region</th>
      <th>Province_State</th>
      <th>Date</th>
      <th>ConfirmedCases</th>
      <th>Fatalities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6974</th>
      <td>10863</td>
      <td>France</td>
      <td>French Guiana</td>
      <td>2020-03-24</td>
      <td>23.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7038</th>
      <td>10963</td>
      <td>France</td>
      <td>French Polynesia</td>
      <td>2020-03-24</td>
      <td>25.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7102</th>
      <td>11063</td>
      <td>France</td>
      <td>Guadeloupe</td>
      <td>2020-03-24</td>
      <td>62.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>7166</th>
      <td>11163</td>
      <td>France</td>
      <td>Martinique</td>
      <td>2020-03-24</td>
      <td>57.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>7230</th>
      <td>11263</td>
      <td>France</td>
      <td>Mayotte</td>
      <td>2020-03-24</td>
      <td>36.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7294</th>
      <td>11363</td>
      <td>France</td>
      <td>New Caledonia</td>
      <td>2020-03-24</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7358</th>
      <td>11463</td>
      <td>France</td>
      <td>Reunion</td>
      <td>2020-03-24</td>
      <td>94.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7422</th>
      <td>11563</td>
      <td>France</td>
      <td>Saint Barthelemy</td>
      <td>2020-03-24</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7486</th>
      <td>11663</td>
      <td>France</td>
      <td>St Martin</td>
      <td>2020-03-24</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7550</th>
      <td>11763</td>
      <td>France</td>
      <td>NaN</td>
      <td>2020-03-24</td>
      <td>22304.0</td>
      <td>1100.0</td>
    </tr>
  </tbody>
</table>
</div>


    Total number of confirmed cases:  22622.0
    And this is the aggregation we obtained





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country_Region</th>
      <th>Id</th>
      <th>ConfirmedCases</th>
      <th>Fatalities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>310</th>
      <td>2020-03-24</td>
      <td>France</td>
      <td>113130</td>
      <td>22622.0</td>
      <td>1102.0</td>
    </tr>
  </tbody>
</table>
</div>



We have manually checked that the values we obtained after aggregation are correct. Now we are going to plot a comparison of these values per country.


```python
# remember to transform the date timestamp
sample.Date = sample.Date.apply(lambda x : mdates.datestr2num(x))
```


```python
# Confirmed cases
sns.relplot(x='Date',y='ConfirmedCases', col='Country_Region', hue='Country_Region', col_wrap=2, data=sample,kind='line',)
pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.gcf().autofmt_xdate()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_27_0.png)
    



```python
# Fatalities
sns.relplot(x='Date',y='Fatalities', col='Country_Region', hue='Country_Region', col_wrap=2, data=sample,kind='line',)
pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.gcf().autofmt_xdate()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_28_0.png)
    


Additionally, we can compare all the timelines in the same plot.


```python
sns.relplot(x='Date',y='ConfirmedCases', hue='Country_Region', data=sample,kind='line',)
pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.gcf().autofmt_xdate()

sns.relplot(x='Date',y='Fatalities', hue='Country_Region', data=sample,kind='line',)
pyplot.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
pyplot.gca().xaxis.set_major_locator(mdates.DayLocator(interval=5))
pyplot.gcf().autofmt_xdate()
```


    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_30_0.png)
    



    
![png](/assets/2020/03/Covid19_visualization_files/Covid19_visualization_30_1.png)
    


# Conclusions

In this notebook we have shown how we can use Python Matplotlib and Seaborn with Pandas to plot the time series corresponding to the Covid19 virus.
