```python
import pandas as pd
import geopandas as gpd
import numpy as np
import plotly.express as px
import plotly.graph_objects as go
import plotly.io as pio
```


```python
pio.renderers.default = 'png'
pio.templates.default = "plotly_white"
pd.options.display.max_columns = None
```


```python
file_path = "/Users/adelh/Downloads/analyst-task/data-zaplnenost-kontejneru/measurements-march.csv"
data=pd.read_csv(file_path)
```


```python
file_path = '/Users/adelh/Downloads/analyst-task/data-zaplnenost-kontejneru/containers.geojson'
gdf = gpd.read_file(file_path)
```


```python
data.head(3)
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
      <th>id</th>
      <th>container_id</th>
      <th>code</th>
      <th>percent_calculated</th>
      <th>upturned</th>
      <th>temperature</th>
      <th>battery_status</th>
      <th>measured_at</th>
      <th>measured_at_utc</th>
      <th>prediction</th>
      <th>prediction_utc</th>
      <th>firealarm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11678631</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>25</td>
      <td>3.74</td>
      <td>2019-03-30T23:09:29.000Z</td>
      <td>2019-03-30T23:09:29.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11677936</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>26</td>
      <td>3.74</td>
      <td>2019-03-30T21:09:29.000Z</td>
      <td>2019-03-30T21:09:29.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11676616</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>26</td>
      <td>3.74</td>
      <td>2019-03-30T19:09:28.000Z</td>
      <td>2019-03-30T19:09:28.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
gdf.head(3)
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
      <th>id</th>
      <th>code</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>address</th>
      <th>district</th>
      <th>postal_code</th>
      <th>total_volume</th>
      <th>trash_type</th>
      <th>prediction</th>
      <th>bin_type</th>
      <th>installed_at</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>29866</td>
      <td>S0001C00011</td>
      <td>50.088839</td>
      <td>14.410216</td>
      <td>Cihelná 548</td>
      <td>None</td>
      <td>11000</td>
      <td>3000</td>
      <td>plastic</td>
      <td>NaT</td>
      <td>Schäfer/Europa-OV</td>
      <td>2018-12-18 00:00:00+00:00</td>
      <td>POINT (14.41022 50.08884)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29867</td>
      <td>S0001C00012</td>
      <td>50.088839</td>
      <td>14.410216</td>
      <td>Cihelná 548</td>
      <td>None</td>
      <td>11000</td>
      <td>3000</td>
      <td>paper</td>
      <td>NaT</td>
      <td>Schäfer/Europa-OV</td>
      <td>2018-12-18 00:00:00+00:00</td>
      <td>POINT (14.41022 50.08884)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>29882</td>
      <td>S0002C00021</td>
      <td>50.090378</td>
      <td>14.423075</td>
      <td>Haštalská 748</td>
      <td>None</td>
      <td>11000</td>
      <td>3000</td>
      <td>paper</td>
      <td>NaT</td>
      <td>Schäfer/Europa-OV</td>
      <td>2018-12-18 00:00:00+00:00</td>
      <td>POINT (14.42308 50.09038)</td>
    </tr>
  </tbody>
</table>
</div>




```python
data = pd.merge(data, gdf[['code','trash_type','installed_at','address']], on='code', how='left')
data['installed_at'] = data['installed_at'].dt.strftime('%Y-%m')
data.head()
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
      <th>id</th>
      <th>container_id</th>
      <th>code</th>
      <th>percent_calculated</th>
      <th>upturned</th>
      <th>temperature</th>
      <th>battery_status</th>
      <th>measured_at</th>
      <th>measured_at_utc</th>
      <th>prediction</th>
      <th>prediction_utc</th>
      <th>firealarm</th>
      <th>trash_type</th>
      <th>installed_at</th>
      <th>address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11678631</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>25</td>
      <td>3.74</td>
      <td>2019-03-30T23:09:29.000Z</td>
      <td>2019-03-30T23:09:29.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
      <td>metal</td>
      <td>2019-01</td>
      <td>Molákova 601/2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11677936</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>26</td>
      <td>3.74</td>
      <td>2019-03-30T21:09:29.000Z</td>
      <td>2019-03-30T21:09:29.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
      <td>metal</td>
      <td>2019-01</td>
      <td>Molákova 601/2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11676616</td>
      <td>30133</td>
      <td>S0133C01336</td>
      <td>53</td>
      <td>0</td>
      <td>26</td>
      <td>3.74</td>
      <td>2019-03-30T19:09:28.000Z</td>
      <td>2019-03-30T19:09:28.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>2019-05-13T12:39:17.000Z</td>
      <td>0</td>
      <td>metal</td>
      <td>2019-01</td>
      <td>Molákova 601/2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11675949</td>
      <td>30142</td>
      <td>S0135C01353</td>
      <td>32</td>
      <td>0</td>
      <td>15</td>
      <td>3.77</td>
      <td>2019-03-30T18:00:17.000Z</td>
      <td>2019-03-30T18:00:17.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_coloured</td>
      <td>2019-01</td>
      <td>Na Rokytce 22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11675836</td>
      <td>30040</td>
      <td>S0050C00502</td>
      <td>64</td>
      <td>0</td>
      <td>14</td>
      <td>3.60</td>
      <td>2019-03-30T17:56:09.000Z</td>
      <td>2019-03-30T17:56:09.000Z</td>
      <td>2019-04-14T21:43:38.000Z</td>
      <td>2019-04-14T21:43:38.000Z</td>
      <td>0</td>
      <td>plastic</td>
      <td>2018-12</td>
      <td>Jeseniova 1196</td>
    </tr>
  </tbody>
</table>
</div>




```python
data['combined_column'] = data.apply(lambda row: f"{row['container_id']}/{row['address']}//{row['installed_at']}", axis=1)
```


```python
glass = data.loc[lambda x: x["trash_type"].isin(['glass_white', 'glass_colored'])]
```


```python
print("Number of containers:", glass['container_id'].nunique())
```

    Number of containers: 62



```python
print("Date range:", glass['measured_at'].min(), glass['measured_at'].max())
```

    Date range: 2019-03-01T04:07:54.000Z 2019-03-30T17:30:56.000Z



```python
print("Battety status:", glass['battery_status'].min(), glass['battery_status'].max())
```

    Battety status: 3.6 3.82



```python
print("Total number of duplicate rows:", glass.duplicated().sum())
```

    Total number of duplicate rows: 0



```python
# null
null_counts = glass.isnull().sum()
print(null_counts)
```

    id                       0
    container_id             0
    code                     0
    percent_calculated       0
    upturned                 0
    temperature              0
    battery_status           0
    measured_at              0
    measured_at_utc          0
    prediction            1428
    prediction_utc        1428
    firealarm                0
    trash_type               0
    installed_at           174
    address                  0
    combined_column          0
    dtype: int64



```python
containers_without_nulls = glass[glass['prediction'].notnull()]['combined_column'].unique()
filtered_glass = glass[glass['combined_column'].isin(containers_without_nulls)]
filtered_glass.isnull().sum()
```




    id                      0
    container_id            0
    code                    0
    percent_calculated      0
    upturned                0
    temperature             0
    battery_status          0
    measured_at             0
    measured_at_utc         0
    prediction              0
    prediction_utc          0
    firealarm               0
    trash_type              0
    installed_at          174
    address                 0
    combined_column         0
    dtype: int64




```python
containers_with_nulls_p = glass[glass['prediction'].isnull()]['combined_column'].unique()
count_containers_without_nulls_p = glass[glass['prediction'].notnull()]['combined_column'].unique()

containers_with_nulls_ins = glass[glass['installed_at'].isnull()]['combined_column'].unique()
count_containers_without_nulls_ins = glass[glass['installed_at'].notnull()]['combined_column'].unique()
```


```python
containers_with_nulls_ins = len(containers_with_nulls_ins)
print("Number of unique containers with null values in installed_at:", containers_with_nulls_ins)
```

    Number of unique containers with null values in installed_at: 1



```python
containers_with_nulls_p = len(containers_with_nulls_p)
print("Number of unique containers with null values in prediction:", containers_with_nulls_p)

```

    Number of unique containers with null values in prediction: 10



```python
count_containers_without_nulls_pc = len(count_containers_without_nulls_p)
print("Number of unique containers without null values in prediction:", count_containers_without_nulls_pc)
```

    Number of unique containers without null values in prediction: 52



```python
containers_without_nulls_p = glass[glass['combined_column'].isin(count_containers_without_nulls_p)]

```


```python
# upturned
glass.groupby('combined_column')['upturned'].sum()
```




    combined_column
    29791/Topolová-Jabloňová 2963/12//2018-11     2
    29797/U Vršovického nádraží 0//2018-11        0
    29807/Tupolevova 477//2018-11                 0
    29823/Puškinovo náměstí 692//nan              0
    29828/Litvínovská 288//2018-11                0
                                                 ..
    30159/Nám. Dr. Václava Holého //2019-01       2
    30165/Františka Kadlece //2019-01             0
    30171/Nad Vavrouškou Velká Skála //2019-01    1
    30175/Za Poříčskou bránou //2019-01           0
    30403/Pohořelec  111/25//2019-01              4
    Name: upturned, Length: 62, dtype: int64




```python
glass['measured_at'] = pd.to_datetime(glass['measured_at'])
glass = glass.sort_values(by=['container_id', 'measured_at'])
glass['time_difference'] = glass.groupby('container_id')['measured_at'].diff()
```

    /var/folders/70/4lp5jlf92b54tq2xc2ym56zm0000gn/T/ipykernel_30236/3501347775.py:1: SettingWithCopyWarning:
    
    
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
    



```python
agg_df = glass.groupby('container_id')['time_difference'].agg(['mean', 'min', 'max']).reset_index()
agg_df['difference'] = agg_df['max'] - agg_df['mean']
```


```python
agg_df.head(2)
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
      <th>container_id</th>
      <th>mean</th>
      <th>min</th>
      <th>max</th>
      <th>difference</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>29791</td>
      <td>0 days 06:15:44.823008849</td>
      <td>0 days 03:08:08</td>
      <td>0 days 16:02:28</td>
      <td>0 days 09:46:43.176991151</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29797</td>
      <td>0 days 03:58:32.129213483</td>
      <td>0 days 01:07:49</td>
      <td>0 days 12:01:53</td>
      <td>0 days 08:03:20.870786517</td>
    </tr>
  </tbody>
</table>
</div>




```python
# measurement failures
filtered_containers = agg_df.loc[agg_df['difference'] >= pd.Timedelta(days=1), 'container_id']
filtered_containers_df = pd.merge(glass, filtered_containers, how='inner')
```


```python
filtered_containers_df.head()
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
      <th>id</th>
      <th>container_id</th>
      <th>code</th>
      <th>percent_calculated</th>
      <th>upturned</th>
      <th>temperature</th>
      <th>battery_status</th>
      <th>measured_at</th>
      <th>measured_at_utc</th>
      <th>prediction</th>
      <th>prediction_utc</th>
      <th>firealarm</th>
      <th>trash_type</th>
      <th>installed_at</th>
      <th>address</th>
      <th>combined_column</th>
      <th>time_difference</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11176681</td>
      <td>29922</td>
      <td>S0022C00222</td>
      <td>86</td>
      <td>0</td>
      <td>6</td>
      <td>3.6</td>
      <td>2019-03-01 21:50:39+00:00</td>
      <td>2019-03-01T21:50:39.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_white</td>
      <td>2018-12</td>
      <td>Pod kaštany 177</td>
      <td>29922/Pod kaštany 177//2018-12</td>
      <td>NaT</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11187219</td>
      <td>29922</td>
      <td>S0022C00222</td>
      <td>91</td>
      <td>0</td>
      <td>6</td>
      <td>3.6</td>
      <td>2019-03-02 13:53:09+00:00</td>
      <td>2019-03-02T13:53:09.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_white</td>
      <td>2018-12</td>
      <td>Pod kaštany 177</td>
      <td>29922/Pod kaštany 177//2018-12</td>
      <td>0 days 16:02:30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11196263</td>
      <td>29922</td>
      <td>S0022C00222</td>
      <td>93</td>
      <td>0</td>
      <td>5</td>
      <td>3.6</td>
      <td>2019-03-03 05:17:57+00:00</td>
      <td>2019-03-03T05:17:57.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_white</td>
      <td>2018-12</td>
      <td>Pod kaštany 177</td>
      <td>29922/Pod kaštany 177//2018-12</td>
      <td>0 days 15:24:48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11244045</td>
      <td>29922</td>
      <td>S0022C00222</td>
      <td>100</td>
      <td>0</td>
      <td>3</td>
      <td>3.6</td>
      <td>2019-03-06 05:29:10+00:00</td>
      <td>2019-03-06T05:29:10.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_white</td>
      <td>2018-12</td>
      <td>Pod kaštany 177</td>
      <td>29922/Pod kaštany 177//2018-12</td>
      <td>3 days 00:11:13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11246830</td>
      <td>29922</td>
      <td>S0022C00222</td>
      <td>87</td>
      <td>0</td>
      <td>4</td>
      <td>3.6</td>
      <td>2019-03-06 09:29:47+00:00</td>
      <td>2019-03-06T09:29:47.000Z</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>glass_white</td>
      <td>2018-12</td>
      <td>Pod kaštany 177</td>
      <td>29922/Pod kaštany 177//2018-12</td>
      <td>0 days 04:00:37</td>
    </tr>
  </tbody>
</table>
</div>




```python
# data plotting
unique_container_ids = glass['combined_column'].unique()
container_id_groups = [unique_container_ids[i:i+5] for i in range(0, len(unique_container_ids), 5)]


for i, group in enumerate(container_id_groups):
    subset_df = glass[glass['combined_column'].isin(group)]

    fig = px.line(subset_df, x="measured_at", y="percent_calculated", color='combined_column', title=f"Container group {i+1}")

    upturned_data = subset_df[subset_df['upturned'] == 1]
    for _, row in upturned_data.iterrows():
        fig.add_annotation(x=row['measured_at'], y=row['percent_calculated'], text='upturned=1', showarrow=True, arrowhead=2, arrowcolor='red')


    fig.update_layout(title=f"Container group {i+1}", xaxis_title="Time", yaxis_title="Percent Calculated")
    fig.show()
```


    
![png](data_info_files/data_info_26_0.png)
    



    
![png](data_info_files/data_info_26_1.png)
    



    
![png](data_info_files/data_info_26_2.png)
    



    
![png](data_info_files/data_info_26_3.png)
    



    
![png](data_info_files/data_info_26_4.png)
    



    
![png](data_info_files/data_info_26_5.png)
    



    
![png](data_info_files/data_info_26_6.png)
    



    
![png](data_info_files/data_info_26_7.png)
    



    
![png](data_info_files/data_info_26_8.png)
    



    
![png](data_info_files/data_info_26_9.png)
    



    
![png](data_info_files/data_info_26_10.png)
    



    
![png](data_info_files/data_info_26_11.png)
    



    
![png](data_info_files/data_info_26_12.png)
    



```python
std_dev_by_container = glass.groupby('container_id')['percent_calculated'].std()
containers_with_min_deviation = std_dev_by_container[std_dev_by_container == std_dev_by_container.min()].index
filtered_containers = std_dev_by_container[std_dev_by_container <= 1.5].index
filtered_df = glass[glass['container_id'].isin(filtered_containers)]
fig = px.line(filtered_df, x="measured_at", y="percent_calculated", color='combined_column',title="Containers whose filling did not change during the entire measurement period")
fig.show()
```


    
![png](data_info_files/data_info_27_0.png)
    



```python

# data plotting prediction

for i, group in enumerate(container_id_groups):
    subset_df = containers_without_nulls_p[containers_without_nulls_p['combined_column'].isin(group)]

    fig = px.line(subset_df, x="measured_at", y="percent_calculated", color='combined_column', title=f"Container group {i+1}")

    upturned_data = subset_df[subset_df['upturned'] == 1]
    for _, row in upturned_data.iterrows():
        fig.add_annotation(x=row['measured_at'], y=row['percent_calculated'], text='upturned=1', showarrow=True, arrowhead=2, arrowcolor='red')

    # predict
    for cont_id, group_data in subset_df.groupby('combined_column'):
        fig.add_trace(go.Scatter(x=group_data['measured_at'], y=group_data['prediction'], mode='markers', name=f'Prediction_{cont_id}', marker=dict(size=10)))

    fig.update_layout(title=f"Container group {i+1}", xaxis_title="Time", yaxis_title="Percent Calculated")
    pio.write_html(fig, f"container_group_{i+1}.html")
    fig.show()




```


    
![png](data_info_files/data_info_28_0.png)
    



    
![png](data_info_files/data_info_28_1.png)
    



    
![png](data_info_files/data_info_28_2.png)
    



    
![png](data_info_files/data_info_28_3.png)
    



    
![png](data_info_files/data_info_28_4.png)
    



    
![png](data_info_files/data_info_28_5.png)
    



    
![png](data_info_files/data_info_28_6.png)
    



    
![png](data_info_files/data_info_28_7.png)
    



    
![png](data_info_files/data_info_28_8.png)
    



    
![png](data_info_files/data_info_28_9.png)
    



    
![png](data_info_files/data_info_28_10.png)
    

# Main problems in data :
- there are no duplicate records in the glass containers
- null values are in the prediction and installerd-at columns, where only one container has no date when it was installed and 10 containers have no prediction values
- the overall quality of the data is very bad, it shows significant measurement errors, as can be seen if we visualize the data
- upturned values are either completely missing or do not correspond to reality, some series show gaps in measurement


```python

```
