# Matchup satellite and buoy data

> notebook filename \| matchup\_satellite\_buoy\_data.Rmd  
> history \| Created May 2021 - converted to R notebook from original
> file SSTandBuoy.R

In this exercise you will extract buoy data from ERDDAP tabular data and
then extract satellite data that is coincident with the buoy data.

The exercise demonstrates the following techniques:

-   Use the **tabledap** function to extract tabular data from ERDDAP  
-   Using **xtracto** to extract satellite data coincident with the buoy
    data  
-   Using **rxtracto\_3D** to extract satellite data for a rectangular
    area  
-   Using **rerddap** to retrieve information about a dataset from
    ERDDAP  
-   Producing xy scatter plots  
-   Generating linear regressions  
-   Producing satellite maps and overlaying buoy data

## Install required packages and load libraries

``` r
# Function to check if pkgs are installed, install missing pkgs, and load
pkgTest <- function(x)
{
  if (!require(x,character.only = TRUE))
  {
    install.packages(x,dep=TRUE,repos='http://cran.us.r-project.org')
    if(!require(x,character.only = TRUE)) stop(x, " :Package not found")
  }
}

pkgTest <- function(x)
{
  if (!require(x,character.only = TRUE))
  {
    install.packages(x,dep=TRUE)
    if(!require(x,character.only = TRUE)) stop("Package not found")
  }
}

list.of.packages <- c( "ncdf4", "rerddap", "plotdap", "httr",
                       "lubridate", "gridGraphics",  "mapdata",
                       "ggplot2", "RColorBrewer", "grid", "PBSmapping", 
                       "rerddapXtracto")

# create list of installed packages
pkges = installed.packages()[,"Package"]

for (pk in list.of.packages) {
  pkgTest(pk)
}
```

## Extract buoy data from ERDDAP

**Extract data using the tabledap function**

-   For every day in August 2018  
-   In the region bounded by 30.86 to 41.75 north latitude and -128 to
    -116 east longitude  
-   request the station, latitude, longitude, time, and water
    temperature parameters  
-   put the data into a data frame

``` r
buoy <- tabledap(
  'cwwcNDBCMet', 
  fields=c('station', 'latitude',  'longitude', 'time', 'wtmp'), 
  'time>=2018-08-01',   'time<=2018-08-13', 
  'latitude>=30.86','latitude<=41.75', 'longitude>=-128','longitude<=-116',
  'wtmp>0'
)

#Create dataframe 
buoy.df <-data.frame(station=buoy$station,
                 longitude=as.numeric(buoy$longitude),
                 latitude=as.numeric(buoy$latitude),
                 time=strptime(buoy$time, "%Y-%m-%dT%H:%M:%S"),
                 sst=as.numeric(buoy$wtmp))
```

**Isolate the unique stations**

``` r
# Find out how many stations were retrieved
unique.sta <- unique(buoy$sta)
n.sta <- length(unique.sta)
```

**Select buoy data closest in time to satellite data**

-   Buoy data is hourly, but there is only one satellite value per
    day.  
-   So subset buoy data to that taken as 22h00 GMT (2pm local + 8),
    which corresponds to when the satellite passes overhead.

``` r
dailybuoy <- subset(buoy.df,hour(time)==22)
```

## Extract SST satellite data for matchups to buoy data.

**Examine the metadata for the VIIRS monthly dataset (ID =
erdVHsstaWS3day)**  
**The script below:**

-   Gathers metadata by using the **rerddap::info** function
-   Display the information

``` r
# Select VIIRS dataset and get information about the dataset 

dataInfo <- rerddap::info('erdVHsstaWS3day')
dataInfo
```

    ## <ERDDAP info> erdVHsstaWS3day 
    ##  Base URL: https://upwell.pfeg.noaa.gov/erddap 
    ##  Dataset Type: griddap 
    ##  Dimensions (range):  
    ##      time: (2014-11-02T12:00:00Z, 2022-01-03T12:00:00Z) 
    ##      altitude: (0.0, 0.0) 
    ##      latitude: (30.860437023511412, 41.753893186176605) 
    ##      longitude: (-128.2508393601582, -114.7491606398418) 
    ##  Variables:  
    ##      nobs: 
    ##      sst: 
    ##          Units: degree_C

**Extact the matchup data using rxtracto**

-   Use the variable name in dataInfo for the parameter argument
-   Use the longitude, latitude, and time coordinates from dailybuoy to
    set xcoord, ycoord, and tcoord
-   This dataset has a altitude dimension, so add a vector of zeros, one
    for each matchup
-   Run rxtracto

``` r
parameter <- 'sst'
xcoord <- dailybuoy$longitude
ycoord <- dailybuoy$latitude
tcoord <- dailybuoy$time
zcoord <- ycoord*0


extract <- rxtracto(dataInfo, parameter=parameter, 
                    tcoord=tcoord,
                    xcoord=xcoord,ycoord=ycoord,zcoord=zcoord,
                    xlen=.01,ylen=.01)
                     
dailybuoy$viirs<-extract$`mean sst`
```

**Get subset of data where a satellite value was found**

-   Not all matchup will yield data, for example due to cloud cover.

``` r
# Get subset of data where there is a satellite value 
goodbuoy<-subset(dailybuoy, viirs > 0)
unique.sta<-unique(goodbuoy$sta)
nbuoy<-length(unique.sta)
ndata<-length(goodbuoy$sta)
```

## Compare results for satellite and buoy

-   Plot the VIIRS satellite verses the buoy data to visualize how well
    the two datasets track each other.

``` r
# Set up map title 
main="California coast, 8/1/18-8/13/18"   

p <- ggplot(goodbuoy, aes(sst, viirs,color=latitude)) + 
     coord_fixed(xlim=c(8,25),ylim=c(8,25)) 
p + geom_point() + 
  ylab('VIIRS SST')  + 
  xlab('NDBC Buoy SST @ 2pm') +
  scale_x_continuous(minor_breaks = seq(8, 25)) + 
  scale_y_continuous(minor_breaks = seq(8, 25)) + 
  #geom_abline(a=fit[1],b=fit[2]) +
  #annotation_custom(my_grob) + 
  scale_color_gradientn(colours = rev(rainbow(9)), name="Buoy\nLatitude") +
  labs(title=main) + theme(plot.title = element_text(size=25, face="bold", vjust=2)) 
```

![](matchup_satellite_buoy_data_files/figure-gfm/xyplot-1.png)<!-- -->

Run a linear regression of VIIRS satellite verses the buoy data. \* The
R squared in close to 1 (0.9807) \* The slope is near 1 (0.950711)

``` r
lmHeight = lm(viirs~sst, data = goodbuoy)
summary(lmHeight)
```

    ## 
    ## Call:
    ## lm(formula = viirs ~ sst, data = goodbuoy)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.9591 -0.4528  0.0073  0.4375  4.0311 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.96058    0.07524   12.77   <2e-16 ***
    ## sst          0.94728    0.00392  241.65   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.751 on 1391 degrees of freedom
    ## Multiple R-squared:  0.9767, Adjusted R-squared:  0.9767 
    ## F-statistic: 5.84e+04 on 1 and 1391 DF,  p-value: < 2.2e-16

## Create a satellite map and overlay buoy data

**Extract VIIRS chlorophyll data for the month of August 2018**

``` r
# First define the box and time limits of the requested data 
ylim<-c(30.87,41.75)
xlim<-c(-128,-115)

# Extract the monthly satellite data
SST <- rxtracto_3D(dataInfo,xcoord=xlim,ycoord=ylim,parameter=parameter, 
                   tcoord=c('2018-08-06','2018-08-06'),zcoord=zcoord)

SST$sst <- drop(SST$sst)
```

**Create the map frame for the satellite data and buoy SST overlay**

``` r
mapFrame<- function(longitude,latitude,sst){
  dims<-dim(sst)
  sst<-array(sst,dims[1]*dims[2])
  sstFrame<-expand.grid(x=longitude,y=latitude)
  sstFrame$sst<-sst
  return(sstFrame)
}

sstFrame<-mapFrame(SST$longitude,SST$latitude,SST$sst)
coast <- map_data("worldHires", ylim = ylim, xlim = xlim)
my.col <- colorRampPalette(rev(brewer.pal(11, "RdYlBu")))(22-13) 

buoy2<-subset(dailybuoy, month(time)==8 &day(time)==5 & sst > 0)
```

**Create the map**

``` r
myplot<-ggplot(data = sstFrame, aes(x = x, y = y, fill = sst)) +
  geom_tile(na.rm=T) +
  geom_polygon(data = coast, aes(x=long, y = lat, group = group), fill = "grey80") +
  theme_bw(base_size = 15) + ylab("Latitude") + xlab("Longitude") +
  coord_fixed(1.3,xlim = xlim, ylim = ylim) +
  scale_fill_gradientn(colours = rev(rainbow(12)),limits=c(10,25),na.value = NA) +
  ggtitle(paste("VIIRS and NDBC buoy SST \n", unique(as.Date(SST$time)))) +
  geom_point(data=buoy2, aes(x=longitude,y=latitude,color=sst),size=3,shape=21,color="black") + 
  scale_color_gradientn(colours = rev(rainbow(12)),limits=c(10,25),na.value ="grey20", guide=FALSE) 
  
myplot
```

    ## Warning: It is deprecated to specify `guide = FALSE` to remove a guide. Please
    ## use `guide = "none"` instead.

![](matchup_satellite_buoy_data_files/figure-gfm/map-1.png)<!-- -->
