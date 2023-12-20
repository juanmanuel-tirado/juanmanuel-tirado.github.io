---
categories:
- Data science
date: "2020-04-17T11:42:27Z"
excerpt: Has air traffic something to do with Covid19? Learn some tools to explore
  the impact of passengers in pandemics using networks.
tags:
- covid19
- datascience
- graph-tool
- graphs
- networks
- pandas
- plotly
- python
title: Covid19 spreading in a networking model (part I)
url: /covid19-spreading-in-a-networking-model-part-i/
mathjax: true
---

This is a reproduction of a Jupyter notebook you can find [here](https://github.com/juanmanuel-tirado/python/blob/master/covid19/Covid19%20spreading%20network%20(part%20I).ipynb)


## Introduction

The Covid19 has imposed a lot of pressure on epidemiologists to come up with models that can explain the impact of pandemics. There already is lot of theory developed around this topic you can check out. In particular, understanding  [SIR models](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology) is really useful to understand how researchers approach the problem of virus spreading. In this notebook, I introduce some ideas about how to use Pandas, plots and graphs to explore the evolution of pandemics. I show some tools you can use to simulate networks dynamics. Of course, this cannot be considered a contribution to the state of the art. However, you may find some interesting guidelines for other problems.


## The question

The main question we ask ourselves is **has air passengers something to do with Covid19?**. If so, **can we use air traffic data to know something else about Covid19 evolution?** 

It is known that the virus started in China and later propagated around the world. If air traffic has contributed to the spread of the virus, this means that we can somehow leverage a model that explains or predict the evolution of the disease. Furthermore, we could even predict what countries could be potentially infected and/or what flights shall be cancelled.

There is a vast amount of work that can be done in this topic. To make it more accessible we will split it into smaller parts. In this first part, we will explore the data we plan to use and how to work with graphs.

## Dataset

The [OpenFlights](https://openflights.org/data.html) dataset contains airlines, air routes and airports per country. The information contained in the dataset contains information up to 2014. This may be a bit outdated. However, it is detailed enough for our purposes.

Additionally, we use the [Kraggle Covid19 challenge](https://www.kaggle.com/c/covid19-global-forecasting-week-2) data as we did in our previous Covid19 series.


```python
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
import datetime
```


```python
routes_dataset_url = 'https://raw.githubusercontent.com/jpatokal/openflights/master/data/routes.dat'
airports_dataset_url = 'https://raw.githubusercontent.com/jpatokal/openflights/master/data/airports.dat'
countries_dataset_url = 'https://raw.githubusercontent.com/jpatokal/openflights/master/data/countries.dat'


# Load the file with the routes
routes = pd.read_csv(routes_dataset_url, names=['Airline','AirlineID','SourceAirport', 
                                                       'SourceAirportID', 'DestinationAirport', 'DestinationAirportID',
                                                      'CodeShare','Stops','Equipment'], 
                     na_values='\\N')
display(routes)


# Load the file with the airports
airports = pd.read_csv(airports_dataset_url, names=['AirportID','Name','City','Country','IATA','ICAO',
                                                         'Latitude', 'Longitude', 'Altitude', 'Timezone', 'DST',
                                                         'TzDatabaseTimeZone', 'Type', 'Source'], index_col='AirportID',
                      na_values='\\N')

display(airports)

# Covid19 data
covid_data = pd.read_csv('covid19-global-forecasting-week-2/train.csv',parse_dates=['Date'], 
                   date_parser=lambda x: datetime.datetime.strptime(x, '%Y-%m-%d'))

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
      <th>Airline</th>
      <th>AirlineID</th>
      <th>SourceAirport</th>
      <th>SourceAirportID</th>
      <th>DestinationAirport</th>
      <th>DestinationAirportID</th>
      <th>CodeShare</th>
      <th>Stops</th>
      <th>Equipment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2B</td>
      <td>410.0</td>
      <td>AER</td>
      <td>2965.0</td>
      <td>KZN</td>
      <td>2990.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>CR2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2B</td>
      <td>410.0</td>
      <td>ASF</td>
      <td>2966.0</td>
      <td>KZN</td>
      <td>2990.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>CR2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2B</td>
      <td>410.0</td>
      <td>ASF</td>
      <td>2966.0</td>
      <td>MRV</td>
      <td>2962.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>CR2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2B</td>
      <td>410.0</td>
      <td>CEK</td>
      <td>2968.0</td>
      <td>KZN</td>
      <td>2990.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>CR2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2B</td>
      <td>410.0</td>
      <td>CEK</td>
      <td>2968.0</td>
      <td>OVB</td>
      <td>4078.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>CR2</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>67658</th>
      <td>ZL</td>
      <td>4178.0</td>
      <td>WYA</td>
      <td>6334.0</td>
      <td>ADL</td>
      <td>3341.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>SF3</td>
    </tr>
    <tr>
      <th>67659</th>
      <td>ZM</td>
      <td>19016.0</td>
      <td>DME</td>
      <td>4029.0</td>
      <td>FRU</td>
      <td>2912.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>734</td>
    </tr>
    <tr>
      <th>67660</th>
      <td>ZM</td>
      <td>19016.0</td>
      <td>FRU</td>
      <td>2912.0</td>
      <td>DME</td>
      <td>4029.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>734</td>
    </tr>
    <tr>
      <th>67661</th>
      <td>ZM</td>
      <td>19016.0</td>
      <td>FRU</td>
      <td>2912.0</td>
      <td>OSS</td>
      <td>2913.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>734</td>
    </tr>
    <tr>
      <th>67662</th>
      <td>ZM</td>
      <td>19016.0</td>
      <td>OSS</td>
      <td>2913.0</td>
      <td>FRU</td>
      <td>2912.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>734</td>
    </tr>
  </tbody>
</table>
<p>67663 rows × 9 columns</p>
</div>



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
      <th>Name</th>
      <th>City</th>
      <th>Country</th>
      <th>IATA</th>
      <th>ICAO</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Altitude</th>
      <th>Timezone</th>
      <th>DST</th>
      <th>TzDatabaseTimeZone</th>
      <th>Type</th>
      <th>Source</th>
    </tr>
    <tr>
      <th>AirportID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Goroka Airport</td>
      <td>Goroka</td>
      <td>Papua New Guinea</td>
      <td>GKA</td>
      <td>AYGA</td>
      <td>-6.081690</td>
      <td>145.391998</td>
      <td>5282</td>
      <td>10.0</td>
      <td>U</td>
      <td>Pacific/Port_Moresby</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Madang Airport</td>
      <td>Madang</td>
      <td>Papua New Guinea</td>
      <td>MAG</td>
      <td>AYMD</td>
      <td>-5.207080</td>
      <td>145.789001</td>
      <td>20</td>
      <td>10.0</td>
      <td>U</td>
      <td>Pacific/Port_Moresby</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mount Hagen Kagamuga Airport</td>
      <td>Mount Hagen</td>
      <td>Papua New Guinea</td>
      <td>HGU</td>
      <td>AYMH</td>
      <td>-5.826790</td>
      <td>144.296005</td>
      <td>5388</td>
      <td>10.0</td>
      <td>U</td>
      <td>Pacific/Port_Moresby</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nadzab Airport</td>
      <td>Nadzab</td>
      <td>Papua New Guinea</td>
      <td>LAE</td>
      <td>AYNZ</td>
      <td>-6.569803</td>
      <td>146.725977</td>
      <td>239</td>
      <td>10.0</td>
      <td>U</td>
      <td>Pacific/Port_Moresby</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Port Moresby Jacksons International Airport</td>
      <td>Port Moresby</td>
      <td>Papua New Guinea</td>
      <td>POM</td>
      <td>AYPY</td>
      <td>-9.443380</td>
      <td>147.220001</td>
      <td>146</td>
      <td>10.0</td>
      <td>U</td>
      <td>Pacific/Port_Moresby</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>14106</th>
      <td>Rogachyovo Air Base</td>
      <td>Belaya</td>
      <td>Russia</td>
      <td>NaN</td>
      <td>ULDA</td>
      <td>71.616699</td>
      <td>52.478298</td>
      <td>272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>14107</th>
      <td>Ulan-Ude East Airport</td>
      <td>Ulan Ude</td>
      <td>Russia</td>
      <td>NaN</td>
      <td>XIUW</td>
      <td>51.849998</td>
      <td>107.737999</td>
      <td>1670</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>14108</th>
      <td>Krechevitsy Air Base</td>
      <td>Novgorod</td>
      <td>Russia</td>
      <td>NaN</td>
      <td>ULLK</td>
      <td>58.625000</td>
      <td>31.385000</td>
      <td>85</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>14109</th>
      <td>Desierto de Atacama Airport</td>
      <td>Copiapo</td>
      <td>Chile</td>
      <td>CPO</td>
      <td>SCAT</td>
      <td>-27.261200</td>
      <td>-70.779198</td>
      <td>670</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
    <tr>
      <th>14110</th>
      <td>Melitopol Air Base</td>
      <td>Melitopol</td>
      <td>Ukraine</td>
      <td>NaN</td>
      <td>UKDM</td>
      <td>46.880001</td>
      <td>35.305000</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>airport</td>
      <td>OurAirports</td>
    </tr>
  </tbody>
</table>
<p>7698 rows × 13 columns</p>
</div>



```python
# We compute some intermediate dataframes that will be useful later

# Aggregate the number of flights per airport.
# Source_airport -> destination_airport, number of flights
flights_airport = routes.groupby(['SourceAirportID','DestinationAirportID']).size().reset_index(name='num_flights')

# Get the countries for the previous airport IDs
flights_country=flights_airport.join(airports.Country,on='SourceAirportID',lsuffix='Source').\
                join(airports.Country,on='DestinationAirportID',lsuffix='Destination')[['Country','CountryDestination','num_flights']]

# Aggregate the number of flights between countries
# SourceCountry -> DestinationCountry, number of flights
paths = flights_country.groupby(['Country','CountryDestination']).sum()
# Aggregate the number of flights to other countries.
paths_other_countries = flights_country[flights_country.Country != flights_country.CountryDestination].groupby(['Country','CountryDestination']).sum()

# Total number of flights per country
total_flights_country = paths.groupby(['Country']).sum()
# Total number of flights to other countries
total_flights_other_countries = paths_other_countries.groupby(['Country']).sum()

```

## Air routes visualization

The OpenFlights offers information about the flights between airports. Right now we are only interested in the number of flights between countries. This will simplify the visualization of the air traffic around the globle and it is enough to spot airflights hubs in the dataset.

**Note:** generating the plot may take a while. However, we added a fancy loading bar :)



```python
# Some code to have a nice progress bar :)
from IPython.display import clear_output

def update_progress(progress):
    bar_length = 20
    if isinstance(progress, int):
        progress = float(progress)
    if not isinstance(progress, float):
        progress = 0
    if progress < 0:
        progress = 0
    if progress >= 1:
        progress = 1

    block = int(round(bar_length * progress))
    clear_output(wait = True)
    text = 'Progress: [{0}] {1:.1f}%'.format( '#' * block + '-' * (bar_length - block), progress * 100)
    print(text)
```


```python
fig = go.Figure()
total_rows = float(len(paths_other_countries.index))
num_row=-1
for index, row in paths_other_countries.iterrows():
    
    num_row = num_row + 1
    update_progress(num_row/total_rows)
    fig.add_trace(
        go.Scattergeo(
            locationmode = 'country names',
            locations = index,
            mode = 'lines',
            hoverinfo= 'skip',
            line = dict(width=1,color='red'),
            opacity = float(row['num_flights'])/float(total_flights_other_countries.loc[index[0]]),
            showlegend=False,
        )
    )

fig.update_layout(
    geo = dict(
        showcountries = True,
    ),
    showlegend = False,
    title = 'Aggregated air routes between countries excluding domestic flights'
)

fig.show(renderer='svg')
```

    Progress: [####################] 100.0%



    
![svg](assets/2020/04/assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_6_1.svg)
    


If the air traffic has something to do with the Covid19 spread then, China's air traffic requires our attention. The figure below shows the routes between China and other countries excluding domestic flights. The wide of every line is representative of the number of flights between countries. 

Excluding domestic flights (which are quite a few), China has connections to 62 countries. Assuming daily flights, this means that an infected passenger can propagate the disease around the globe in a few hours.


```python

aux = paths_other_countries.reset_index()
aux = aux[aux.Country=='China']


fig = go.Figure()

total_rows = float(len(aux.index))
num_row=-1

the_min = aux.num_flights.min()
the_max = aux.num_flights.max()
the_mean = aux.num_flights.mean()
the_std = aux.num_flights.std()
scalator = float(the_max - the_min)

for index, row in aux.iterrows():
    num_row = num_row + 1
    update_progress(num_row/total_rows)
    width = ((float(row['num_flights'])-the_min)/scalator)*15
    #width = np.max([0.5, width])
   
    fig.add_trace(
        go.Scattergeo(
            locationmode = 'country names',
            locations = [row.Country,row.CountryDestination],
            mode = 'lines',
            hoverinfo= 'skip',
            line = dict(width=width,color='red'),
            showlegend=False,
        )
    )
    

fig.update_layout(
    geo = dict(
        showcountries = True,
    ),
    showlegend = False,
    title = 'China aggregated air routes between countries excluding domestic flights'
)

fig.show(renderer='svg')

```

    Progress: [####################] 98.4%



    
![svg](assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_8_1.svg)
    


## Disseminations with graphs

Now that we have a better understanding of our dataset and how does it look, it is time to prepare some artifacts to study the spread of the virus. In this case, we are going to translate our Pandas tables into a graph that represents the network of countries connected by their flights.

We can create a directed graph $G(V,E)$ where the set of vertices $V$ represents the countries and the set of edges $E$ represents existing air routes between countries $A$ and $B$. Additionally, we can set weights $W(E)$ with the number of flights between two countries.

There are some nice libraries to work with graphs in Python. However, I particularly like [Graph Tool](https://graph-tool.skewed.de/), maintained by [Tiago de Paula Peixoto](https://skewed.de/tiago) from the Central European University. It has implementations of some sophisticated algorithms done in C++ with OpenMP.


```python
# Run this to ensure that the drawing module is correctly stablished
from graph_tool.all import *
import graph_tool as gt
```


```python
g = gt.Graph()

# Create the country property with the country names
countries_prop = g.new_vertex_property('string')
g.vertex_properties['country'] = countries_prop

# Total number of outgoing flights per vertex
out_flights_prop = g.new_vertex_property('int')
g.vertex_properties['out_flights'] = out_flights_prop

# Create edge property with the number of flights between countries
num_flights_prop = g.new_edge_property('double')
g.edge_properties['num_flights'] = num_flights_prop

# Get the complete list of countries
list_countries = np.union1d(paths.reset_index().Country.unique(), paths.reset_index().CountryDestination.unique())

g.add_vertex(len(list_countries))

# For a given country, get the vertex
country_vertex = {}
index=0
#for c in total_flights_country.index:
for c in list_countries:
    v = g.vertex(index)
    countries_prop[v] = c
    country_vertex[c] = v
    # skip self-loops
    try: 
        out_flights_prop[v] = total_flights_other_countries.loc[c]['num_flights']
    except:
        out_flights_prop[v] = 0.0
        
    index=index+1


# Add the edges
for index,num_flights in paths.iterrows():
    s = country_vertex[index[0]]
    d = country_vertex[index[1]]
    # No self-loops
    if s != d:
        e = g.add_edge(s,d)
        num_flights_prop[e] = num_flights

```

We draw our graph using a radial layout with China in the center. The edges correspond to the neighbors that can be reached in a single step from China. This is, the 62 countries with direct flights from China.


```python
# plot with a radial distribution with China in the center
china_vertex = country_vertex['China']

pos = gt.draw.radial_tree_layout(g,china_vertex)
gv = gt.GraphView(g, efilt=lambda e : e.source()==china_vertex)

p=gt.draw.graph_draw(gv,pos)
```


    
![png](/assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_13_0.png)
    


Considering a dissemination scenario, we draw the edges for all the countries that can be reached after a scale in a flight from China. As you can see the number is particularly large.


```python
second_step = []
for v in g.get_out_neighbors(china_vertex):
    second_step.append(v)
    
gv2 = gt.GraphView(g, efilt=lambda e : e.target!=china_vertex and e.source() in second_step)
p=gt.draw.graph_draw(gv2,pos)
```


    
![png](/assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_15_0.png)
    


The graph above is a bit misleading. Not all the vertices will be reached from the root vertex (China) with the same probability. In other words, the frequency of flights between countries will increase the probabilities to transfer an infected passenger from China.

If we consider the weight $W$ of every edge to be the number of outgoing flights following the edge direction, everything becomes more interesting. The figure below changes the width of every edge connecting China with its flight-connected neighbors. A few number of countries (Taiwan, South Korea, Japan) accumulates most of China's outgoing traffic as the wide arrows show.

**Note:** in the figure below the $W$ weight is normalized. Otherwise, most of the connections would not be plotted.


```python
aux = gv.edge_properties['num_flights'].copy()

aux.a = (aux.a - aux.a.min())/(aux.a.max()-aux.a.min()) * 10
p=gt.draw.graph_draw(gv,pos,edge_pen_width=aux)

```


    
![png](/assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_17_0.png)
    


## SIR epidemic process

The [SIR](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology) model is a simple, yet powerful, compartimental model for epidemics. The model consists of three compartments $S$, $I$ and $R$. $S$ is the number of susceptible, $I$ the number of infectuous and $R$ the number of recovered or deceased individuals. This is the most common model used right now and has many extensions and enhancements. The number of individuals in $S$ falls down as they become infected ($I$) and then recovered $R$. This is one of the famous infection curves used to forecast Covid19 evolution.

How to determine when a new individual moves between compartments depends on transition rates. Continuing with our initial idea about how air traffic can be an important actor in Covid19 dissemination, we can use our air flight connections graph to run a SIR simulation. 

Fortunately, graph tool comes with an implementation of the SIR model easily configurable. We run a SIR model in 63 steps which is the number of days the Covid19 required to expand worldwide in our dataset. For every edge in our graph we compute $\beta_e = \sum_{J \in out(A)} W(A,J)$ that basically sets the transition between vertices as the ratio of flights that connection represent among the total number of outgoing flights in vertex $A$. Additionally, we configure  SIR to remove spontaneous infections and reinfections and set China as the dissemination seed.



```python

def runSIR(state,num_iter=100):
    '''
    For a graph tool epidemic model run the state and return a dataframe with the countries infected in every step
    '''
    infections = pd.DataFrame()
    
    previous = state.get_state().fa
    initial = np.where(previous==1)[0]
    
    for t in range(num_iter):
        ret=state.iterate_sync()
        new = state.get_state().fa
        # Find new infected countries
        diff = new - previous
        already_infected = state.get_state().fa.sum()
        non_infected = len(state.get_state().fa)-already_infected
        new_infected = [countries_prop[g.vertex(v)] for v in np.where(diff==1)[0]]
        previous = new.copy()
        # collect the results in a Pandas friendly way
        infections = infections.append([{'step':t,'norm_steps':t/float(num_iter),'infected':already_infected,
                                'new_infected': len(new_infected),'non_infected': non_infected,
                                'new_infected_countries': new_infected}], ignore_index=True)
        
    return infections

```


```python
# Create an edge property map that can help us to define the beta probability between vertices 
beta_prop = g.new_edge_property('double')
for e in g.edges():
    beta_prop[e] = num_flights_prop[e]/out_flights_prop[e.source()]

# Assume that vertices are not susceptible, except the seed.
susceptible_prop = g.new_vertex_property('float')
for v in g.vertices():
       susceptible_prop[v] = 0.0
susceptible_prop[china_vertex]=1.0

```


```python
state = gt.dynamics.SIRState(g, beta=beta_prop,v0 = china_vertex, constant_beta=True,gamma=0,
                              r=0,s=susceptible_prop,epsilon=0,)

infections_SIR = runSIR(state,64)
display(infections_SIR[infections_SIR.new_infected>0].head(10))
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
      <th>step</th>
      <th>norm_steps</th>
      <th>infected</th>
      <th>new_infected</th>
      <th>non_infected</th>
      <th>new_infected_countries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.015625</td>
      <td>8</td>
      <td>5</td>
      <td>217</td>
      <td>[Australia, India, Indonesia, Kyrgyzstan, Sout...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.031250</td>
      <td>13</td>
      <td>5</td>
      <td>212</td>
      <td>[Israel, Russia, Saudi Arabia, Thailand, Unite...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.046875</td>
      <td>20</td>
      <td>7</td>
      <td>205</td>
      <td>[Egypt, Hong Kong, Malaysia, Taiwan, Turkey, U...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0.062500</td>
      <td>27</td>
      <td>7</td>
      <td>198</td>
      <td>[Burma, France, Germany, Pakistan, Peru, Polan...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>0.078125</td>
      <td>36</td>
      <td>9</td>
      <td>189</td>
      <td>[Austria, Belarus, Denmark, Kenya, New Zealand...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.093750</td>
      <td>50</td>
      <td>14</td>
      <td>175</td>
      <td>[Argentina, Bosnia and Herzegovina, Brazil, Ca...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>0.109375</td>
      <td>59</td>
      <td>9</td>
      <td>166</td>
      <td>[Ethiopia, Greece, Greenland, Italy, Libya, Ma...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>0.125000</td>
      <td>71</td>
      <td>12</td>
      <td>154</td>
      <td>[Bangladesh, Belgium, Cambodia, Chile, Colombi...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>0.140625</td>
      <td>79</td>
      <td>8</td>
      <td>146</td>
      <td>[Azerbaijan, Bahrain, Czech Republic, Jordan, ...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>0.156250</td>
      <td>86</td>
      <td>7</td>
      <td>139</td>
      <td>[Dominican Republic, Ecuador, Iran, Lebanon, M...</td>
    </tr>
  </tbody>
</table>
</div>


The output above shows the number of infected countries over time. Observe that it takes a while for the network to consider new infected countries. 

It is interesting to compare our outcomes with the current Covid19 evolution per country. We consider in our dataset that a country is infected as soon as it confirms a single case. We assume that every day after the first case appeared in China can be considered as a step in the dissemination process. This assumption is done in order to facilitate the comparison with our SIR simulation.


```python
# Compute the number of infected countries
# Get the first date with confirmed cases for every country
first_date = covid_data[covid_data.ConfirmedCases > 0].groupby('Country_Region', as_index=False).Date.min().sort_values(by='Date')
# Compute the number of elapsed days since the beginning of China's outbreak
first_date['step']=first_date.Date-datetime.datetime(2020,1,22)
# Convert this column into an integer with the number of days.
first_date.step = first_date.step.dt.days
# Get the number of infected countries per region
covid_infected = first_date[['step','Country_Region']].groupby('step').agg(new_infected=('step','count'))
covid_infected['infected'] = covid_infected['new_infected'].cumsum()
covid_infected = covid_infected.reset_index()
```


```python
f = go.Figure()
f.add_trace(
    go.Scatter(x=covid_infected.step,y=covid_infected.infected,name='Covid19')
)
f.add_trace(
    go.Scatter(x=infections_SIR.step,y=infections_SIR.infected,name='SIR')
)
f.update_layout(
    title_text='Comparing SIR infected countries evolution with original Covid19',
    xaxis=dict(title='Step'),
    yaxis=dict(title='Infected countries')
)

f.show(renderer='svg')
```


    
![svg](assets/2020/04/Covid19_spreading_i_files/Covid19_spreading_i_24_0.svg)
    


The original Covid19 evolution differs in the first steps with our SIR simulation. The initial plateau before the increasing slope, takes much longer in the Covid19 series. There is probably a lag between a country welcoming infected passengers and the declaration of that case. However, it is interesting to observe this difference between the number of declared cases and the evolution of our model. Obviously, there is some remaining exploration work before we can understanding why this is happening.


# Conclusions

In this notebook we have introduced some interesting tools to analyze the influence of air flights in the dissemination of the Covid19 around the world. Firstly, we have explored the OpenFlights dataset containing air traffic information around the world. Second, we have used this dataset to create an air traffic network. Finally, we have used this network to run a SIR model to understand the dissemination of the virus using air connections.


# References

Tiago P. Peixoto, “The graph-tool python library”, figshare. (2014) [DOI: 10.6084/m9.figshare.1164194](https://dx.doi.org/10.6084/m9.figshare.1164194) [sci-hub](https://sci-hub.tw/10.6084/m9.figshare.1164194)
