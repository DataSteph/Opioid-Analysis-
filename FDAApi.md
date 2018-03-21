

```python
import json
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import requests
from localenv import fda_api_key

%matplotlib inline
```


```python
def format_url(base_url,search_fields,count_fields):
    return f"{base_url}api_key={fda_api_key}&search={search_fields}&count={count_fields}"
```


```python
base_url = 'https://api.fda.gov/drug/event.json?'

#Search for Number of deaths by gender in prescription drugs
search_fields ='receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+'\
               'patient.drug.openfda.product_type.exact:\"HUMAN+PRESCRIPTION+DRUG\"+AND+seriousnessdeath:1'
    
print(search_fields)
count_fields = 'patient.patientsex'
target_url = format_url(base_url,search_fields,count_fields)

print(target_url)
```

    receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+patient.drug.openfda.product_type.exact:"HUMAN+PRESCRIPTION+DRUG"+AND+seriousnessdeath:1
    https://api.fda.gov/drug/event.json?api_key=0lYxljicOBvelBsVCqCEdMAUzWDKB1eBVSESZjrx&search=receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+patient.drug.openfda.product_type.exact:"HUMAN+PRESCRIPTION+DRUG"+AND+seriousnessdeath:1&count=patient.patientsex



```python
response= requests.get(target_url).json()
print(json.dumps(response, sort_keys=True, indent=4, separators=(',', ': ')))
```

    {
        "meta": {
            "disclaimer": "Do not rely on openFDA to make decisions regarding medical care. While we make every effort to ensure that data is accurate, you should assume all results are unvalidated. We may limit or otherwise restrict your access to the API in line with our Terms of Service.",
            "last_updated": "2018-03-19",
            "license": "https://open.fda.gov/license/",
            "terms": "https://open.fda.gov/terms/"
        },
        "results": [
            {
                "count": 80616,
                "term": 1
            },
            {
                "count": 77408,
                "term": 2
            },
            {
                "count": 3287,
                "term": 0
            }
        ]
    }



```python
# Loop through all results
gender_results = [] 
gender_dict = { 1:'Male',
                2:'Female',
                0:'Unknown'
              }
for gender_data in response['results']:
    gender_results.append({'Gender':gender_dict[gender_data.get('term')],
                            'Count':gender_data.get('count')
                          })
    
print(gender_results)

```

    [{'Gender': 'Male', 'Count': 80616}, {'Gender': 'Female', 'Count': 77408}, {'Gender': 'Unknown', 'Count': 3287}]



```python
pd_deaths_gender = pd.DataFrame.from_dict(gender_results)
pd_deaths_gender

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
      <th>Count</th>
      <th>Gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>80616</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>1</th>
      <td>77408</td>
      <td>Female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3287</td>
      <td>Unknown</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate Type Percents
total_deaths = pd_deaths_gender['Count'].sum()
gender_percentages=round((pd_deaths_gender['Count'] / total_deaths) * 100, 2)
gender_percentages
print(len(gender_percentages))
```

    3



```python
# Build Pie Chart
fig1, ax = plt.subplots(figsize=(8,8))
my_circle = plt.Circle((0,0), 0.7, color="white")
ax.axis("equal")
explode = [0.05] * len(gender_percentages)


plt.title("Total Deaths from Prescription Drugs by Gender (2009-2016)",size=20)
#pieplot

gender_pieplt, texts, autotexts = ax.pie(gender_percentages, autopct="%1.1f%%",  colors=("teal","#97d73eff","lightgray"), 
                                       explode=explode, startangle=90, pctdistance=0.85)

#ax.set_title("Deaths by Gender due to prescribed drugs")
p=plt.gcf()
p.gca().add_artist(my_circle)
plt.xlabel("Gender", size=16)
plt.ylabel("Deaths", size= 16)
# Create a legend

# Create a legend
lgnd = plt.legend(fontsize="medium", mode="Expanded", 
                  numpoints=1,  labels=pd_deaths_gender['Gender'],
                  loc="best", title="Gender", 
                  labelspacing=0.5, fancybox=True)

autotexts[0].set_color('blue')
autotexts[1].set_color('blue')
autotexts[2].set_color('blue')

fig1.savefig("DeathsAgeGroupPrescription.png")
# Show Figure
plt.show()


```


![png](output_7_0.png)



```python
count_fields = 'patient.patientagegroup'
target_url = format_url(base_url,search_fields,count_fields)

print(target_url)
```

    https://api.fda.gov/drug/event.json?api_key=0lYxljicOBvelBsVCqCEdMAUzWDKB1eBVSESZjrx&search=receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+patient.drug.openfda.product_type.exact:"HUMAN+PRESCRIPTION+DRUG"+AND+seriousnessdeath:1&count=patient.patientagegroup



```python
response= requests.get(target_url).json()
print(json.dumps(response, sort_keys=True, indent=4, separators=(',', ': ')))
```

    {
        "meta": {
            "disclaimer": "Do not rely on openFDA to make decisions regarding medical care. While we make every effort to ensure that data is accurate, you should assume all results are unvalidated. We may limit or otherwise restrict your access to the API in line with our Terms of Service.",
            "last_updated": "2018-03-19",
            "license": "https://open.fda.gov/license/",
            "terms": "https://open.fda.gov/terms/"
        },
        "results": [
            {
                "count": 15439,
                "term": 6
            },
            {
                "count": 10047,
                "term": 5
            },
            {
                "count": 157,
                "term": 3
            },
            {
                "count": 128,
                "term": 2
            },
            {
                "count": 108,
                "term": 4
            },
            {
                "count": 102,
                "term": 1
            }
        ]
    }



```python
# Loop through all results
group_age_results = [] 
group_age_dict = { 1:'Neonate',
                   2:'Infant',
                   3:'Child',
                   4:'Adolescent',
                   5:'Elderly',
                   6:'Adult'
                 }
for age_data in response['results']:
    group_age_results.append({'Age Group': group_age_dict[age_data.get('term')],
                              'Total Deaths' : age_data.get('count')
                             })
    
print(group_age_results)

pd_age_category = pd.DataFrame.from_dict(group_age_results)
pd_age_category
```

    [{'Age Group': 'Adult', 'Total Deaths': 15439}, {'Age Group': 'Elderly', 'Total Deaths': 10047}, {'Age Group': 'Child', 'Total Deaths': 157}, {'Age Group': 'Infant', 'Total Deaths': 128}, {'Age Group': 'Adolescent', 'Total Deaths': 108}, {'Age Group': 'Neonate', 'Total Deaths': 102}]





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
      <th>Age Group</th>
      <th>Total Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adult</td>
      <td>15439</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Elderly</td>
      <td>10047</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Child</td>
      <td>157</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Infant</td>
      <td>128</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Adolescent</td>
      <td>108</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Neonate</td>
      <td>102</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate Age Category Percents
total_deaths_age = pd_age_category['Total Deaths'].sum()
age_group_percentages=round((pd_age_category['Total Deaths'] / total_deaths_age) * 100, 2)
```


```python
#x_axis = np.arange(pd_age_category['Count'].max() + 100, 250)
x = pd_age_category['Age Group']
y = np.array(pd_age_category['Total Deaths'])
sns.set(color_codes=True)
explode = [0.05] * len(age_group_percentages)
fig2, ax2 = plt.subplots(figsize=(8,8))
my_circle1 = plt.Circle((0,0), 0.7, color="white")


patches, texts = ax2.pie(age_group_percentages, explode=explode, startangle=90, pctdistance=0.85)

#ax.set_title("Deaths by Gender due to prescribed drugs")
p=plt.gcf()
p.gca().add_artist(my_circle1)
plt.title("Deaths related to Opioid Prescriptions by Age Group", size = 20)
plt.xlabel("Age Category Group", size=16)
plt.ylabel("Deaths", size=16)

# Create a legend
labels = ['{0} - {1:1.2f} %'.format(i,j) for i,j in zip(x, age_group_percentages)]

sort_legend = True
if sort_legend:
    patches, labels, dummy =  zip(*sorted(zip(patches, labels, y),
                                          key=lambda x: x[2],
                                          reverse=True))

plt.legend(patches, labels, loc='best', bbox_to_anchor=(-0.1, 1.),
           fontsize=12, title='Age Category Group', fancybox=True)

plt.savefig('piechart.png', bbox_inches='tight')


ax2.set_autoscale_on(True)

fig2 = ax2.get_figure()
fig2.savefig("DeathsAgeGroupPrescription.png")

```


![png](output_12_0.png)



```python
search_fields ='receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+'\
               'patient.drug.openfda.product_type.exact:\"HUMAN+PRESCRIPTION+DRUG\"'

count_fields = 'patient.reaction.reactionmeddrapt.exact'
target_url = format_url(base_url,search_fields,count_fields)
print(target_url)
```

    https://api.fda.gov/drug/event.json?api_key=0lYxljicOBvelBsVCqCEdMAUzWDKB1eBVSESZjrx&search=receivedate:[20090101+TO+20161231]+AND+occurcountry:US+AND+patient.drug.openfda.product_type.exact:"HUMAN+PRESCRIPTION+DRUG"&count=patient.reaction.reactionmeddrapt.exact



```python
response= requests.get(target_url).json()

```


```python
adverse_reaction = []

for reaction_data in response['results']:
    adverse_reaction.append({'Reaction': str.title(reaction_data.get('term')),
                              'Total' : reaction_data.get('count')
                             })
    



pd_adverse_reaction = pd.DataFrame.from_dict(adverse_reaction)
```


```python
#file_df["avg_cost"] = file_df["avg_cost"].map(dollar_round2)
# file_df["avg_cost"] = file_df["avg_cost"].map("${:.2f}".format)
#pd_adverse_reaction['Reaction'] = pd_adverse_reaction['Reaction'].map(str.lower(pd_adverse_reaction['Reaction'].values()))
pd_adverse_reaction.sort_values(by="Total", ascending=False, inplace=True)
pd_adverse_reaction.head(10)

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
      <th>Reaction</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Drug Ineffective</td>
      <td>179742</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Fatigue</td>
      <td>120280</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nausea</td>
      <td>114277</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Headache</td>
      <td>98958</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Death</td>
      <td>95218</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Pain</td>
      <td>94394</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Diarrhoea</td>
      <td>81416</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Dyspnoea</td>
      <td>75482</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Malaise</td>
      <td>73328</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Off Label Use</td>
      <td>73077</td>
    </tr>
  </tbody>
</table>
</div>




```python
summary_df = pd_adverse_reaction.head(6)
fig, ax = plt.subplots(figsize=(12,12))
fig.set_size_inches(11.7,8.27)
sns.pointplot(x="Reaction",
            y="Total",
            data=summary_df,
            color="orange")
plt.grid(linestyle="solid")
plt.xlabel("Adverse Reaction", size = 15)
plt.ylabel("Cases", size = 15)
plt.title ("Adverse Reactions Prescription Drugs (2009-2016)", size=20)
fig = ax.get_figure()
fig.savefig("AdverseReactionsPrescriptionDrugs.png")
```


![png](output_17_0.png)

