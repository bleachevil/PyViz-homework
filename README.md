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






