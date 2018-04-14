

```python
# Dependencies
import pandas as pd
```


```python
# Use pandas to read data
heroes_df = pd.read_json('purchase_data.json',orient = 'columns')
heroes_df.head()
```


```python
# PLAYER COUNT

# Count the number of unique players using SN
player_counts = heroes_df['SN'].nunique()


# Display this information in a table
player_count = pd.DataFrame({'Total Players' : [player_counts]})
player_count
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS (TOTAL)

# Find the number of unique items by using nunique
max_item = heroes_df['Item ID'].nunique()
max_item

# Find the average purchase price
avg_price = heroes_df['Price'].mean()
avg_price

# Find the total number of purchases using Item ID
total_purchases = heroes_df['Item ID'].count()
total_purchases

# Find the total revenue
total_revenue = heroes_df['Price'].sum()
total_revenue

# Display this information in a table
purchasing_analysis = pd.DataFrame({'Number of Unique Items' : [max_item], 'Average Price' : [avg_price] ,
             'Number of Purchases' : [total_purchases], 'Total Revenue' : [total_revenue]})
purchasing_analysis
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.931192</td>
      <td>780</td>
      <td>183</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# GENDER DEMOGRAPHICS
# drop duplicates by SN
gender_df = heroes_df.drop_duplicates('SN')

# group new dataframe by gender and find how many are in each group
gender_df.groupby('Gender').size()

# now find the number in each gender
gender = gender_df["Gender"].value_counts()

# find the percentage of each gender by taking that gender's number and dividing by the number of total players and multiplying by 100
gender_percentage = (gender/player_counts * 100)

# display this information in a table
gender_demographics = pd.DataFrame({'Percentage of Players' : gender_percentage, 'Total Count' : gender})
gender_demographics.round(2)

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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS (GENDER)

# group our dataframe by Gender
purchase_df = heroes_df.groupby('Gender')

# find the size of each gender category
purchase_count = purchase_df.size()

# find the average purchase price per gender by using mean
average_purchase = purchase_df['Price'].mean()
average_purchase.round(2)

# find the total purchase value per gender by using sum
total_purchase = purchase_df['Price'].sum()
#total_purchase

# take the total purchase value and divide it by gender, which counts the number of all genders combined
normalized_totals = total_purchase/gender
#normalized_totals.round(2)

# create a table with this information
purchasing_analysis = pd.DataFrame({'Purchase Count' : purchase_count
                                    , 'Average Purchase Price' : average_purchase.map("${:,.2f}".format)
                                    , 'Total Purchase Value' : total_purchase.map("${:,.2f}".format)
                                    , 'Normalized Totals' : normalized_totals.map("${:,.2f}".format)})
purchasing_analysis.round(2)

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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create bins to sort by age
bins = [0,10,15,20,25,30,35,40,100]

# Create names for the bins
group_names = ['<10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', '40+']


# Cut age and place the ages into bins
pd.cut(heroes_df['Age'], bins, labels = group_names)

# Add Age Range
heroes_df['Age Range'] = pd.cut(heroes_df['Age'], bins, labels=group_names)

```


```python
# AGE DEMOGRAPHICS

# group the dataframe based on age range
age_groups = heroes_df.groupby('Age Range')

# count the number of items in each age group
age_count = age_groups.size()


# find the percentage by taking the number of items in each age group divided by the total number of players and multiplying by 100
age_percentage = (age_count/player_counts * 100)

# display this information in a table and round to the nearest 2 decimal places
age_purchasing_analysis = pd.DataFrame({'Percentage of Players' : age_percentage
                                        , 'Total Count' : age_count})
age_purchasing_analysis.round(2)


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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5.58</td>
      <td>32</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>13.61</td>
      <td>78</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>32.11</td>
      <td>184</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>53.23</td>
      <td>305</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>13.26</td>
      <td>76</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>10.12</td>
      <td>58</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_purchase_count = age_groups['SN'].nunique()
#age_purchase_count

age_average_purchase = age_groups['Price'].mean()
#age_average_purchase

age_total_purchase = age_groups['Price'].sum()
#age_total_purchase

age_normalized_totals = age_total_purchase/age_purchase_count
#age_normalized_totals

age_analysis = pd.DataFrame({'Purchase Count' : age_purchase_count
                             , 'Average Purchase Price' : age_average_purchase.map("${:,.2f}".format)
                             , 'Total Purchase Value' : age_total_purchase.map("${:,.2f}".format)
                             , 'Normalized Totals' : age_normalized_totals.map("${:,.2f}".format)})
age_analysis.head()

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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>$3.02</td>
      <td>$4.39</td>
      <td>22</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>$2.87</td>
      <td>$4.15</td>
      <td>54</td>
      <td>$224.15</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>$2.87</td>
      <td>$3.80</td>
      <td>139</td>
      <td>$528.74</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>$2.96</td>
      <td>$3.86</td>
      <td>234</td>
      <td>$902.61</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>$2.89</td>
      <td>$4.23</td>
      <td>52</td>
      <td>$219.82</td>
    </tr>
  </tbody>
</table>
</div>




```python
# TOP SPENDERS
# Top Spenders; identify the top 5 spenders in the game by total purchase value

# Group dataframe by SN
top_spender = heroes_df.groupby('SN')

# find the top spender by using sum
total_purchase = top_spender['Price'].sum()

# find average purchase price by using mean
avg_purchase_price = top_spender['Price'].mean()

# use count to find the number of items a user bought
total_purchase_count = top_spender['Item Name'].count()

# store in table
top_spenders = pd.DataFrame({'Purchase Count' : total_purchase_count
                             ,'Average Purchase Price' : avg_purchase_price.map("${:,.2f}".format)
                             , 'Total Purchase Value' : total_purchase.map("${:,.2f}".format) })

top_spenders = top_spenders.sort_values(['Total Purchase Value'], ascending=False)
top_spenders.head()


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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Qarwen67</th>
      <td>$2.49</td>
      <td>4</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>$3.13</td>
      <td>3</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>$3.06</td>
      <td>3</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>$3.06</td>
      <td>3</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>$4.59</td>
      <td>2</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST POPULAR ITEMS

# count the number of items
most_pop = pd.DataFrame(heroes_df.groupby(['Item ID', 
                                           'Item Name']) ['Price'].count())

# find the average of the items
item_avg = pd.DataFrame(heroes_df.groupby(['Item ID',
                                           'Item Name']) ['Price'].mean())

# add up all the items
item_total = pd.DataFrame(heroes_df.groupby(['Item ID', 
                                             'Item Name']) ['Price'].sum())

# store these results in a table
most_pop_items = pd.DataFrame({'Purchase Count': most_pop['Price']
                               , 'Item Price':(item_avg['Price']).map("${:,.2f}".format)
                               , 'Total Purchase Value':(item_total['Price']).map("${:,.2f}".format)})

# sort by descending so the most popular item shows up first
most_pop_items=most_pop_items.sort_values(['Purchase Count'], ascending = False)

most_pop_items.head()


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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST PROFITABLE ITEMS

# Group dataframe by Item Name and ID
most_profitable = heroes_df.groupby(['Item ID', 'Item Name'])

# find the most profitable item by using sum
most_profitable_item = most_profitable['Price'].sum()

# find average purchase price by using mean
avg_profitable_price = most_profitable['Price'].mean()

# use count to find the number of items a user bought
purchase_count = most_profitable['Item Name'].count()



# use variables above to create a table that displays the most profitable items
most_prof_items = pd.DataFrame({'Purchase Count' : purchase_count
                                , 'Item Price' : avg_profitable_price.map("${:,.2f}".format)
                                , 'Total Purchase Value': most_profitable_item.map("${:,.2f}".format)})


# sort by descending so the most profitable item shows up first
most_prof_items = most_prof_items.sort_values(['Total Purchase Value'], ascending = False)

most_prof_items.head()


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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>170</th>
      <th>Shadowsteel</th>
      <td>$1.98</td>
      <td>5</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>21</th>
      <th>Souleater</th>
      <td>$3.27</td>
      <td>3</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>37</th>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>$1.93</td>
      <td>5</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>127</th>
      <th>Heartseeker, Reaver of Souls</th>
      <td>$3.21</td>
      <td>3</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>120</th>
      <th>Agatha</th>
      <td>$1.91</td>
      <td>5</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>


