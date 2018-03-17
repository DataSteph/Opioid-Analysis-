

```python
import os
import pandas as pd
import matplotlib.pyplot as plt
import numpy as numpy
```


```python
mortality_csv = "detailed_mortality.csv"
basecsv = pd.read_csv(mortality_csv)
df = df[["Year","State", "Cause of death", "Age Group", "Race", "Population"]]
mortality_df = df.dropna(how="any")
mortality_df["Year"]= mortality_df["Year"].astype(int)
reduced_mortality = mortality_df.loc[df2["Year"]>2008]
reduced_mortality.head()
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
      <th>Year</th>
      <th>State</th>
      <th>Cause of death</th>
      <th>Age Group</th>
      <th>Race</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14085</th>
      <td>2009</td>
      <td>Alabama</td>
      <td>Mental and behavioural disorders due to use of...</td>
      <td>&lt; 1 year</td>
      <td>American Indian or Alaska Native</td>
      <td>457</td>
    </tr>
    <tr>
      <th>14086</th>
      <td>2009</td>
      <td>Alabama</td>
      <td>Mental and behavioural disorders due to use of...</td>
      <td>&lt; 1 year</td>
      <td>Asian or Pacific Islander</td>
      <td>902</td>
    </tr>
    <tr>
      <th>14087</th>
      <td>2009</td>
      <td>Alabama</td>
      <td>Mental and behavioural disorders due to use of...</td>
      <td>&lt; 1 year</td>
      <td>Black or African American</td>
      <td>19536</td>
    </tr>
    <tr>
      <th>14088</th>
      <td>2009</td>
      <td>Alabama</td>
      <td>Mental and behavioural disorders due to use of...</td>
      <td>&lt; 1 year</td>
      <td>White</td>
      <td>40898</td>
    </tr>
    <tr>
      <th>14090</th>
      <td>2009</td>
      <td>Alabama</td>
      <td>Mental and behavioural disorders due to use of...</td>
      <td>1-4 years</td>
      <td>American Indian or Alaska Native</td>
      <td>1948</td>
    </tr>
  </tbody>
</table>
</div>



# Deaths by Year Data


```python
#year_summary = reduced_mortality_df.set_index(["Year"])
yeardata = reduced_mortality[["Year", "State","Population"]]
deathperyear = yeardata.groupby("Year")["Year"].count()
totalpopulation = yeardata.groupby("Year")["Population"].sum()
year_summary_df = pd.DataFrame({"Total Death by Year": deathperyear,
                               "Total Population": totalpopulation})
year_summary_df
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
      <th>Total Death by Year</th>
      <th>Total Population</th>
    </tr>
    <tr>
      <th>Year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2009</th>
      <td>33880</td>
      <td>4579021953640898194839307546416227726564694961...</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>33880</td>
      <td>5159291950439108218340457760616106727424994952...</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>33880</td>
      <td>2938081839839716203240307772015986826945207937...</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>33880</td>
      <td>3339001836738840176339457633715843226415429942...</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>33880</td>
      <td>3318861827937874159139357479915661726345418954...</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>33880</td>
      <td>3519691841138884145339317394515551026265355960...</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>33880</td>
      <td>5171144193903770614234051734441556502582522996...</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>33880</td>
      <td>5571066198983674816704427744421537572344519395...</td>
    </tr>
  </tbody>
</table>
</div>



# Death by State, Year


```python
state_yearsummary = reduced_mortality[["Year", "State", "Population"]]
statebyyear = state_yearsummary.groupby(["State", "Year"])["Population"].count()
statebyyear.unstack().head()
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
      <th>Alabama</th>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
    </tr>
    <tr>
      <th>Alaska</th>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
    </tr>
    <tr>
      <th>Arizona</th>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
    </tr>
    <tr>
      <th>Arkansas</th>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
    </tr>
    <tr>
      <th>California</th>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
      <td>616</td>
    </tr>
  </tbody>
</table>
</div>



# GroupBy Age


```python
age_summary = reduced_mortality[["Year", "State", "Age Group", "Cause of death"]]
agebyyear = age_summary.groupby(["Age Group", "Year"])["Cause of death"].count()
agebyyear.unstack().head()
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
      <th>Age Group</th>
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
      <th>1-4 years</th>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
    </tr>
    <tr>
      <th>10-14 years</th>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
    </tr>
    <tr>
      <th>15-19 years</th>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
    </tr>
    <tr>
      <th>20-24 years</th>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
    </tr>
    <tr>
      <th>25-34 years</th>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
      <td>2420</td>
    </tr>
  </tbody>
</table>
</div>


