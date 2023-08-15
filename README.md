# R Code Repository

This repository includes R codes in Rmd (R-markdown) and md (markdown) file format, and all other supporting files to demonstrate access and usage of geospatial data (satellite, non satellite data).

## Repository Structure
Files are organized in folders by topics.  Each folder includes Rmd file and all associated files or sub-folders.

For example, in a folder titled, "accessing-satellite-data", you may find the following contents: 

```
accessing-satellite-data/
               data/
               basemaps/
               images/
               accessing-satellite-data.Rmd
               accessing-satellite-data.md
```
### File types
**.Rmd** : R-markdown files are R files and you can download to open in RStudio to view, edit and run the codes.

**.md**  : Markdown files are not R files and **will not** work in RStudio.  The main objective of md files are for users to preview the R codes and the outputs on Github. 

## Content Description
Each R file contains step-by-step code examples for various tasks required to work with satellite or non-satellite geospatial data including getting data from ERDDAP data servers to using data.

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

* [Accessing projected datasets](accessing_projected_datasets.md)  
This excercise demonstrates techniques for working with sea ice data served in polar stereographic map projection.

## NOAA CoastWatch/OceanWatch/PolarWatch Program  

NOAA CoastWatch/OceanWatch provides easy access for everyone to global and regional satellite data products for use in understanding, managing and protecting ocean and coastal resources and for assessing impacts of environmental change in ecosystems, weather, and climate.  

## CoastWatch West Coast Regional Node  

These course materials were developed and are distributed by the [NOAA CoastWatch Training Team??](https://coastwatch.pfeg.noaa.gov/) of NOAA CoastWatch. Information about the courses and services offered  can be found at the following links:  

| [Courses](https://coastwatch.pfeg.noaa.gov/courses/satellite_course.html) | [Data Catalog](https://coastwatch.pfeg.noaa.gov/data.html) | [Data Server](https://coastwatch.pfeg.noaa.gov/erddapinfo/index.html) |  [Xtractomatic](https://coastwatch.pfeg.noaa.gov/xtracto/) |
