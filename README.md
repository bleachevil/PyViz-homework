# PyViz-homework
# Toronto Dwellings Analysis

### imports
```
import panel as pn
pn.extension('plotly')
import plotly.express as px
import pandas as pd
import hvplot.pandas
import matplotlib.pyplot as plt
import os
from pathlib import Path
from dotenv import load_dotenv
```
### Read the Mapbox API key
```
load_dotenv()
map_box_api = os.getenv("mapbox")
```

### Load Data
```
file_path = Path("Data/toronto_neighbourhoods_census_data.csv")
to_data = pd.read_csv(file_path, index_col="year")
to_data.head()
```

### Result of to_data

![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data1.png?raw=true)

## Dwelling Types Per Year

### Calculate the sum number of dwelling types units per year
```
group_data = to_data.groupby(to_data.index).sum()
group_data.drop(columns = ['average_house_value','shelter_costs_owned','shelter_costs_rented'],inplace=True)
group_data
```
### Result of group_data

![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data2.png?raw=true)

### Save the dataframe as a csv file
```
group_data.to_csv("toronto_neighbourhoods_census_data_grouped.csv")
```
### Result is updated in the repository

### create_bar_chart function
```
def create_bar_chart(data, title, xlabel, ylabel, color):
        fig = data.hvplot.bar(title=title,xlabel=xlabel, ylabel=ylabel, color=color,rot=90,height=500,width=500).opts(yformatter="%.0f")
        return fig
```

### Bar chart for 2001 with red color
```
group_data1= group_data.T
group_data2 = group_data1.iloc[:,[0]]
group_data2
create_bar_chart(group_data2,"Dwelling Types in Toronto in 2001","2001","Dwelling Type Units","red")
```
### Resul of bar chart for 2001
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data3.png?raw=true)



### Bar chart for 2006 with blue color
```
group_data1= group_data.T
group_data3 = group_data1.iloc[:,[1]]
group_data3
create_bar_chart(group_data3,"Dwelling Types in Toronto in 2006","2006","Dwelling Type Units","blue")
```
### Resul of bar chart for 2006
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data4.png?raw=true)



### Bar chart for 2011 with orange color
```
group_data1= group_data.T
group_data4 = group_data1.iloc[:,[2]]
group_data4
create_bar_chart(group_data4,"Dwelling Types in Toronto in 2011","2011","Dwelling Type Units","orange")
```
### Resul of bar chart for 2011
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data5.png?raw=true)


### Bar chart for 2016 with pink color
```
group_data1= group_data.T
group_data5 = group_data1.iloc[:,[3]]
group_data5
create_bar_chart(group_data5,"Dwelling Types in Toronto in 2016","2016","Dwelling Type Units","pink")
```
### Resul of bar chart for 2016
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/data6.png?raw=true)


## Average Monthly Shelter Costs in Toronto Per Year

### Data - Calculate the average monthly shelter costs for owned and rented dwellings
```
avg_data = to_data.groupby(to_data.index).mean()
avg_data1 = avg_data[['shelter_costs_owned','shelter_costs_rented']]
avg_data1
```

### Result of avg_data_house
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata1.png?raw=true)

### create_line_chart function
```
def create_line_chart(data, title, xlabel, ylabel, color):
        fig = data.hvplot.line(title=title,xlabel=xlabel, ylabel=ylabel, color=color,rot=90,height=500).opts(yformatter="%.0f")
        return fig
```

### Graph - Line chart for owned dwellings
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata2.png?raw=true)

### Graph - Line chart for rental dwellings
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata3.png?raw=true)


## Average House Value by Neighbourhood¶

### Calculate the average house value per year
```
avg_data_house = avg_data[['average_house_value']]
avg_data_house
```

### Result of avg_data_house
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata4.png?raw=true)

### Plot the average house value per year as a line chart
```
create_line_chart(avg_data_house,"Average House Value in Toronto","year","Avg House Value","blue")
```
### Graph
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata5.png?raw=true)

## Average House Value by Neighbourhood

### Create a new DataFrame with the mean house values by neighbourhood per year
```
to_data.reset_index(inplace=True)
to_data.head()
```

### Result of to_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata6.png?raw=true)

### Select data for graph
```
n_data = to_data[['year','neighbourhood','average_house_value']]
n_data
```

### Result of n_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata7.png?raw=true)

### Use hvplot to create an interactive line chart of the average house value per neighbourhood
```
n_data.hvplot.line(x='year', y='average_house_value', width=600, groupby='neighbourhood').opts(yformatter="%.0f")
```

### Result
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata8.png?raw=true)


## Number of Dwelling Types per Year

### Fetch the data of all dwelling types per year
```
to_data.head()
to_data.drop(columns = ['average_house_value','shelter_costs_owned','shelter_costs_rented'],inplace=True)
to_data
```

### Result of to_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata9.png?raw=true)


### Use hvplot to create an interactive bar chart of the number of dwelling types per neighbourhood
```
to_data.hvplot.bar(stacked=False, height=500,rot=90,groupby='neighbourhood',xlabel = "year", ylabel = "Dwelling Type Units").opts(yformatter="%.0f")
```

### Graph
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/avgdata10.png?raw=true)


## The Top 10 Most Expensive Neighbourhoods

### Getting the data from the top 10 expensive neighbourhoods

```
top_data = to_data.reset_index().groupby('neighbourhood').mean()
top_data.sort_values(by=['average_house_value'], ascending=False).nlargest(10,'average_house_value')
total_top_data = top_data[['average_house_value']].sort_values(by=['average_house_value'], ascending=False).nlargest(10,'average_house_value')
total_top_data
```

### Result of total_top_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/topdata1.png?raw=true)

### Plotting the data from the top 10 expensive neighbourhoods
```
total_top_data.hvplot.bar(title="Top 10 Expensive Neighbourhoods in Toronto",xlabel="Neighbourhood", ylabel="Avg. House Value", color="blue",rot=90,height=500,width=500).opts(yformatter="%.0f")
```
### Graph
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/topdata2.png?raw=true)


## Neighbourhood Map
### Load Location Data¶
```
file_path = Path("Data/toronto_neighbourhoods_coordinates.csv")
df_neighbourhood_locations = pd.read_csv(file_path)
df_neighbourhood_locations.head()
```
### Result of df_neighbourhood_locations
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/mapdata1.png?raw=true)

### Calculate the mean values for each neighborhood
```
top_data = to_data.reset_index().groupby('neighbourhood').mean()
t_data = top_data.reset_index().drop(columns=['year'])
t_data
```
### Result of t_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/mapdata2.png?raw=true)

### Join the average values with the neighbourhood locations
```
join_data = df_neighbourhood_locations.merge(t_data)
join_data
```

### Result of join_data
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/mapdata3.png?raw=true)

### Mapbox Visualization
```
px.set_mapbox_access_token(map_box_api)
map_1 = px.scatter_mapbox(
    join_data,
    color="average_house_value",
    hover_name="neighbourhood",
    hover_data=["average_house_value","shelter_costs_owned","shelter_costs_rented","single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house"],
    lat="lat",
    lon="lon",
)
map_1.show()
```

### Graph
![](https://github.com/bleachevil/PyViz-homework/blob/main/Image/mapdata4.png?raw=true)

