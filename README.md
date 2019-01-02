
# NYC 311 API

You've gotten a chance to explore API basics through the Yelp API. In preparation for your final project, we will investigate another API from scratch. This should provide you with another familiar dataq option as well as practice for applying the same process to new unfamiliar APIs.

To start, go over to the API documentation at: 

https://dev.socrata.com/foundry/data.cityofnewyork.us/fhrw-4uyv


<img src="311_api_docs.png">

## Make an initial API call to retrieve 311 complaints from a neighborhood or zip code of your choice.


```python
# Your code here
```


```python
# Formulation 1

#token = 'Z3dVA3KBniPgZlDEtKycvsOKz' #This app token is posted to make teachers demonstration easier. 
#Please use sparingly. Overuse will lead to blacklisting and the token will be rendered useless.


#!/usr/bin/env python

# make sure to install these packages before running:
# pip install pandas
# pip install sodapy

import pandas as pd
from sodapy import Socrata

# Unauthenticated client only works with public data sets. Note 'None'
# in place of application token, and no username or password:
client = Socrata("data.cityofnewyork.us", token)

# Example authenticated client (needed for non-public datasets):
# client = Socrata(data.cityofnewyork.us,
#                  MyAppToken,
#                  userame="user@example.com",
#                  password="AFakePassword")

# First 2000 results, returned as JSON from API / converted to Python list of
# dictionaries by sodapy.
results = client.get("fhrw-4uyv", incident_zip = '11204', limit=2000)
```


```python
# Formulation 2
import requests
import pandas as pd

zip_code = '11204'

# can't figure out date ranges at the moment...
start_date = '2018-01-01T12:00:00'
end_date = '2018-02-01T12:00:00'

# create pull request based on parameters
url = "https://data.cityofnewyork.us/resource/fhrw-4uyv.json?incident_zip={}".format(zip_code)

# do the pull
response = requests.get(url)
if response.status_code == 200:
    data = response.json()
else:
    print('Hit an error.')
```

## Briefly Explore the Structure of the Response You Received.


```python
#Formulation 1
type(results)
```




    list




```python
len(results)
```




    2000




```python
results[0]
```




    {'address_type': 'ADDRESS',
     'agency': 'NYPD',
     'agency_name': 'New York City Police Department',
     'bbl': '3061710058',
     'borough': 'BROOKLYN',
     'city': 'BROOKLYN',
     'closed_date': '2018-10-29T03:08:00.000',
     'community_board': '11 BROOKLYN',
     'complaint_type': 'Blocked Driveway',
     'created_date': '2018-10-29T02:57:40.000',
     'cross_street_1': '17 AVENUE',
     'cross_street_2': '18 AVENUE',
     'descriptor': 'No Access',
     'due_date': '2018-10-29T10:57:40.000',
     'facility_type': 'Precinct',
     'incident_address': '1763 71 STREET',
     'incident_zip': '11204',
     'latitude': '40.61627769992391',
     'location': {'type': 'Point',
      'coordinates': [-73.994600708906, 40.616277699924]},
     'location_type': 'Street/Sidewalk',
     'longitude': '-73.99460070890645',
     'open_data_channel_type': 'ONLINE',
     'park_borough': 'BROOKLYN',
     'park_facility_name': 'Unspecified',
     'resolution_action_updated_date': '2018-10-29T03:08:00.000',
     'resolution_description': 'The Police Department responded to the complaint and took action to fix the condition.',
     'status': 'Closed',
     'street_name': '71 STREET',
     'unique_key': '40695291',
     'x_coordinate_state_plane': '985749',
     'y_coordinate_state_plane': '163803'}




```python
# Formulation 2
print(type(data))
```

    <class 'list'>



```python
len(data)
```




    1000




```python
data[0]
```




    {'address_type': 'ADDRESS',
     'agency': 'NYPD',
     'agency_name': 'New York City Police Department',
     'bbl': '3061710058',
     'borough': 'BROOKLYN',
     'city': 'BROOKLYN',
     'closed_date': '2018-10-29T03:08:00.000',
     'community_board': '11 BROOKLYN',
     'complaint_type': 'Blocked Driveway',
     'created_date': '2018-10-29T02:57:40.000',
     'cross_street_1': '17 AVENUE',
     'cross_street_2': '18 AVENUE',
     'descriptor': 'No Access',
     'due_date': '2018-10-29T10:57:40.000',
     'facility_type': 'Precinct',
     'incident_address': '1763 71 STREET',
     'incident_zip': '11204',
     'latitude': '40.61627769992391',
     'location': {'type': 'Point',
      'coordinates': [-73.994600708906, 40.616277699924]},
     'location_type': 'Street/Sidewalk',
     'longitude': '-73.99460070890645',
     'open_data_channel_type': 'ONLINE',
     'park_borough': 'BROOKLYN',
     'park_facility_name': 'Unspecified',
     'resolution_action_updated_date': '2018-10-29T03:08:00.000',
     'resolution_description': 'The Police Department responded to the complaint and took action to fix the condition.',
     'status': 'Closed',
     'street_name': '71 STREET',
     'unique_key': '40695291',
     'x_coordinate_state_plane': '985749',
     'y_coordinate_state_plane': '163803'}



## Create a Pandas DataFrame of the Data From the Response


```python
# Formulation 1 
df = pd.DataFrame(results)

print(len(df))
print(df.columns)
df.head()
```

    2000
    Index(['address_type', 'agency', 'agency_name', 'bbl', 'borough', 'city',
           'closed_date', 'community_board', 'complaint_type', 'created_date',
           'cross_street_1', 'cross_street_2', 'descriptor', 'due_date',
           'facility_type', 'incident_address', 'incident_zip',
           'intersection_street_1', 'intersection_street_2', 'latitude',
           'location', 'location_type', 'longitude', 'open_data_channel_type',
           'park_borough', 'park_facility_name', 'resolution_action_updated_date',
           'resolution_description', 'status', 'street_name',
           'taxi_company_borough', 'taxi_pick_up_location', 'unique_key',
           'x_coordinate_state_plane', 'y_coordinate_state_plane'],
          dtype='object')





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
      <th>address_type</th>
      <th>agency</th>
      <th>agency_name</th>
      <th>bbl</th>
      <th>borough</th>
      <th>city</th>
      <th>closed_date</th>
      <th>community_board</th>
      <th>complaint_type</th>
      <th>created_date</th>
      <th>...</th>
      <th>park_facility_name</th>
      <th>resolution_action_updated_date</th>
      <th>resolution_description</th>
      <th>status</th>
      <th>street_name</th>
      <th>taxi_company_borough</th>
      <th>taxi_pick_up_location</th>
      <th>unique_key</th>
      <th>x_coordinate_state_plane</th>
      <th>y_coordinate_state_plane</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3061710058</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-29T03:08:00.000</td>
      <td>11 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-29T02:57:40.000</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>2018-10-29T03:08:00.000</td>
      <td>The Police Department responded to the complai...</td>
      <td>Closed</td>
      <td>71 STREET</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40695291</td>
      <td>985749</td>
      <td>163803</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3062080044</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>WATER LEAK</td>
      <td>2018-10-29T21:53:03.000</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40695280</td>
      <td>987971</td>
      <td>161112</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3062080044</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>UNSANITARY CONDITION</td>
      <td>2018-10-29T21:53:02.000</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40695270</td>
      <td>987971</td>
      <td>161112</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3065950001</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>UNSANITARY CONDITION</td>
      <td>2018-10-29T12:27:25.000</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40695114</td>
      <td>988219</td>
      <td>161415</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3055120067</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-30T02:19:06.000</td>
      <td>12 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-29T16:30:56.000</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>2018-10-29T16:57:52.000</td>
      <td>The Police Department responded to the complai...</td>
      <td>Closed</td>
      <td>60 STREET</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40695369</td>
      <td>987975</td>
      <td>165752</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 35 columns</p>
</div>




```python
# Formulation 2 
df = pd.DataFrame(data)

print(len(df))
print(df.columns)
df.head()
```

    1000
    Index(['address_type', 'agency', 'agency_name', 'bbl', 'borough', 'city',
           'closed_date', 'community_board', 'complaint_type', 'created_date',
           'cross_street_1', 'cross_street_2', 'descriptor', 'due_date',
           'facility_type', 'incident_address', 'incident_zip',
           'intersection_street_1', 'intersection_street_2', 'latitude',
           'location', 'location_type', 'longitude', 'open_data_channel_type',
           'park_borough', 'park_facility_name', 'resolution_action_updated_date',
           'resolution_description', 'status', 'street_name',
           'taxi_pick_up_location', 'unique_key', 'x_coordinate_state_plane',
           'y_coordinate_state_plane'],
          dtype='object')





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
      <th>address_type</th>
      <th>agency</th>
      <th>agency_name</th>
      <th>bbl</th>
      <th>borough</th>
      <th>city</th>
      <th>closed_date</th>
      <th>community_board</th>
      <th>complaint_type</th>
      <th>created_date</th>
      <th>...</th>
      <th>park_borough</th>
      <th>park_facility_name</th>
      <th>resolution_action_updated_date</th>
      <th>resolution_description</th>
      <th>status</th>
      <th>street_name</th>
      <th>taxi_pick_up_location</th>
      <th>unique_key</th>
      <th>x_coordinate_state_plane</th>
      <th>y_coordinate_state_plane</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3061710058</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-29T03:08:00.000</td>
      <td>11 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-29T02:57:40.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T03:08:00.000</td>
      <td>The Police Department responded to the complai...</td>
      <td>Closed</td>
      <td>71 STREET</td>
      <td>NaN</td>
      <td>40695291</td>
      <td>985749</td>
      <td>163803</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3062080044</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>WATER LEAK</td>
      <td>2018-10-29T21:53:03.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>40695280</td>
      <td>987971</td>
      <td>161112</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3062080044</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>UNSANITARY CONDITION</td>
      <td>2018-10-29T21:53:02.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>40695270</td>
      <td>987971</td>
      <td>161112</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ADDRESS</td>
      <td>HPD</td>
      <td>Department of Housing Preservation and Develop...</td>
      <td>3065950001</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>UNSANITARY CONDITION</td>
      <td>2018-10-29T12:27:25.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:00:00.000</td>
      <td>The following complaint conditions are still o...</td>
      <td>Open</td>
      <td>BAY PARKWAY</td>
      <td>NaN</td>
      <td>40695114</td>
      <td>988219</td>
      <td>161415</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3055120067</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-30T02:19:06.000</td>
      <td>12 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-29T16:30:56.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T16:57:52.000</td>
      <td>The Police Department responded to the complai...</td>
      <td>Closed</td>
      <td>60 STREET</td>
      <td>NaN</td>
      <td>40695369</td>
      <td>987975</td>
      <td>165752</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>



## Create a Histogram of the Complaint Types From Your Dataset


```python
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# Your code here 
df.complaint_type.value_counts().plot(kind='barh', figsize=(8,12))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x10a261b70>




![png](index_files/index_17_1.png)

