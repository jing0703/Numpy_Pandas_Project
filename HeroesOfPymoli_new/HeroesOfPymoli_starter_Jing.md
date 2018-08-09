
### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  
-----

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

## Player Count

* Display the total number of players



```python
unique_player = purchase_data["SN"].unique()
Total_Players = len(unique_player)
Total_Players_count = pd.DataFrame({"Total Players": [Total_Players]})
Total_Players_count
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
Unique_Items = len(purchase_data["Item ID"].unique())
Average_Price = purchase_data["Price"].mean()
Number_of_Purchases = sum(purchase_data["Item Name"].value_counts())
Total_Revenue = purchase_data["Price"].sum()
Purchasing_df = pd.DataFrame({"Number of Unique Items": [Unique_Items],"Average Price":[Average_Price],"Number of Purchases":[Number_of_Purchases],"Total Revenue":[Total_Revenue]})
Purchasing_df['Average Price']= Purchasing_df['Average Price'].map("${:.2f}".format)
Purchasing_df['Total Revenue']= Purchasing_df['Total Revenue'].map("${:.2f}".format)
organized_Purchasing_df = Purchasing_df[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
organized_Purchasing_df
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
Gender_df = purchase_data.groupby('Gender')['SN'].nunique()
Gender_percent = (Gender_df / Total_Players) *100
Gender_Demographics = pd.concat([Gender_percent,Gender_df], axis=1,keys=['Percentage of Players','Total Count'])
Gender_Demographics['Percentage of Players']= Gender_Demographics['Percentage of Players'].map("{0:.2f}%".format)
Gender_Demographics
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
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>14.06%</td>
      <td>81</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>84.03%</td>
      <td>484</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.91%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, etc. by gender


* For normalized purchasing, divide total purchase value by purchase count, by gender


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
Purchase_Count = purchase_data.groupby('Gender')['Item ID'].count()
Total_Purchase_Value = purchase_data.groupby('Gender')['Price'].sum()
Average_Purchase_Price = Total_Purchase_Value/Purchase_Count
Normalized_Totals = Total_Purchase_Value / Gender_df
Purchasing_Analysis = pd.concat([Purchase_Count,Average_Purchase_Price,Total_Purchase_Value,Normalized_Totals], axis=1,keys=['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals'])
Purchasing_Analysis['Average Purchase Price']= Purchasing_Analysis['Average Purchase Price'].map("${:.2f}".format)
Purchasing_Analysis['Total Purchase Value']= Purchasing_Analysis['Total Purchase Value'].map("${:.2f}".format)
Purchasing_Analysis['Normalized Totals']= Purchasing_Analysis['Normalized Totals'].map("${:.2f}".format)
Purchasing_Analysis
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
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
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
purchase_data["age group"] = pd.cut(purchase_data["Age"], age_bins,labels=group_names)
age_count = purchase_data.groupby("age group")['SN'].nunique()
age_percent = (age_count / Total_Players) *100
Age_Demographics = pd.concat([age_percent,age_count], axis=1,keys=['Percentage of Players','Total Count'])
Age_Demographics['Percentage of Players']= Age_Demographics['Percentage of Players'].map("{0:.2f}%".format)
Age_Demographics
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
      <th>age group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>2.95%</td>
      <td>17</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.82%</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>18.58%</td>
      <td>107</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>44.79%</td>
      <td>258</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>13.37%</td>
      <td>77</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>9.03%</td>
      <td>52</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.38%</td>
      <td>31</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.08%</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, etc. in the table below


* Calculate Normalized Purchasing


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
Purchase_Count = purchase_data.groupby("age group")['Item ID'].count()
Average_Purchase_Price = purchase_data.groupby("age group")['Price'].mean()
Total_Purchase_Value = purchase_data.groupby("age group")['Price'].sum()
Normalized_Totals = Total_Purchase_Value / age_count
Purchasing_age = pd.concat([Purchase_Count,Average_Purchase_Price,Total_Purchase_Value,Normalized_Totals], axis=1,keys=['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals'])
Purchasing_age['Average Purchase Price']= Purchasing_age['Average Purchase Price'].map("${:.2f}".format)
Purchasing_age['Total Purchase Value']= Purchasing_age['Total Purchase Value'].map("${:.2f}".format)
Purchasing_age['Normalized Totals']= Purchasing_age['Normalized Totals'].map("${:.2f}".format)
Purchasing_age
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>age group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
Spenders_Average_Price = purchase_data.groupby('SN')['Price'].mean()
Spenders_Total_Purchase = purchase_data.groupby('SN')['Price'].sum()
Spenders_Purchase_Count = purchase_data.groupby('SN')['Item ID'].count()
Spenders = pd.concat([Spenders_Purchase_Count,Spenders_Average_Price,Spenders_Total_Purchase], axis=1,keys=['Purchase Count','Average Purchase Price','Total Purchase Value'])
Spenders['Average Purchase Price']= Spenders['Average Purchase Price'].map("${:.2f}".format)
Spenders['Total Purchase Value']= Spenders['Total Purchase Value'].map("${:.2f}".format)
top_spenders = Spenders.sort_values('Purchase Count',ascending=False)
top_spenders.head(n=5)
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Asur53</th>
      <td>3</td>
      <td>$2.48</td>
      <td>$7.44</td>
    </tr>
    <tr>
      <th>Inguron55</th>
      <td>3</td>
      <td>$3.70</td>
      <td>$11.11</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
purchase_data['Purchase Count'] = purchase_data.groupby(['Item Name'])['Item ID'].transform('count')
purchase_data['Total Purchase Value'] = purchase_data.groupby(['Item Name'])['Price'].transform('sum')
no_dup = purchase_data.drop_duplicates(subset=['Item Name'])
popular = no_dup.sort_values('Purchase Count',ascending=False)
popular.set_index(['Item ID','Item Name'], inplace=True)
popular_item = popular[['Purchase Count','Price','Total Purchase Value']]
popular_item['Price']= popular_item['Price'].map("${:.2f}".format)
popular_item['Total Purchase Value']= popular_item['Total Purchase Value'].map("${:.2f}".format)
renamed_df = popular_item.rename(columns={"Price":"Item Price"})
renamed_df.head(n=5)
```

    /anaconda3/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:11: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      # This is added back by InteractiveShellApp.init_path()
    /anaconda3/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:12: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if sys.path[0] == '':





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
      <th>Purchase Count</th>
      <th>Item Price</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.88</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
Profitable_Items = no_dup.sort_values('Total Purchase Value',ascending=False)
Profitable_Items.set_index(['Item ID','Item Name'], inplace=True)
Profitable_df = Profitable_Items[['Purchase Count','Price','Total Purchase Value']]
Profitable_df['Price']= Profitable_df['Price'].map("${:.2f}".format)
Profitable_df['Total Purchase Value']= Profitable_df['Total Purchase Value'].map("${:.2f}".format)
renamed = Profitable_df.rename(columns={"Price":"Item Price"})
renamed.head(n=5)
```

    /anaconda3/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.
    /anaconda3/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





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
      <th>Purchase Count</th>
      <th>Item Price</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.88</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


