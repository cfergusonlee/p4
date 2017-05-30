# Exploratory Data Analysis of Uber Pickups

**By: [Courtney Ferguson Lee](https://www.linkedin.com/in/courtneyfergusonlee/)**

**Date: May 9, 2017**

# Introduction

I chose to investigate an Uber dataset of New York City pickups from January
through June of 2015. The Taxi & Limousine Commission (TLC) released the data
after receiving a Freedom of Information Law (FOIL) request from FiveThirtyEight 
on June 22, 2015. I pulled the data from the FiveThirtyEight github repo found
[here](https://github.com/fivethirtyeight/uber-tlc-foil-response).  Each 
observation represents a single pickup with the following columns:

Variable | Definition
------------------- | --------------------------------------------------
Dispatching_base_num | TLC base company code that dispatched the Uber
Pickup_date | Full date of the pickup in yyyy-mm-dd h:m:s format
Affiliated_base_num | TLC base company code of the Uber pickup
locationID | Pickup location ID of the Uber pickup
Borough | New York City Borough where the pickup took place
Zone | Neighborhood in the New York City Borough where the pickup took place
lat | Latitude of the pickup Zone
lon | Longitude of the pickup Zone
month | Month that the pickup took place
date | Date (1-31) that the pickup took place
day | Day (Monday-Sunday) that the pickup took place
hour | Hour that the pickup took place

I was most interested in how ridership varied over time and location.  I 
wanted to know how it changed by hour, week, day and month.  I also wanted to
know what the most popular Boroughs and Zones were.

```r
# Read csv's and load taxi lookup data
setwd('/Users/courtneyfergusonlee/p4')
uber <- read.csv('data/uber-raw-data-janjune-15.csv')
taxi_lookup <- read.csv('data/taxi-zone-lookup.csv', stringsAsFactors = F)

# Load from RData file until ready to knit
# load('data/uber.RData', .GlobalEnv)

# Edit Zones for more accurate location results
taxi_lookup[17, 3] <- 'Bedford-Stuyvesant'
taxi_lookup[23, 3] <- 'Bloomfield'
taxi_lookup[81, 3] <- 'Eastchester, Bronx'

# Get latitude and longitude info for location data (Long Process)
citystate = 'new york city new york'
taxi_lookup <- mutate(taxi_lookup, 
       lat = geocode(paste(Zone, citystate, sep = ', '), output = 'latlon')$lat,
       lon = geocode(paste(Zone, citystate, sep = ', '), output = 'latlon')$lon)

# Check for missing latitude or longitude data
taxi_lookup.lat_na <- subset(taxi_lookup, is.na(lat))
taxi_lookup.lon_na <- subset(taxi_lookup, is.na(lon))

# If there's missing data, need to rename variables and replace values as nec.
names(taxi_lookup.lat_na) <- c("LocationID", 
                           "Borough",    
                           "Zone",       
                           "latitude",
                           "longitude")
names(taxi_lookup.lon_na) <- c("LocationID", 
                           "Borough",    
                           "Zone",       
                           "latitude",
                           "longitude")

# Lookup missing values in bulk, check for warnings
taxi_lookup.lat_na <- mutate(taxi_lookup.lat_na, 
       latitude = geocode(paste(Zone, citystate, sep = ', '), 
                          output = 'latlon')$lat)
taxi_lookup.lon_na <- mutate(taxi_lookup.lon_na, 
       longitude = geocode(paste(Zone, citystate, sep = ', '), 
                          output = 'latlon')$lon)

# Replace missing values CAREFULLY (Lookup is a long process)
taxi_lookup[taxi_lookup.lat_na$LocationID, 4] <- taxi_lookup.lat_na$latitude
taxi_lookup[taxi_lookup.lon_na$LocationID, 5] <- taxi_lookup.lon_na$longitude

```


