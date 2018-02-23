

```python
import pandas as pd
```


```python
# Read json files into dataframe
purch_data = pd.read_json('purchase_data.json')
purch_data2 = pd.read_json('purchase_data2.json')
```


```python
# Combine data frames into one
frames = [purch_data, purch_data2]
master_df = pd.concat(frames)
master_df.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Drop any NA rows and get player count
master_df.dropna(how='any')
player_count = len(master_df['SN'].unique())
pc_df = pd.DataFrame({'Total Players': [player_count]})
pc_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>612</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis
unique_item_count = len(master_df['Item Name'].unique())
avg_purch_price = round(master_df['Price'].mean(),2)
purchases = len(master_df.index)
total_revenue = round(master_df['Price'].sum(),2)
# Create output dataframe
output1 = pd.DataFrame({
    'Number of Unique Items': [unique_item_count], 
    'Average Price': '$'+str(avg_purch_price), 
    'Number of Purchases': purchases, 
    'Total Revenue': '$'+str(total_revenue)
})
output1 = output1[['Number of Unique Items', 'Average Price', 'Number of Purchases', 'Total Revenue']]
output1
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>180</td>
      <td>$2.93</td>
      <td>858</td>
      <td>$2514.43</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
male_count = master_df['Gender'].value_counts()['Male']
female_count = master_df['Gender'].value_counts()['Female']
other_count = master_df['Gender'].value_counts()['Other / Non-Disclosed']
male_percent = round(male_count / purchases*100, 2)
female_percent = round(female_count / purchases*100, 2)
other_percent = round(other_count / purchases*100, 2)
output2 = pd.DataFrame(
    data=[[female_percent, female_count], [male_percent, male_count], [other_percent, other_count]], 
    columns=['Percentage of Players', 'Total Count'], 
    index=['Female', 'Male', 'Other / Non-Disclosed']
)
output2
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Female</th>
      <td>17.37</td>
      <td>149</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.24</td>
      <td>697</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
group_count = master_df.groupby('Gender').count()
output3 = group_count[['Age']]
output3.columns = ['Purchase Count']
output3['Average Purchase Price'] = master_df.groupby('Gender').mean()[['Price']]
output3['Total Purchase Value'] = master_df.groupby('Gender').sum()[['Price']]
output3['Average Purchase Price'] = output3['Average Purchase Price'].apply(lambda x: '$'+str(round(x,2)))
output3['Total Purchase Value'] = output3['Total Purchase Value'].apply(lambda x: '$'+str(round(x,2)))
output3



```

    C:\Users\jakec\Anaconda3\lib\site-packages\ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    C:\Users\jakec\Anaconda3\lib\site-packages\ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    C:\Users\jakec\Anaconda3\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    C:\Users\jakec\Anaconda3\lib\site-packages\ipykernel_launcher.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>149</td>
      <td>$2.85</td>
      <td>$424.29</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>697</td>
      <td>$2.94</td>
      <td>$2052.28</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>12</td>
      <td>$3.15</td>
      <td>$37.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
master_df['Age'].max()
```




    45




```python
# Age demographics
bins = [0, 10, 15, 20, 25, 30, 35, 40, 150]

bin_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', 
             '40+']

master_df['Age Group'] = pd.cut(master_df['Age'], bins, right=False, labels=bin_names)

output4 = master_df.groupby('Age Group').count()[['Age']]
output4.columns = ['Purchase Count']
output4['Average Purchase Price'] = master_df.groupby('Age Group').mean()['Price']
output4['Total Purchase Value'] = master_df.groupby('Age Group').sum()['Price']
output4['Average Purchase Price'] = output4['Average Purchase Price'].apply(lambda x: '$'+str(round(x,2)))
output4['Total Purchase Value'] = output4['Total Purchase Value'].apply(lambda x: '$'+str(round(x,2)))
output4

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>33</td>
      <td>$2.95</td>
      <td>$97.28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>38</td>
      <td>$2.79</td>
      <td>$105.91</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>144</td>
      <td>$2.89</td>
      <td>$416.83</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>372</td>
      <td>$2.92</td>
      <td>$1087.66</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>134</td>
      <td>$2.96</td>
      <td>$396.44</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>71</td>
      <td>$2.97</td>
      <td>$211.14</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>48</td>
      <td>$2.93</td>
      <td>$140.77</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>18</td>
      <td>$3.24</td>
      <td>$58.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
top_spenders = master_df.groupby('SN').sum()[['Price']]
top_spenders = top_spenders.sort_values('Price', ascending=False)
top_spenders_mean = master_df.groupby('SN').mean()[['Price']]
top_spenders_mean
output5 = pd.merge(top_spenders, top_spenders_mean, how='left', left_index=True, right_index=True)
top_spenders_count = master_df.groupby('SN').count()[['Price']]
output5 = pd.merge(output5, top_spenders_count, how='left', left_index=True, right_index=True)
output5.columns = ['Total Purchase Value', 'Average Purchase Price', 'Purchase Count']
output5 = output5[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value']]
output5 = output5.iloc[:5]
output5['Average Purchase Price'] = output5['Average Purchase Price'].apply(lambda x: '$'+str(round(x,2)))
output5['Total Purchase Value'] = output5['Total Purchase Value'].apply(lambda x: '$'+str(round(x,2)))
output5
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Aerithllora36</th>
      <td>4</td>
      <td>$3.77</td>
      <td>$15.1</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>4</td>
      <td>$3.25</td>
      <td>$13.02</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
top_items = master_df.groupby(['Item Name', 'Item ID']).count()[['Price']].sort_values('Price', ascending=False)
top_items.reset_index(inplace=True)
top_items.columns = ['Item Name', 'Item ID', 'Purchase Count']
master_df1 = master_df[['Item Name', 'Price']]
output6 = top_items.merge(master_df1, on='Item Name', how='left')
output6.drop_duplicates(subset='Item Name', inplace=True)
top_items2 = master_df1.groupby('Item Name').sum()
top_items2.reset_index(inplace=True)
top_items2.columns = ['Item Name', 'Total Purchase Value']
output6 = output6.merge(top_items2, on='Item Name', how='left')
output6 = output6[['Item ID', 'Item Name', 'Purchase Count', 'Price', 'Total Purchase Value']]
output6 = output6.iloc[:5]
output6['Price'] = output6['Price'].apply(lambda x: '$'+str(round(x,2)))
output6['Total Purchase Value'] = output6['Total Purchase Value'].apply(lambda x: '$'+str(round(x,2)))
output6

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>12</td>
      <td>$2.23</td>
      <td>$29.34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>Trickster</td>
      <td>10</td>
      <td>$2.07</td>
      <td>$23.22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>9</td>
      <td>$2.19</td>
      <td>$23.55</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>Serenity</td>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
prof_items = master_df.groupby(['Item Name', 'Item ID']).sum()[['Price']].sort_values('Price', ascending=False)
prof_items.reset_index(inplace=True)
prof_items.columns = [['Item Name', 'Item ID', 'Total Purchase Value']]
group_count = master_df.groupby('Item Name').count()[['Price']]
group_count.reset_index(inplace=True)
group_count.columns = ['Item Name', 'Purchase Count']
output7 = prof_items.merge(group_count, how='left', on='Item Name')
output7 = output7.merge(master_df1, how='left', on='Item Name')
output7.drop_duplicates(subset='Item Name', inplace=True)
output7 = output7[['Item ID', 'Item Name', 'Purchase Count', 'Price', 'Total Purchase Value']]
output7 = output7.iloc[:5]
output7['Price'] = output7['Price'].apply(lambda x: '$'+str(round(x,2)))
output7['Total Purchase Value'] = output7['Total Purchase Value'].apply(lambda x: '$'+str(round(x,2)))
output7
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>9</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>9</td>
      <td>$3.61</td>
      <td>$33.03</td>
    </tr>
    <tr>
      <th>18</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>25</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.7</td>
    </tr>
    <tr>
      <th>31</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>12</td>
      <td>$2.23</td>
      <td>$29.34</td>
    </tr>
  </tbody>
</table>
</div>


