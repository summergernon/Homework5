# Homework5


```python
# Analysis
# 1. Urban drivers made up over three quarters of the total drivers
# 2. Suburban rides made up over one quarter of the total rides
# 3. For the most part, the average fare for rural rides was higher than other city types.
# This could be because rural drives have a farther distance to cover.
```


```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

```


```python
# read the ride_csv into a dataframe
ride_csv = 'raw_data/ride_data.csv'
ride_df = pd.read_csv(ride_csv)
#ride_df.head()
```


```python
# read the city_csv file into a dataframe
city_csv = 'raw_data/city_data.csv'
city_df = pd.read_csv(city_csv)
#city_df.head()
```


```python
# Merge the data frames together based on which city is mentioned
merge_table = pd.merge(ride_df, city_df, on = 'city')
merge_table.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Only look at urban types
urban_type = merge_table.loc[merge_table['type'] == 'Urban']
# urban_type.head()
```


```python
# Group by city
grouped_urban = urban_type.groupby(['city'])
```


```python
# Find the average fare per city
average_urban_fare = grouped_urban['fare'].mean()
# average_urban_fare.head()
```


```python
# Count the number of rides per city
urban_rides = grouped_urban['fare'].count()
# urban_rides.head()
```


```python
# Take the first value from driver_count to get the number of drivers per city
urban_drivers = grouped_urban['driver_count'].first()
# urban_drivers.head()
```


```python
# Build a scatter plot for the urban values
#plt.scatter(urban_rides, average_urban_fare, marker = 'o', facecolors = 'red', edgecolors = 'black', s = urban_drivers, alpha = 0.5)
#plt.ylim(0,45)
#plt.xlim(0,40)
#plt.show()

```


```python
# Look at only suburban types
suburban_type = merge_table.loc[merge_table['type'] == 'Suburban']
#suburban_type.head()
```


```python
# Group by city
grouped_suburban = suburban_type.groupby(['city'])
```


```python
# Find average fare per city
average_suburban_fare = grouped_suburban['fare'].mean()
#average_suburban_fare.head()
```


```python
# Count the number of rides per city
suburban_rides = grouped_suburban['fare'].count()
#suburban_rides.head()
```


```python
# Take the first value from driver_count to get the number of drivers per city
suburban_drivers = grouped_suburban['driver_count'].first()
#suburban_drivers.head()
```


```python
# Build a scatter plot for the suburban values
#plt.scatter(suburban_rides, average_suburban_fare, marker = 'o', facecolors = 'blue', edgecolors = 'black', s = suburban_drivers, alpha = 0.5)
#plt.ylim(0,45)
#plt.xlim(0,40)
#plt.show()

```


```python
# Look at only rural types
rural_type = merge_table.loc[merge_table['type'] == 'Rural']
#rural_type.head()
```


```python
# Group by city
grouped_rural = rural_type.groupby(['city'])
```


```python
# Find average fare per city
average_rural_fare = grouped_rural['fare'].mean()
#average_rural_fare.head()
```


```python
# Count the number of rides per city
rural_rides = grouped_rural['fare'].count()
#rural_rides.head()
```


```python
# Take the first value from driver_count to get the number of drivers per city
rural_drivers = grouped_rural['driver_count'].first()
#rural_drivers.head()
```


```python
# Build a scatter plot for the rural values
#plt.scatter(rural_rides, average_rural_fare, marker = 'o', facecolors = 'yellow', edgecolors = 'black', s = rural_drivers, alpha = 0.5)
#plt.ylim(0,45)
#plt.xlim(0,40)
#plt.show()

```


```python
# Plot our scatter plots together and multiply size by a number to increase the size of the markers
urban = plt.scatter(urban_rides, average_urban_fare, marker = 'o', facecolors = 'lightcoral', edgecolors = 'black', s = urban_drivers*8, alpha = 0.75)
suburban = plt.scatter(suburban_rides, average_suburban_fare, marker = 'o', facecolors = 'lightskyblue', edgecolors = 'black', s = suburban_drivers*8, alpha = 0.75)
rural = plt.scatter(rural_rides, average_rural_fare, marker = 'o', facecolors = 'gold', edgecolors = 'black', s = rural_drivers*8, alpha = 0.75)
plt.xlim(0,40)
plt.ylim(15,55)
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')
plt.title('Pyber Ride Sharing Data (2016)')
legend = plt.legend((urban, suburban, rural), ('Urban', 'Suburban', 'Rural'), loc ='upper right')
# Make the markers in the legend the same siae
legend.legendHandles[0]._sizes = [75]
legend.legendHandles[1]._sizes = [75]
legend.legendHandles[2]._sizes = [75]
# Add text to the side of the plot
plt.figtext(1,.5, 'Note: Cirle size correlates with driver count per city')
# Add gridlines
plt.grid()
plt.show()
```


![png](output_23_0.png)



```python
# Create variables that add the fare for each city type

urban_total_fare = urban_type['fare'].sum()

suburban_total_fare = suburban_type['fare'].sum()

rural_total_fare = rural_type['fare'].sum()

```


```python
# Pie Chart-% of total fares by city type
labels = ['Urban', 'Suburban', 'Rural']

sizes = [urban_total_fare, suburban_total_fare, rural_total_fare]

colors = ['lightcoral', 'lightskyblue', 'gold']

explode = [0.075, 0, 0]
```


```python
# Plot the pie chart
plt.pie(sizes, explode=explode, labels=labels
        , colors=colors, autopct='%1.1f%%'
        , shadow=True, startangle=220)
plt.title('% of Total Fares by City Type')
plt.axis("equal")
```




    (-1.1165648204653278,
     1.1714863342490793,
     -1.1500420479185027,
     1.1156160518427185)




![png](output_26_1.png)



```python
# Pie Chart-% of total rides by city type
urban_total_rides = urban_type['fare'].count()

suburban_total_rides = suburban_type['fare'].count()

rural_total_rides = rural_type['fare'].count()

sizes = [urban_total_rides, suburban_total_rides, rural_total_rides]
```


```python
plt.pie(sizes, explode=explode, labels=labels
        , colors=colors, autopct='%1.1f%%'
        , shadow=True, startangle=220)
plt.title('% of Total Rides by City Type')
plt.axis("equal")
```




    (-1.1111841253438814,
     1.1909410154026123,
     -1.140876034947668,
     1.0920471062857113)




![png](output_28_1.png)



```python
# Pie Chart-% of Total Drivers by City Type
urban_total_drivers = urban_drivers.sum() 

suburban_total_drivers = suburban_drivers.sum()

rural_total_drivers = rural_drivers.sum()

```


```python
sizes = [urban_total_drivers, suburban_total_drivers, rural_total_drivers]
```


```python
plt.pie(sizes, explode=explode, labels=labels
        , colors=colors, autopct='%1.1f%%'
        , shadow=True, startangle=220)
plt.title('% of Total Drivers by City Type')
plt.axis("equal")
```




    (-1.118796066404444,
     1.1799140752474053,
     -1.1036109324981351,
     1.1037293838003295)




![png](output_31_1.png)

