
# R Notebooks for the CoastWatch Satellite Training Course  

These files are the R Notebooks for the CoastWatch Satellite Course R Tutorials 

## Exercise book contents  
The Notebooks contain step-by-step code examples for extracting data from the ERDDAP data servers using the rxtracto R library.  
* [Extract data within a boundary](extract_data_marine_sanctuary.md)   
In this exercise, you will download data from within the boundaries of the Monterey Bay National Marine Sanctuary (MBNMS) and visualize the data in a map.  

* [Matchups satellite data to an animal tracks](matchup_satellite_track_data.md)  
This exercise you will extract satellite data around a set of points defined by longitude, latitude, and time coordinates like that produced by an animal telemetry tag, a ship track, or a glider tract.  

* [Create timeseries from satellite data](timeseries_satellite_data.md)  
This excercise extracts a time series of monthly satellite chlorophyll data for the period of 1997-present from four different monthly satellite datasets.  

* [Matchup satellite to buoy data](matchup_satellite_buoy_data.md)  
In this exercise you will extract buoy data from ERDDAP tabular data and then extract satellite data that is coincident with the buoy data.  

* [Define a marine habitat](define_marine_habitat.md)  
In this exercise you will plot the thermal boundary of loggerhead sea turtles using satellite sea surface temperature.  

* [Reprojecting satellite and buoy data](reprojecting_satellite_buoy_data.md)  
In exercise, we will obtain cruise track data from a research ship, in-situ buoy data, and satellite sea surface temperature data from the PolarWatch ERDDAP data server and then plot all three types of the data on a map in an Alaska Albers projection.  

* [Mapping projected datasets](mapping_projected_datasets.md)  
This excercise demonstrates techniques for working with sea ice data served in polar stereographic map projection.


## Downloading 
Download the enitre r_code repository. Subfolders contain images and CSV files used in the Notebook code. 

## NOAA CoastWatch/OceanWatch/PolarWatch Program  

NOAA CoastWatch/OceanWatch provides easy access for everyone to global and regional satellite data products for use in understanding, managing and protecting ocean and coastal resources and for assessing impacts of environmental change in ecosystems, weather, and climate.  

![](images/cw_logo_80.png)  <span style="color:blue;font-size:30px;">[CoastWatch](https://coastwatch.noaa.gov/)</span>


## CoastWatch West Coast Regional Node  

These course materials were developed and are distributed by the [West Coast Node](https://coastwatch.pfeg.noaa.gov/) of NOAA CoastWatch. Information about the courses and services offered by West Coast Node can be found at the following links:  

| [Courses](https://coastwatch.pfeg.noaa.gov/courses/satellite_course.html) | [Data Catalog](https://coastwatch.pfeg.noaa.gov/data.html) | [Data Server](https://coastwatch.pfeg.noaa.gov/erddapinfo/index.html) |  [Xtractomatic](https://coastwatch.pfeg.noaa.gov/xtracto/) |
