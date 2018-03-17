

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import csv
import os
import seaborn as sns
import plotly.plotly as py
import plotly.tools as tls
from ipyleaflet import Map
%matplotlib inline
sns.set(style="white", rc={"axes.facecolor": (0, 0, 0, 0)})
```


```python
pwd
```




    '/Users/stephaniechanshomefolder/Desktop/Opioid-Analysis-'




```python
prescription_df = pd.read_csv('/Users/stephaniechanshomefolder/Desktop/Opioid-Analysis-/Prescription_Rate.csv')
prescription_df.head()
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
      <th>State ABBR</th>
      <th>2006 Prescribing Rate</th>
      <th>2007 Prescribing Rate</th>
      <th>2008 Prescribing Rate</th>
      <th>2009 Prescribing Rate</th>
      <th>2010 Prescribing Rate</th>
      <th>2011 Prescribing Rate</th>
      <th>2012 Prescribing Rate</th>
      <th>2013 Prescribing Rate</th>
      <th>2014 Prescribing Rate</th>
      <th>2015 Prescribing Rate</th>
      <th>2016 Prescribing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>115.6</td>
      <td>120.3</td>
      <td>126.1</td>
      <td>131.6</td>
      <td>134.3</td>
      <td>136.6</td>
      <td>143.8</td>
      <td>142.4</td>
      <td>135.2</td>
      <td>125.0</td>
      <td>121.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>AK</td>
      <td>63.4</td>
      <td>66.6</td>
      <td>68.5</td>
      <td>67.3</td>
      <td>68.4</td>
      <td>68.0</td>
      <td>66.8</td>
      <td>63.7</td>
      <td>62.7</td>
      <td>60.8</td>
      <td>58.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>AZ</td>
      <td>74.3</td>
      <td>77.8</td>
      <td>80.9</td>
      <td>84.2</td>
      <td>88.5</td>
      <td>88.6</td>
      <td>85.3</td>
      <td>80.4</td>
      <td>79.7</td>
      <td>75.5</td>
      <td>70.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>AR</td>
      <td>98.3</td>
      <td>108.2</td>
      <td>112.1</td>
      <td>116.0</td>
      <td>120.8</td>
      <td>115.2</td>
      <td>121.8</td>
      <td>120.9</td>
      <td>123.2</td>
      <td>117.2</td>
      <td>114.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>CA</td>
      <td>51.0</td>
      <td>53.6</td>
      <td>55.1</td>
      <td>55.6</td>
      <td>55.8</td>
      <td>55.9</td>
      <td>56.4</td>
      <td>54.4</td>
      <td>52.7</td>
      <td>47.7</td>
      <td>44.8</td>
    </tr>
  </tbody>
</table>
</div>




```python
prescription_df.rename(columns={'2009 Prescribing Rate': '2009', 
                                '2010 Prescribing Rate': '2010',
                                '2011 Prescribing Rate': '2011',
                                '2012 Prescribing Rate': '2012',
                                '2013 Prescribing Rate': '2013',
                                '2014 Prescribing Rate': '2014',
                                '2015 Prescribing Rate': '2015',
                                '2016 Prescribing Rate': '2016'
                               }, 
                       inplace=True)
newdf=prescription_df.drop(['State ABBR', 
                      '2006 Prescribing Rate', 
                      '2007 Prescribing Rate', 
                      '2008 Prescribing Rate'],
                      axis=1)
# newdf.set_index('State')
newdf
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
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>131.6</td>
      <td>134.3</td>
      <td>136.6</td>
      <td>143.8</td>
      <td>142.4</td>
      <td>135.2</td>
      <td>125.0</td>
      <td>121.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>67.3</td>
      <td>68.4</td>
      <td>68.0</td>
      <td>66.8</td>
      <td>63.7</td>
      <td>62.7</td>
      <td>60.8</td>
      <td>58.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>84.2</td>
      <td>88.5</td>
      <td>88.6</td>
      <td>85.3</td>
      <td>80.4</td>
      <td>79.7</td>
      <td>75.5</td>
      <td>70.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>116.0</td>
      <td>120.8</td>
      <td>115.2</td>
      <td>121.8</td>
      <td>120.9</td>
      <td>123.2</td>
      <td>117.2</td>
      <td>114.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>55.6</td>
      <td>55.8</td>
      <td>55.9</td>
      <td>56.4</td>
      <td>54.4</td>
      <td>52.7</td>
      <td>47.7</td>
      <td>44.8</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Colorado</td>
      <td>69.8</td>
      <td>72.0</td>
      <td>73.0</td>
      <td>73.5</td>
      <td>71.2</td>
      <td>69.6</td>
      <td>65.1</td>
      <td>59.8</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Connecticut</td>
      <td>68.1</td>
      <td>68.6</td>
      <td>69.1</td>
      <td>69.3</td>
      <td>67.4</td>
      <td>66.0</td>
      <td>62.3</td>
      <td>55.9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Delaware</td>
      <td>97.5</td>
      <td>101.1</td>
      <td>99.7</td>
      <td>94.0</td>
      <td>92.7</td>
      <td>91.0</td>
      <td>84.4</td>
      <td>79.2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>District of Columbia</td>
      <td>34.4</td>
      <td>37.1</td>
      <td>39.8</td>
      <td>40.3</td>
      <td>41.1</td>
      <td>40.1</td>
      <td>35.7</td>
      <td>32.5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Florida</td>
      <td>86.3</td>
      <td>87.6</td>
      <td>83.5</td>
      <td>75.9</td>
      <td>73.5</td>
      <td>71.4</td>
      <td>67.1</td>
      <td>66.6</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Georgia</td>
      <td>88.2</td>
      <td>90.2</td>
      <td>88.6</td>
      <td>89.4</td>
      <td>86.6</td>
      <td>83.8</td>
      <td>79.4</td>
      <td>77.8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hawaii</td>
      <td>47.8</td>
      <td>50.1</td>
      <td>50.1</td>
      <td>50.4</td>
      <td>49.4</td>
      <td>47.7</td>
      <td>44.3</td>
      <td>41.9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Idaho</td>
      <td>85.2</td>
      <td>88.6</td>
      <td>91.4</td>
      <td>92.0</td>
      <td>89.2</td>
      <td>87.4</td>
      <td>81.9</td>
      <td>77.6</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Illinois</td>
      <td>61.0</td>
      <td>63.1</td>
      <td>64.3</td>
      <td>66.1</td>
      <td>63.7</td>
      <td>62.3</td>
      <td>59.1</td>
      <td>56.8</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Indiana</td>
      <td>105.6</td>
      <td>107.1</td>
      <td>106.7</td>
      <td>110.5</td>
      <td>106.3</td>
      <td>96.7</td>
      <td>89.1</td>
      <td>83.9</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Iowa</td>
      <td>60.4</td>
      <td>62.3</td>
      <td>64.0</td>
      <td>74.1</td>
      <td>72.2</td>
      <td>72.3</td>
      <td>68.6</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Kansas</td>
      <td>82.8</td>
      <td>86.1</td>
      <td>87.8</td>
      <td>90.3</td>
      <td>88.7</td>
      <td>86.5</td>
      <td>80.5</td>
      <td>76.9</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Kentucky</td>
      <td>135.2</td>
      <td>136.5</td>
      <td>137.0</td>
      <td>127.9</td>
      <td>111.7</td>
      <td>110.0</td>
      <td>102.6</td>
      <td>97.2</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Louisiana</td>
      <td>113.0</td>
      <td>112.6</td>
      <td>111.7</td>
      <td>113.0</td>
      <td>112.4</td>
      <td>108.9</td>
      <td>100.4</td>
      <td>98.1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Maine</td>
      <td>87.9</td>
      <td>92.8</td>
      <td>93.1</td>
      <td>89.7</td>
      <td>85.9</td>
      <td>82.4</td>
      <td>76.5</td>
      <td>66.9</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Maryland</td>
      <td>66.9</td>
      <td>71.2</td>
      <td>72.9</td>
      <td>72.2</td>
      <td>69.0</td>
      <td>67.6</td>
      <td>63.0</td>
      <td>58.7</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Massachusetts</td>
      <td>68.9</td>
      <td>67.9</td>
      <td>65.9</td>
      <td>65.7</td>
      <td>63.0</td>
      <td>59.6</td>
      <td>54.0</td>
      <td>47.1</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Michigan</td>
      <td>91.6</td>
      <td>96.0</td>
      <td>98.8</td>
      <td>100.7</td>
      <td>98.9</td>
      <td>98.0</td>
      <td>90.5</td>
      <td>84.9</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Minnesota</td>
      <td>58.6</td>
      <td>59.0</td>
      <td>60.0</td>
      <td>60.9</td>
      <td>58.3</td>
      <td>56.6</td>
      <td>52.1</td>
      <td>46.9</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Mississippi</td>
      <td>117.3</td>
      <td>118.1</td>
      <td>117.2</td>
      <td>121.8</td>
      <td>119.6</td>
      <td>116.3</td>
      <td>110.9</td>
      <td>105.6</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Missouri</td>
      <td>88.5</td>
      <td>91.0</td>
      <td>91.6</td>
      <td>95.4</td>
      <td>93.3</td>
      <td>90.7</td>
      <td>84.5</td>
      <td>80.4</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Montana</td>
      <td>85.8</td>
      <td>87.2</td>
      <td>85.3</td>
      <td>87.7</td>
      <td>84.2</td>
      <td>80.1</td>
      <td>73.3</td>
      <td>69.8</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Nebraska</td>
      <td>66.4</td>
      <td>69.0</td>
      <td>68.8</td>
      <td>73.2</td>
      <td>71.3</td>
      <td>70.1</td>
      <td>65.5</td>
      <td>62.8</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Nevada</td>
      <td>94.0</td>
      <td>97.3</td>
      <td>100.3</td>
      <td>98.9</td>
      <td>91.1</td>
      <td>90.1</td>
      <td>85.4</td>
      <td>80.7</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Hampshire</td>
      <td>81.5</td>
      <td>81.6</td>
      <td>80.9</td>
      <td>83.7</td>
      <td>82.0</td>
      <td>79.6</td>
      <td>74.8</td>
      <td>64.3</td>
    </tr>
    <tr>
      <th>30</th>
      <td>New Jersey</td>
      <td>59.9</td>
      <td>61.0</td>
      <td>61.5</td>
      <td>60.1</td>
      <td>58.3</td>
      <td>57.2</td>
      <td>56.0</td>
      <td>52.6</td>
    </tr>
    <tr>
      <th>31</th>
      <td>New Mexico</td>
      <td>75.3</td>
      <td>81.9</td>
      <td>81.6</td>
      <td>76.8</td>
      <td>71.4</td>
      <td>71.5</td>
      <td>69.8</td>
      <td>65.1</td>
    </tr>
    <tr>
      <th>32</th>
      <td>New York</td>
      <td>49.5</td>
      <td>51.0</td>
      <td>51.1</td>
      <td>51.8</td>
      <td>46.7</td>
      <td>43.9</td>
      <td>45.1</td>
      <td>42.7</td>
    </tr>
    <tr>
      <th>33</th>
      <td>North Carolina</td>
      <td>89.3</td>
      <td>93.1</td>
      <td>93.5</td>
      <td>98.6</td>
      <td>96.7</td>
      <td>93.7</td>
      <td>88.4</td>
      <td>82.5</td>
    </tr>
    <tr>
      <th>34</th>
      <td>North Dakota</td>
      <td>60.3</td>
      <td>63.0</td>
      <td>61.3</td>
      <td>62.1</td>
      <td>60.1</td>
      <td>58.1</td>
      <td>53.0</td>
      <td>47.8</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Ohio</td>
      <td>100.4</td>
      <td>102.4</td>
      <td>101.5</td>
      <td>97.5</td>
      <td>93.1</td>
      <td>89.5</td>
      <td>82.7</td>
      <td>75.3</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Oklahoma</td>
      <td>115.0</td>
      <td>119.6</td>
      <td>122.3</td>
      <td>127.4</td>
      <td>123.3</td>
      <td>110.9</td>
      <td>104.4</td>
      <td>97.9</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Oregon</td>
      <td>99.3</td>
      <td>101.2</td>
      <td>100.7</td>
      <td>98.7</td>
      <td>94.2</td>
      <td>91.9</td>
      <td>84.2</td>
      <td>76.3</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Pennsylvania</td>
      <td>78.1</td>
      <td>81.0</td>
      <td>81.7</td>
      <td>83.3</td>
      <td>81.6</td>
      <td>79.9</td>
      <td>75.5</td>
      <td>69.5</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Rhode Island</td>
      <td>82.5</td>
      <td>82.7</td>
      <td>82.5</td>
      <td>83.2</td>
      <td>76.9</td>
      <td>72.8</td>
      <td>65.7</td>
      <td>60.3</td>
    </tr>
    <tr>
      <th>40</th>
      <td>South Carolina</td>
      <td>95.8</td>
      <td>98.6</td>
      <td>96.8</td>
      <td>104.0</td>
      <td>103.0</td>
      <td>101.3</td>
      <td>95.1</td>
      <td>89.4</td>
    </tr>
    <tr>
      <th>41</th>
      <td>South Dakota</td>
      <td>52.3</td>
      <td>54.0</td>
      <td>55.1</td>
      <td>60.1</td>
      <td>60.6</td>
      <td>61.7</td>
      <td>59.1</td>
      <td>54.8</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Tennessee</td>
      <td>138.4</td>
      <td>140.0</td>
      <td>138.5</td>
      <td>136.1</td>
      <td>127.1</td>
      <td>121.3</td>
      <td>114.9</td>
      <td>107.5</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Texas</td>
      <td>71.8</td>
      <td>73.0</td>
      <td>72.0</td>
      <td>73.4</td>
      <td>70.0</td>
      <td>67.0</td>
      <td>59.8</td>
      <td>57.6</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Utah</td>
      <td>86.9</td>
      <td>86.6</td>
      <td>85.2</td>
      <td>84.5</td>
      <td>82.1</td>
      <td>78.8</td>
      <td>74.4</td>
      <td>70.4</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Vermont</td>
      <td>54.5</td>
      <td>54.1</td>
      <td>53.6</td>
      <td>54.6</td>
      <td>52.2</td>
      <td>50.4</td>
      <td>60.1</td>
      <td>58.6</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Virginia</td>
      <td>74.1</td>
      <td>75.5</td>
      <td>76.6</td>
      <td>79.6</td>
      <td>76.6</td>
      <td>73.5</td>
      <td>68.1</td>
      <td>63.4</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Washington</td>
      <td>86.1</td>
      <td>85.0</td>
      <td>81.6</td>
      <td>78.7</td>
      <td>75.2</td>
      <td>74.2</td>
      <td>69.8</td>
      <td>64.9</td>
    </tr>
    <tr>
      <th>48</th>
      <td>West Virginia</td>
      <td>146.9</td>
      <td>143.1</td>
      <td>139.6</td>
      <td>136.9</td>
      <td>129.0</td>
      <td>126.4</td>
      <td>111.3</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wisconsin</td>
      <td>71.9</td>
      <td>74.4</td>
      <td>75.2</td>
      <td>76.8</td>
      <td>73.8</td>
      <td>71.9</td>
      <td>67.5</td>
      <td>62.2</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Wyoming</td>
      <td>81.0</td>
      <td>80.4</td>
      <td>78.2</td>
      <td>80.5</td>
      <td>81.5</td>
      <td>80.9</td>
      <td>75.4</td>
      <td>71.1</td>
    </tr>
  </tbody>
</table>
</div>



# Ave. of Opioid Prescribing Rate per 100 people in U.S. 2009-2016


```python
totalus=newdf.mean(axis=0).reset_index()
totalus

totalaveus=pd.DataFrame({'Year':totalus['index'],
                         'U.S. Ave. per Year':totalus[0]})
totalaveus
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
      <th>U.S. Ave. per Year</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>83.660784</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>85.558824</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>85.409804</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>86.184314</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>83.096078</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5</th>
      <td>80.688235</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>6</th>
      <td>75.637255</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>7</th>
      <td>70.817647</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots()
fig.set_size_inches(11.7,8.27)
sns.pointplot(x="Year",
            y="U.S. Ave. per Year",
            data=totalaveus,
            color="red")
plt.grid(linestyle="solid")
plt.xlabel("Years", size = 15)
plt.ylabel("Average Rate", size = 15)
plt.title ("Average Number of Opioid Prescriptions in U.S. (2009-2016)", size=20)
```




    Text(0.5,1,'Average Number of Opioid Prescriptions in U.S. (2009-2016)')




![png](output_6_1.png)


# Top 10 States Highest Opioid Prescribing Rate per 100 people in U.S. 2009-2016


```python
statemean=pd.DataFrame({'State': newdf['State'],
                       'Ave. Prescribing Rate': newdf.mean(axis=1)}
                      )
top=statemean.sort_values(by='Ave. Prescribing Rate', ascending=False)
z=top.head(10)
q=top.tail(10)
```


```python
pal = sns.cubehelix_palette(10, rot=-.25, light=.7)
g = sns.FacetGrid(z, row="State", hue="State", aspect=15, size=.5, palette='viridis')

# Draw the densities in a few steps
g.map(sns.kdeplot, "Ave. Prescribing Rate", clip_on=False, shade=True, alpha=1, lw=1.5, bw=.2)
g.map(sns.kdeplot, "Ave. Prescribing Rate", clip_on=False, color="w", lw=2, bw=.2)
g.map(plt.axhline, y=0, lw=2, clip_on=False)

# # Define and use a simple function to label the plot in axes coordinates
def label(x, color, label):
    ax = plt.gca()
    ax.text(0, .2, label, fontweight="bold", color=color, 
            ha="left", va="center", transform=ax.transAxes)

g.map(label, "Ave. Prescribing Rate")

# # Set the subplots to overlap
g.fig.subplots_adjust(hspace=-.25)

# # Remove axes details that don't play will with overlap
g.set_titles('')
g.set(yticks=[])
g.despine(bottom=True, left=True)
```




    <seaborn.axisgrid.FacetGrid at 0x1a1a806ef0>




![png](output_9_1.png)



```python
pal = sns.cubehelix_palette(10, rot=-.25, light=.7)
g = sns.FacetGrid(q, row="State", hue="State", aspect=15, size=.5, palette='viridis')

# Draw the densities in a few steps
g.map(sns.kdeplot, "Ave. Prescribing Rate", clip_on=False, shade=True, alpha=1, lw=1.5, bw=.2)
g.map(sns.kdeplot, "Ave. Prescribing Rate", clip_on=False, color="w", lw=2, bw=.2)
g.map(plt.axhline, y=0, lw=2, clip_on=False)

# # Define and use a simple function to label the plot in axes coordinates
def label(x, color, label):
    ax = plt.gca()
    ax.text(0, .2, label, fontweight="bold", color=color, 
            ha="left", va="center", transform=ax.transAxes)

g.map(label, "Ave. Prescribing Rate")

# # Set the subplots to overlap
g.fig.subplots_adjust(hspace=-.25)

# # Remove axes details that don't play will with overlap
g.set_titles("")
g.set(yticks=[])
g.despine(bottom=True, left=True)
```




    <seaborn.axisgrid.FacetGrid at 0x106c042e8>




![png](output_10_1.png)


# Top 10 States with Highest Opioid Prescribing Rate per 100 people in U.S. from 2009-2016


```python
#sum up each row, and create a new column that contains the total
sumpre=newdf.sum(axis=1) 
totalprescription=pd.DataFrame({'State':newdf['State'],
                                'Total Prescriptions':sumpre})
```


```python
w=totalprescription.sort_values(by='Total Prescriptions',ascending=False)
totals=w.head(10)
fig, ax = plt.subplots()
fig.set_size_inches(11.7, 8.27)
sns.barplot(x='State', y='Total Prescriptions', data=totals, palette="viridis")
plt.title ("Top 10 States with Highest Opioid Prescription Count (2009-2016)", size=20)
```




    Text(0.5,1,'Top 10 States with Highest Opioid Prescription Count (2009-2016)')




![png](output_13_1.png)

