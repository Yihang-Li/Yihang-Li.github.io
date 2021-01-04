---
title: Summary for visualization of geospatial data
date: 2020-02-10 14:04:01
tags: [python, geospatial, visualization]
categories: Notes
mathjax: true
typora-copy-images-to: ..\assets
typora-root-url: ..
---

> **Here we summarize the geospatial data visualization.**
>
> The `Synthetic Power Grid Data Set`  will be used as an example. [[Download here](https://wimnet.ee.columbia.edu/portfolio/synthetic-power-grids-data-sets/)]

> Before all the things, let's import some basic tools:

``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')
```

<!-- more -->

## 1. Data Preparation

### 1.1 Import Data

> Here, the `Location`( (Lon, Lat) pairs), `Demand` and `Supply` information are loaded by using `pandas.read_csv("path")`. 

``` python
Bus_Location = pd.read_csv("Gen_WI_Bus_Locations.csv")
Demand_Values = pd.read_csv("Gen_WI_Demand_Values.csv")
Supply_Values = pd.read_csv("Gen_WI_Supply_Values.csv")
```

### 1.2 Merge Data

> These 3 tables have the same column named **_Bus Number_** , we can use `reduce()` to merge them.

``` python
# first compile the list of dataframes we want to merge
data_frames = [Bus_Location, Demand_Values, Supply_Values]
from functools import reduce
df_merged = reduce(lambda  left,right: pd.merge(left,right,on=['Bus Number'], how='outer'), data_frames)
#Then call head() method, we can see the first five rows of our merged data
df_merged.head()
```

<img src="/assets/image-20200210151126713.png" alt="head" style="zoom:67%;" />



> Moreover, we can call `describe()` method to summarize our data.

``` python
df_merged[['Lon', 'Lat', 'Demand (MW)', 'Supply (MW)']].describe()
```

<img src="/assets/image-20200210151217027.png" alt="describe" style="zoom:67%;" />

***

## 2. Using geopandas to visualize

> We can refer to `geopandas` docs [Click here](https://geopandas.org/).

### 2.1 Data Structures

> GeoPandas implements two main data structures, a `GeoSeries` and a `GeoDataFrame`. These are subclasses of pandas `Series` and `DataFrame`, respectively. Before visualization, we should convert our data into proper type.

``` python
import geopandas as gpd
gdf = gpd.GeoDataFrame(df_merged, geometry = gpd.points_from_xy(df_merged.Lon, df_merged.Lat))
gdf.head()
#And here call type(gdf):
#geopandas.geodataframe.GeoDataFrame
```

<img src="/assets/image-20200210153603866.png" alt="gdfhead" style="zoom:65%;" />

### 2.2 Coordinate Reference Systems

> CRS are important because the geometric shapes in a GeoSeries or GeoDataFrame object are simply a collection of coordinates in an arbitrary space. A CRS tells Python how those coordinates related to places on the Earth.
>
> CRS are referred to using codes called [proj4 strings](https://en.wikipedia.org/wiki/PROJ.4). You can find the codes for most commonly used projections from [www.spatialreference.org](http://spatialreference.org/). Common projections can also be referred to by EPSG codes, so this same projection can also called using the proj4 string `"+init=epsg:4326"`.

> *geopandas* can accept lots of representations of CRS, including the proj4 string itself (`"+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"`) or parameters broken out in a dictionary: `{'proj': 'latlong', 'ellps': 'WGS84', 'datum': 'WGS84', 'no_defs': True}`). In addition, some functions will take EPSG codes directly.
>
> For reference, a few very common projections and their proj4 strings:
>
> - WGS84 Latitude/Longitude: `"+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"` or `"+init=epsg:4326"`
> - UTM Zones (North): `"+proj=utm +zone=33 +ellps=WGS84 +datum=WGS84 +units=m +no_defs"`
> - UTM Zones (South): `"+proj=utm +zone=33 +ellps=WGS84 +datum=WGS84 +units=m +no_defs +south"`

> `contextily`: context geo tiles in Python
>
> >`contextily` is a small Python 3 package to retrieve and write to disk tile maps from the internet into geospatial raster files. Bounding boxes can be passed in both WGS84 (`EPSG:4326`) and Spheric Mercator (`EPSG:3857`). See [here](https://github.com/darribas/contextily) for usage.

***

**Let's just dive into code**

``` python
import contextily as ctx
gdf.crs = {'init': 'epsg:4326', 'no_defs': True}
gdf = gdf.to_crs(epsg=3857)
ax = gdf.plot(figsize=(10, 10), alpha=0.5, edgecolor='k')
ctx.add_basemap(ax)
```

<img src="/assets/image-20200210161956076.png" alt="first" style="zoom:67%;" />

### 2.3 Mapping attributes into it

> Here we map the `Demand` values into the map, and try to set some parameters

``` python
ax = gdf.plot(figsize=(10, 10), alpha=0.5, edgecolor='w', column = "Demand (MW)", legend = True, 
              legend_kwds={'label': "Demand (MW)",'orientation': "vertical"}) #horizontal
ctx.add_basemap(ax, url=ctx.providers.Stamen.TonerLite)
ax.set_axis_off()
plt.savefig('Demand(MW)')
```

<img src="/assets/image-20200210162505599.png" alt="Second" style="zoom:67%;" />

### 2.4 Summary

> Here we summarize 2.3 as follows:

``` python
def Attribute_mapping(gdf, column, legend_kwds):
    ax = gdf.plot(figsize = (10, 10), alpha = 0.5, edgecolor = 'w', column = column, 					legend = True, legend_kwds = legend_kwds)
    ctx.add_basemap(ax, url=ctx.providers.Stamen.TonerLite)
    ax.set_axis_off()
```

> Our input is:
>
> - `gdf` : Corresponding geopandas dataframe after processing in 2.2
> - `column`: Corresponding Attribute column that we want to map
> - `legend_kwds`: Corresponding legend parameters

**Let's see an example below:**

``` python
#First get our gdf
import geopandas as gpd
gdf = gpd.GeoDataFrame(df_merged, geometry = gpd.points_from_xy(df_merged.Lon, df_merged.Lat)) #Note: df_merged is a pandas dataframe!
#process as in 2.2
import contextily as ctx
gdf.crs = {'init': 'epsg:4326', 'no_defs': True}
gdf = gdf.to_crs(epsg=3857)
#set our column and legend_kwds
column = "Supply (MW)"
legend_kwds={'label': "Supply (MW)",'orientation': "vertical"}
#call the function above
Attribute_mapping(gdf, column, legend_kwds)
```

<img src="/assets/image-20200210164126937.png" alt="Third" style="zoom:67%;" />

***

## 3. Try Something More

### 3.1 Remove Zero

As we see the describe results in 1.2, more than 75% of Demand and Supply are all zero. So, in this part, we try to deal with them.

Just see the code

``` python
None_Zero_Demand = gdf[gdf['Demand (MW)'] != 0.0]
None_Zero_Demand.head()
None_Zero_Demand.describe()
```

<img src="/assets/image-20200210165814250.png" alt="None0Demand" style="zoom:50%;" /><img src="/assets/image-20200210165930907.png" alt="Describe" style="zoom:50%;" />

Here we see:

1. there are only 3095 none zero Demand compare to 14430 in total.
2. All the Supply  are zero while Demand are none zero

### 3.2 Visualize the None Zero Part

``` python
column = "Demand (MW)"
legend_kwds={'label': "None Zero Demand",'orientation': "vertical"}
Attribute_mapping(None_Zero_Demand, column, legend_kwds)
```

<img src="/assets/image-20200210170733776.png" alt="nonezerodemand" style="zoom:67%;" />

Though the points become less, most of them are near zero (purple color). So, we can try to limit the value to visualize more clear.

### 3.3 Separate into Three Parts by ordered values

Let's first order the DataFrame by `Demand`

``` python
None_Zero_Demand = None_Zero_Demand.sort_values(by = 'Demand (MW)')
None_Zero_Demand.head()
```

<img src="/assets/image-20200210171813748.png" alt="order" style="zoom:67%;" />

Then split it into 3 parts

``` python
Split3 = np.array_split(None_Zero_Demand, 3)
Part1, Part2, Part3 = Split3[0], Split3[1], Split3[2]
Part1.head()
Part2.head()
Part3.head()
#Note: here the other two columns not presented in the figures
```

<img src="/assets/image-20200210172559313.png" alt="P1head" style="zoom:30%;" /><img src="/assets/image-20200210172638632.png" alt="P2head" style="zoom:30%;" /><img src="/assets/image-20200210172724404.png" alt="P3head" style="zoom:30%;" />

Finally, visualize each part

``` python
column = "Demand (MW)"
#Part1
legend_kwds={'label': "None Zero Demand",'orientation': "vertical"}
Attribute_mapping(Part1, column, legend_kwds)
#Part2
legend_kwds={'label': "None Zero Demand",'orientation': "vertical"}
Attribute_mapping(Part2, column, legend_kwds)
#Part3
legend_kwds={'label': "None Zero Demand",'orientation': "vertical"}
Attribute_mapping(Part3, column, legend_kwds)
```

<img src="/assets/image-20200210173434367.png" alt="P1V" style="zoom:30%;" /><img src="/assets/image-20200210173507904.png" alt="P2v" style="zoom:30%;" /><img src="/assets/image-20200210173536841.png" alt="P3v" style="zoom:30%;" />

Here we:

- [x] ​	Visualize three parts separately

- [ ] ​    Unify the color bars in above three figures.



