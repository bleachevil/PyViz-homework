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
