
# Opioid Data Analysis and Visualization 


```python
#import dependencies

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
census_csv = "census_income.csv"
opioid_years_csv = "detailed_mortality.csv"
```


```python
opioid_df = pd.read_csv(opioid_years_csv)
opioid_df["Year"] = opioid_df["Year"].apply(int) #applies int to all of the years
```


```python
opioid_df.groupby("Year")["Deaths"].sum().reset_index()
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
      <th>Year</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2009</td>
      <td>19000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>20269.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2011</td>
      <td>22869.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>20603.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>24346.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2014</td>
      <td>27728.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2015</td>
      <td>31951.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016</td>
      <td>40476.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
census_df = pd.read_csv(census_csv)
census_df.sort_values(by = "Poverty Rate",ascending = False).head()
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
      <th>State</th>
      <th>Population</th>
      <th>Number of Persons in Poverty</th>
      <th>Median Household Income</th>
      <th>Per Capita Income</th>
      <th>Year</th>
      <th>Poverty Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>351</th>
      <td>Puerto Rico</td>
      <td>3583073.0</td>
      <td>1616117.0</td>
      <td>19350.0</td>
      <td>11394.0</td>
      <td>2015</td>
      <td>45.104216</td>
    </tr>
    <tr>
      <th>311</th>
      <td>Puerto Rico</td>
      <td>3638965.0</td>
      <td>1630965.0</td>
      <td>19686.0</td>
      <td>11331.0</td>
      <td>2014</td>
      <td>44.819475</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Puerto Rico</td>
      <td>3742586.0</td>
      <td>1676055.0</td>
      <td>19122.0</td>
      <td>10658.0</td>
      <td>2011</td>
      <td>44.783340</td>
    </tr>
    <tr>
      <th>415</th>
      <td>Puerto Rico</td>
      <td>3529385.0</td>
      <td>1577075.0</td>
      <td>19606.0</td>
      <td>11688.0</td>
      <td>2016</td>
      <td>44.684131</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Puerto Rico</td>
      <td>3762322.0</td>
      <td>1680370.0</td>
      <td>18791.0</td>
      <td>10355.0</td>
      <td>2010</td>
      <td>44.663110</td>
    </tr>
  </tbody>
</table>
</div>




```python
merged_df = pd.merge(census_df,opioid_df,how="outer",on=["State","Year"])
```


```python
#includes all the merged data and dropping all the na's 

opioid_census_df = merged_df.rename(columns={"Population_x":"Population by Race/Age Group/Gender per State",
                          "Population_y":"Total State Population",
                          "Number of Persons in Poverty":"Number of Persons in Poverty per State" })

opioid_census_df.head()

opioid_census_df=opioid_census_df.dropna(how="any")
opioid_census_df.head() 
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
      <th>State</th>
      <th>Population by Race/Age Group/Gender per State</th>
      <th>Number of Persons in Poverty per State</th>
      <th>Median Household Income</th>
      <th>Per Capita Income</th>
      <th>Year</th>
      <th>Poverty Rate</th>
      <th>Race</th>
      <th>Ten-Year Age Groups</th>
      <th>Ten-Year Age Groups Code</th>
      <th>Gender</th>
      <th>Gender Code</th>
      <th>Deaths</th>
      <th>Total State Population</th>
      <th>Crude Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alaska</td>
      <td>683142.0</td>
      <td>64038.0</td>
      <td>64635.0</td>
      <td>29382.0</td>
      <td>2009</td>
      <td>9.374039</td>
      <td>White</td>
      <td>45-54 years</td>
      <td>45-54</td>
      <td>Male</td>
      <td>M</td>
      <td>12.0</td>
      <td>45671.0</td>
      <td>Unreliable</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>4633360.0</td>
      <td>757833.0</td>
      <td>41216.0</td>
      <td>22732.0</td>
      <td>2009</td>
      <td>16.356014</td>
      <td>White</td>
      <td>15-24 years</td>
      <td>15-24</td>
      <td>Male</td>
      <td>M</td>
      <td>14.0</td>
      <td>227585.0</td>
      <td>Unreliable</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alabama</td>
      <td>4633360.0</td>
      <td>757833.0</td>
      <td>41216.0</td>
      <td>22732.0</td>
      <td>2009</td>
      <td>16.356014</td>
      <td>White</td>
      <td>25-34 years</td>
      <td>25-34</td>
      <td>Male</td>
      <td>M</td>
      <td>38.0</td>
      <td>209864.0</td>
      <td>18.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alabama</td>
      <td>4633360.0</td>
      <td>757833.0</td>
      <td>41216.0</td>
      <td>22732.0</td>
      <td>2009</td>
      <td>16.356014</td>
      <td>White</td>
      <td>35-44 years</td>
      <td>35-44</td>
      <td>Female</td>
      <td>F</td>
      <td>25.0</td>
      <td>221942.0</td>
      <td>11.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alabama</td>
      <td>4633360.0</td>
      <td>757833.0</td>
      <td>41216.0</td>
      <td>22732.0</td>
      <td>2009</td>
      <td>16.356014</td>
      <td>White</td>
      <td>35-44 years</td>
      <td>35-44</td>
      <td>Male</td>
      <td>M</td>
      <td>27.0</td>
      <td>226271.0</td>
      <td>11.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Predicting opioid mortality assuming that the death rate is linear

from scipy import stats

test = opioid_df.groupby(["Year"])["Deaths"].sum().reset_index()

o_slope, o_int, o_r, o_p,o_se = stats.linregress(test["Year"],test["Deaths"])

o_fit = o_slope*test["Year"] + o_int
o_fit


plt.plot(test["Year"], test["Deaths"],marker='o', color="black" )
plt.plot(test["Year"], o_fit,marker='o', color="red")
plt.title("Opioid Mortality Prediction",size = 15)

print(f" p-value:{round(o_p,5)} < 0.05, statistically significant")
print(f"R : {o_r}, About 90.9% of the changes seen in death count is described by the changes in Years")

```

     p-value:0.00176 < 0.05, statistically significant
    R : 0.9090177920529392, About 90.9% of the changes seen in death count is described by the changes in Years



![png](output_8_1.png)


# Grouped by Age and Year by State


```python
opioid_census_df.groupby(["State","Ten-Year Age Groups","Year"])["Deaths"].sum().unstack()
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
      <th>Year</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
    </tr>
    <tr>
      <th>State</th>
      <th>Ten-Year Age Groups</th>
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
      <th rowspan="5" valign="top">Alabama</th>
      <th>15-24 years</th>
      <td>14.0</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>15.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>38.0</td>
      <td>34.0</td>
      <td>51.0</td>
      <td>54.0</td>
      <td>60.0</td>
      <td>70.0</td>
      <td>76.0</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>52.0</td>
      <td>35.0</td>
      <td>51.0</td>
      <td>39.0</td>
      <td>50.0</td>
      <td>80.0</td>
      <td>71.0</td>
      <td>87.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>62.0</td>
      <td>49.0</td>
      <td>64.0</td>
      <td>70.0</td>
      <td>56.0</td>
      <td>79.0</td>
      <td>95.0</td>
      <td>73.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>15.0</td>
      <td>10.0</td>
      <td>13.0</td>
      <td>24.0</td>
      <td>36.0</td>
      <td>71.0</td>
      <td>54.0</td>
      <td>67.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Alaska</th>
      <th>25-34 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.0</td>
      <td>13.0</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>10.0</td>
      <td>15.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>12.0</td>
      <td>12.0</td>
      <td>12.0</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">Arizona</th>
      <th>15-24 years</th>
      <td>63.0</td>
      <td>52.0</td>
      <td>56.0</td>
      <td>37.0</td>
      <td>74.0</td>
      <td>52.0</td>
      <td>62.0</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>97.0</td>
      <td>92.0</td>
      <td>94.0</td>
      <td>72.0</td>
      <td>99.0</td>
      <td>125.0</td>
      <td>122.0</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>92.0</td>
      <td>113.0</td>
      <td>107.0</td>
      <td>104.0</td>
      <td>121.0</td>
      <td>121.0</td>
      <td>155.0</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>150.0</td>
      <td>163.0</td>
      <td>158.0</td>
      <td>163.0</td>
      <td>194.0</td>
      <td>183.0</td>
      <td>187.0</td>
      <td>201.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>84.0</td>
      <td>106.0</td>
      <td>109.0</td>
      <td>107.0</td>
      <td>135.0</td>
      <td>177.0</td>
      <td>154.0</td>
      <td>186.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>11.0</td>
      <td>29.0</td>
      <td>33.0</td>
      <td>NaN</td>
      <td>58.0</td>
      <td>44.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>75-84 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Arkansas</th>
      <th>15-24 years</th>
      <td>16.0</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>17.0</td>
      <td>13.0</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>39.0</td>
      <td>46.0</td>
      <td>46.0</td>
      <td>41.0</td>
      <td>38.0</td>
      <td>44.0</td>
      <td>44.0</td>
      <td>58.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>48.0</td>
      <td>41.0</td>
      <td>40.0</td>
      <td>43.0</td>
      <td>40.0</td>
      <td>59.0</td>
      <td>60.0</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>63.0</td>
      <td>52.0</td>
      <td>58.0</td>
      <td>53.0</td>
      <td>41.0</td>
      <td>78.0</td>
      <td>70.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>10.0</td>
      <td>26.0</td>
      <td>NaN</td>
      <td>35.0</td>
      <td>47.0</td>
      <td>40.0</td>
      <td>45.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">California</th>
      <th>15-24 years</th>
      <td>103.0</td>
      <td>123.0</td>
      <td>137.0</td>
      <td>106.0</td>
      <td>112.0</td>
      <td>109.0</td>
      <td>106.0</td>
      <td>137.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>230.0</td>
      <td>230.0</td>
      <td>273.0</td>
      <td>220.0</td>
      <td>282.0</td>
      <td>263.0</td>
      <td>308.0</td>
      <td>311.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>406.0</td>
      <td>317.0</td>
      <td>346.0</td>
      <td>304.0</td>
      <td>336.0</td>
      <td>303.0</td>
      <td>346.0</td>
      <td>328.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>730.0</td>
      <td>663.0</td>
      <td>690.0</td>
      <td>602.0</td>
      <td>643.0</td>
      <td>623.0</td>
      <td>586.0</td>
      <td>580.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>509.0</td>
      <td>470.0</td>
      <td>556.0</td>
      <td>574.0</td>
      <td>600.0</td>
      <td>648.0</td>
      <td>689.0</td>
      <td>759.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>104.0</td>
      <td>93.0</td>
      <td>114.0</td>
      <td>109.0</td>
      <td>173.0</td>
      <td>190.0</td>
      <td>205.0</td>
      <td>229.0</td>
    </tr>
    <tr>
      <th>75-84 years</th>
      <td>26.0</td>
      <td>14.0</td>
      <td>33.0</td>
      <td>30.0</td>
      <td>43.0</td>
      <td>32.0</td>
      <td>45.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>85+ years</th>
      <td>NaN</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>12.0</td>
      <td>15.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>Colorado</th>
      <th>15-24 years</th>
      <td>21.0</td>
      <td>19.0</td>
      <td>20.0</td>
      <td>21.0</td>
      <td>41.0</td>
      <td>31.0</td>
      <td>40.0</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
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
      <th rowspan="2" valign="top">Vermont</th>
      <th>55-64 years</th>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Virginia</th>
      <th>15-24 years</th>
      <td>44.0</td>
      <td>16.0</td>
      <td>51.0</td>
      <td>35.0</td>
      <td>44.0</td>
      <td>54.0</td>
      <td>52.0</td>
      <td>84.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>76.0</td>
      <td>83.0</td>
      <td>95.0</td>
      <td>86.0</td>
      <td>160.0</td>
      <td>153.0</td>
      <td>192.0</td>
      <td>279.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>125.0</td>
      <td>79.0</td>
      <td>133.0</td>
      <td>102.0</td>
      <td>129.0</td>
      <td>181.0</td>
      <td>185.0</td>
      <td>220.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>134.0</td>
      <td>96.0</td>
      <td>138.0</td>
      <td>107.0</td>
      <td>159.0</td>
      <td>164.0</td>
      <td>159.0</td>
      <td>228.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>38.0</td>
      <td>39.0</td>
      <td>50.0</td>
      <td>36.0</td>
      <td>94.0</td>
      <td>104.0</td>
      <td>111.0</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">Washington</th>
      <th>15-24 years</th>
      <td>35.0</td>
      <td>39.0</td>
      <td>19.0</td>
      <td>28.0</td>
      <td>43.0</td>
      <td>36.0</td>
      <td>34.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>89.0</td>
      <td>71.0</td>
      <td>96.0</td>
      <td>89.0</td>
      <td>87.0</td>
      <td>81.0</td>
      <td>116.0</td>
      <td>104.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>126.0</td>
      <td>103.0</td>
      <td>101.0</td>
      <td>96.0</td>
      <td>103.0</td>
      <td>96.0</td>
      <td>99.0</td>
      <td>118.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>170.0</td>
      <td>194.0</td>
      <td>182.0</td>
      <td>168.0</td>
      <td>161.0</td>
      <td>179.0</td>
      <td>172.0</td>
      <td>164.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>127.0</td>
      <td>122.0</td>
      <td>138.0</td>
      <td>135.0</td>
      <td>137.0</td>
      <td>148.0</td>
      <td>170.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>20.0</td>
      <td>11.0</td>
      <td>28.0</td>
      <td>32.0</td>
      <td>12.0</td>
      <td>45.0</td>
      <td>59.0</td>
      <td>66.0</td>
    </tr>
    <tr>
      <th>75-84 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>85+ years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>14.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">West Virginia</th>
      <th>15-24 years</th>
      <td>NaN</td>
      <td>40.0</td>
      <td>36.0</td>
      <td>13.0</td>
      <td>18.0</td>
      <td>24.0</td>
      <td>38.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>48.0</td>
      <td>117.0</td>
      <td>154.0</td>
      <td>115.0</td>
      <td>140.0</td>
      <td>130.0</td>
      <td>123.0</td>
      <td>176.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>47.0</td>
      <td>127.0</td>
      <td>155.0</td>
      <td>165.0</td>
      <td>130.0</td>
      <td>152.0</td>
      <td>173.0</td>
      <td>231.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>52.0</td>
      <td>111.0</td>
      <td>158.0</td>
      <td>152.0</td>
      <td>144.0</td>
      <td>168.0</td>
      <td>168.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>NaN</td>
      <td>47.0</td>
      <td>58.0</td>
      <td>61.0</td>
      <td>64.0</td>
      <td>80.0</td>
      <td>101.0</td>
      <td>97.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Wisconsin</th>
      <th>15-24 years</th>
      <td>12.0</td>
      <td>24.0</td>
      <td>28.0</td>
      <td>20.0</td>
      <td>50.0</td>
      <td>39.0</td>
      <td>29.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>34.0</td>
      <td>55.0</td>
      <td>81.0</td>
      <td>90.0</td>
      <td>89.0</td>
      <td>97.0</td>
      <td>114.0</td>
      <td>162.0</td>
    </tr>
    <tr>
      <th>35-44 years</th>
      <td>52.0</td>
      <td>68.0</td>
      <td>57.0</td>
      <td>73.0</td>
      <td>87.0</td>
      <td>96.0</td>
      <td>94.0</td>
      <td>150.0</td>
    </tr>
    <tr>
      <th>45-54 years</th>
      <td>72.0</td>
      <td>90.0</td>
      <td>110.0</td>
      <td>101.0</td>
      <td>113.0</td>
      <td>124.0</td>
      <td>122.0</td>
      <td>145.0</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>40.0</td>
      <td>39.0</td>
      <td>69.0</td>
      <td>63.0</td>
      <td>81.0</td>
      <td>77.0</td>
      <td>79.0</td>
      <td>117.0</td>
    </tr>
    <tr>
      <th>65-74 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Wyoming</th>
      <th>45-54 years</th>
      <td>NaN</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55-64 years</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>277 rows × 8 columns</p>
</div>



# GroupBy Race



```python
total_death_population = opioid_census_df["Deaths"].sum()

race_df = opioid_census_df.groupby(["Race"])["Deaths"].sum().reset_index()
race_df["Death Rate"] = (race_df["Deaths"]/total_death_population)*100
race_df
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
      <th>Race</th>
      <th>Deaths</th>
      <th>Death Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>American Indian or Alaska Native</td>
      <td>75.0</td>
      <td>0.036190</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Asian or Pacific Islander</td>
      <td>320.0</td>
      <td>0.154409</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Black or African American</td>
      <td>17894.0</td>
      <td>8.634350</td>
    </tr>
    <tr>
      <th>3</th>
      <td>White</td>
      <td>188953.0</td>
      <td>91.175051</td>
    </tr>
  </tbody>
</table>
</div>




```python
by_race = opioid_census_df.groupby(["Ten-Year Age Groups","Race"])["Deaths"].sum()
by_race.unstack()

by_race = opioid_census_df.groupby(["Ten-Year Age Groups","Race"])["Deaths"].sum()
by_race= by_race.reset_index()
by_race.head()

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
      <th>Ten-Year Age Groups</th>
      <th>Race</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15-24 years</td>
      <td>Asian or Pacific Islander</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15-24 years</td>
      <td>Black or African American</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-24 years</td>
      <td>White</td>
      <td>14525.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25-34 years</td>
      <td>American Indian or Alaska Native</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-34 years</td>
      <td>Asian or Pacific Islander</td>
      <td>65.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#bar visualization 
fig, ax = plt.subplots()
fig.set_size_inches(11.7, 8.27)


race_mortality = sns.barplot(x="Race",y="Deaths",data=by_race,hue="Ten-Year Age Groups",palette="viridis")
plt.title("Mortality By Age Groups Across Race",size=20)
plt.xlabel("")
plt.xticks(size=10)
plt.ylabel("Deaths",size=15)
plt.ylim(0,max(by_race["Deaths"]) + 1500)
```




    (0, 55103.0)




![png](output_14_1.png)



```python
#pie chart with focus on white death population

white_race = by_race.loc[by_race["Race"] == "White"]
white_death_pop = white_race["Deaths"].sum()
white_race["Death Rate"] = (white_race["Deaths"] / white_death_pop)
white_race = white_race.reset_index(drop=True)
condensed_white = white_race[:-3]

condensed_white.loc[5] = ["65+ years","White",5656,0.029934]
condensed_white
```

    /Users/EricaLei/anaconda/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    /Users/EricaLei/anaconda/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':





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
      <th>Ten-Year Age Groups</th>
      <th>Race</th>
      <th>Deaths</th>
      <th>Death Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15-24 years</td>
      <td>White</td>
      <td>14525.0</td>
      <td>0.076871</td>
    </tr>
    <tr>
      <th>1</th>
      <td>25-34 years</td>
      <td>White</td>
      <td>40560.0</td>
      <td>0.214657</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35-44 years</td>
      <td>White</td>
      <td>42005.0</td>
      <td>0.222304</td>
    </tr>
    <tr>
      <th>3</th>
      <td>45-54 years</td>
      <td>White</td>
      <td>53603.0</td>
      <td>0.283684</td>
    </tr>
    <tr>
      <th>4</th>
      <td>55-64 years</td>
      <td>White</td>
      <td>32604.0</td>
      <td>0.172551</td>
    </tr>
    <tr>
      <th>5</th>
      <td>65+ years</td>
      <td>White</td>
      <td>5656.0</td>
      <td>0.029934</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots()
fig.set_size_inches(8.27, 8.27)

colors = ["#f68f46ff","#3b518bff","#31678dff","#97d73eff","#21918dff","#3cb875ff"]
labels = condensed_white["Ten-Year Age Groups"]
sizes = condensed_white["Death Rate"]

    
explode = (0.05,0.05,0.05,0.05,0.05,0.05)
plt.pie(sizes,labels=labels,colors = colors,autopct='%1.1f%%',startangle=90,pctdistance=0.85,explode=explode)

centre_circle =  plt.Circle((0,0),0.70,fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)

plt.title("White Opioid Mortality by Age Groups",size=20)
```




    <matplotlib.text.Text at 0x1a15387278>




![png](output_16_1.png)


# Group by Income Per Capita


```python
bins=(10000.0,15000.0,20000.0,25000.0,30000.0,35000.0,40000.0,50000.0)
group_name=["10-15","15-20","20-25","25-30","30-35","35-40","40-50"]


income_deaths_df = pd.cut(opioid_census_df["Per Capita Income"],bins,labels=group_name)
income_deaths_df

opioid_census_df["IncomeGroups"] = income_deaths_df
```


```python
value=0
income_deaths_df = opioid_census_df.groupby(["IncomeGroups"] )["Deaths"].sum().reset_index()
income_deaths_df["Deaths"] = income_deaths_df["Deaths"].fillna(value).apply(int)
income_deaths_sum = income_deaths_df["Deaths"].sum()
income_deaths_df["DeathRate"] =(income_deaths_df["Deaths"]/income_deaths_sum)*100
income_deaths_df
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
      <th>IncomeGroups</th>
      <th>Deaths</th>
      <th>DeathRate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10-15</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15-20</td>
      <td>174</td>
      <td>0.083960</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20-25</td>
      <td>34769</td>
      <td>16.777005</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25-30</td>
      <td>107580</td>
      <td>51.910327</td>
    </tr>
    <tr>
      <th>4</th>
      <td>30-35</td>
      <td>43756</td>
      <td>21.113481</td>
    </tr>
    <tr>
      <th>5</th>
      <td>35-40</td>
      <td>20253</td>
      <td>9.772633</td>
    </tr>
    <tr>
      <th>6</th>
      <td>40-50</td>
      <td>710</td>
      <td>0.342595</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7, 8.27)

mortality_income = sns.barplot(x="IncomeGroups", y="Deaths", data=income_deaths_df, palette="viridis")

for index, row in income_deaths_df.iterrows():
    mortality_income.text(row.name,row.Deaths,row.Deaths, color="black",ha="center",fontsize=13)
plt.title("Death Rate by Income Groups",size=20)
plt.xlabel("Annual Income Per Capita (in thousands of dollars)",size=15)
plt.ylabel("Deaths",size=15)
plt.ylim(0,max(income_deaths_df["Deaths"])+15000)
```




    (0, 122580)




![png](output_20_1.png)


# Number of Deaths Across Time and State



```python
totaldeath_state = opioid_df.groupby(["State"])["Deaths"].sum().reset_index().sort_values(
    by="Deaths",ascending=False).reset_index(drop=True)

top_totaldeath_state = totaldeath_state.head(10)
top_totaldeath_state
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
      <th>State</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Florida</td>
      <td>18716.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>California</td>
      <td>17209.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>16698.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ohio</td>
      <td>11402.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Texas</td>
      <td>10531.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Michigan</td>
      <td>9614.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Massachusetts</td>
      <td>9514.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Pennsylvania</td>
      <td>7863.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>North Carolina</td>
      <td>6623.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>New Jersey</td>
      <td>6108.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
year_state_deaths = opioid_df.loc[(opioid_df["State"] == "Florida") | 
             (opioid_df["State"] == "California") |
              (opioid_df["State"]== "New York") | 
              (opioid_df["State"] == "Ohio") |
              (opioid_df["State"] == "Texas") | 
              (opioid_df["State"] == "Michigan") | 
              (opioid_df["State"] == "Massachusetts") | 
              (opioid_df["State"] == "Pennsylvania") | 
              (opioid_df["State"] == "North Carolina") | 
              (opioid_df["State"] == "New Jersey"), ["Year","State","Deaths"]]
year_state_deaths_df = year_state_deaths.groupby(["Year","State"])["Deaths"].sum().reset_index()
year_state_deaths_df.head()
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
      <th>Year</th>
      <th>State</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2009</td>
      <td>California</td>
      <td>2108.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2009</td>
      <td>Florida</td>
      <td>2267.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2009</td>
      <td>Massachusetts</td>
      <td>754.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2009</td>
      <td>Michigan</td>
      <td>1038.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2009</td>
      <td>New Jersey</td>
      <td>125.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7, 8.27)
sns.pointplot(x="Year",y="Deaths", data=year_state_deaths_df,hue="State",palette="viridis")
plt.grid(linestyle="dashed")
plt.xlabel("Years", size = 15)
plt.ylabel("Deaths", size = 15)
plt.title ("Top 10 States: Opioid Mortality Across Time", size=20)
```




    <matplotlib.text.Text at 0x1a15befd68>




![png](output_24_1.png)



```python
top5_year_state_deaths = opioid_df.loc[(opioid_df["State"] == "Florida") | 
             (opioid_df["State"] == "California") |
              (opioid_df["State"]== "New York") | 
              (opioid_df["State"] == "Ohio") |
              (opioid_df["State"] == "Texas") , ["Year","State","Deaths"]]
year5_state_deaths_df = top5_year_state_deaths.groupby(["Year","State"])["Deaths"].sum().reset_index()
year5_state_deaths_df.head()
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
      <th>Year</th>
      <th>State</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2009</td>
      <td>California</td>
      <td>2108.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2009</td>
      <td>Florida</td>
      <td>2267.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2009</td>
      <td>New York</td>
      <td>1589.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2009</td>
      <td>Ohio</td>
      <td>644.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2009</td>
      <td>Texas</td>
      <td>1275.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7, 8.27)
sns.pointplot(x="Year",y="Deaths", data=year5_state_deaths_df,hue="State",palette="viridis")
plt.grid(linestyle="dashed")
plt.xlabel("Years", size = 15)
plt.ylabel("Deaths", size = 15)
plt.title ("Top 5 States: Opioid Mortality Across Time", size=20)
plt.savefig("top5states.png")
```


![png](output_26_0.png)



```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7,8.27)
sns.barplot(x="Year", y="Deaths",data=year_state_deaths_df, hue="State", palette="viridis")
plt.xlabel("Years", size = 15)
plt.ylabel("Deaths", size = 15)
plt.title ("Top 10 States: Opioid Mortality Across Time", size=20)
```




    <matplotlib.text.Text at 0x1a1631e438>




![png](output_27_1.png)



```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7,8.27)
sns.barplot(x="Year", y="Deaths",data=year5_state_deaths_df, hue="State", palette="viridis")
plt.xlabel("Years", size = 15)
plt.ylabel("Deaths", size = 15)
plt.title ("Top 5 States: Opioid Mortality Across Time", size=20)

```




    <matplotlib.text.Text at 0x1a16b8f978>




![png](output_28_1.png)

